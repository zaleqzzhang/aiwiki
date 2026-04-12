---
title: "同步阻塞 vs 异步编排：Hermes  Delegate 与 OpenClaw 多 Agent 机制深度实战对比"
source: "https://x.com/servasyy_ai/status/2042951017462169812"
author:
  - "[[@servasyy_ai]]"
published: 2026-04-11
created: 2026-04-12
description: "在现代 AI Agent 系统的演进中，如何让多个 agent 高效协作已成为核心命题。本文深入剖析两种截然不同的设计哲学：Hermes 的 delegate_task 机制与 OpenClaw 的 subagent 系统，揭示它们在架构、通信模式和适用场景上的本质差异。两种设计..."
tags:
  - "clippings"
---
![Image](https://pbs.twimg.com/media/HFoEDQPakAEZx_U?format=jpg&name=large)

在现代 AI Agent 系统的演进中，如何让多个 agent 高效协作已成为核心命题。本文深入剖析两种截然不同的设计哲学：Hermes 的 delegate\_task 机制与 OpenClaw 的 subagent 系统，揭示它们在架构、通信模式和适用场景上的本质差异。

## 两种设计哲学的碰撞

Hermes 的 delegate\_task 采用“总承包商-分包商”模式。父 agent 像项目经理一样，将任务拆解后分发给子 agent，然后阻塞等待所有结果返回。

这种设计追求极致的隔离性和 token 效率：子 agent 的所有中间推理过程都不会污染父 agent 的上下文窗口，最终只返回一个精炼的 summary。

![Image](https://pbs.twimg.com/media/HFoCAmibIAAuSKD?format=jpg&name=large)

OpenClaw 的 subagent 系统则更像交响乐团的编排。

父 agent 定义全局拓扑后，各个 agent 作为固定角色异步运行，通过事件推送机制相互协作。父 agent 甚至可以在子 agent 运行期间发送“引导消息”调整方向，这种灵活性是 Hermes 所不具备

![Image](https://pbs.twimg.com/media/HFoCJaOa0AAny52?format=jpg&name=large)

## Hermes delegate：精简高效的并行执行器

Hermes 的实现集中在 978 行的 delegate\_tool.py 中。它支持两种工作模式：单任务模式提供一个 goal 参数启动单个子 agent 同步执行；批量模式则接受最多 3 个任务的数组，通过 ThreadPoolExecutor 并行处理。

**子 Agent 的构造与隔离**

每次委派都会创建全新的 AIAgent 实例。这些子 agent 拥有独立的对话历史、独立的 Terminal session（每个 task\_id 对应独立的工作目录和状态），以及专门构建的 System Prompt。子 agent 的配置遵循严格的继承规则：模型可以继承父 agent 或使用 delegation.model 配置，工具集通过交集计算（子不能获得父没有的工具），凭证可以共享父的 credential pool 或独立配置。

为了防止失控，Hermes 对子 agent 进行了严格的工具剥夺：delegate\_task 本身被禁用以防止递归委派，clarify 被移除使其无法与用户交互，memory 工具被剥夺以保护共享记忆，甚至 execute\_code 也被禁用以避免子 agent 编写复杂脚本，send\_message 也被移除以防止跨平台消息发送。

子 agent 的 System Prompt 结构简洁明确：“You are a focused subagent working on a specific delegated task”，包含任务目标、可选的上下文信息、以及从父 agent 解析出的工作目录路径。特别强调的是工作空间规则：“Never assume a repository lives at /workspace/……”，避免路径假设导致的错误。

**生命周期与深度控制**

Hermes 实施了严格的深度限制：MAX\_DEPTH = 2，意味着父 agent（深度 0）可以 spawn 子 agent（深度 1），但子 agent 不能再 spawn 孙子（深度 2 会被拒绝）。每个子 agent 在构造时会递增深度标记 \_delegate\_depth，在调用前检查是否超限。

子 agent 的迭代上限默认为 50 轮（DEFAULT\_MAX\_ITERATIONS，可通过 config.yaml 配置），但关键的是**没有时间维度的超时控制：**future.result() 调用不带 timeout 参数，这意味着子 agent 理论上可以无限期运行直到耗尽迭代次数。

**Token 效率与通信模式**

![Image](https://pbs.twimg.com/media/HFoCgZ6akAAS0rb?format=jpg&name=large)

Hermes 采用纯粹的单向返回（Request-Response）通信模式。父 agent 构造 goal 和 context 后调用 delegate\_task()，然后阻塞等待。子 agent 之间完全无通信，所有中间 tool 调用和推理链都不进入父的上下文窗口。

唯一的“进度通知”是 UI 层面的 callback（在 CLI 的 spinner 上方打印工具调用，或在 Gateway 中批量中继工具名称），但这不是语义通信，只是进度显示。

**异常终止的级联机制**

![Image](https://pbs.twimg.com/media/HFoCqcBaIAACVm4?format=jpg&name=large)

子 agent 的凭证解析遵循三级优先级：

首先检查 delegation.base\_url 配置，如果存在则使用该端点和 api\_key；

其次检查 delegation.provider 配置，通过 runtime provider 系统解析完整凭证；如果都没配置，子 agent 继承父 agent 的全部凭证。当子 agent 与父 agent 使用同一 provider 时，它们共享同一个 credential pool（cooldown 状态同步）；否则加载该 provider 独立的 pool。每个子 agent 获取凭证 lease 后，在 finally 块中自动释放。

然而，Hermes 的设计也存在**明显短板**。最致命的是缺少超时机制：future.result() 调用没有 timeout 参数，子 agent 只受迭代次数上限（默认 50 轮）约束。如果子 agent 每轮都调用慢速 LLM，50 轮可能跑数小时。

此外，并发上限硬编码为 3，嵌套深度限制为 1 层（子 agent 不能再 spawn 孙子），这些都限制了其在复杂场景下的扩展性。

## OpenClaw Subagent：异步协作的编排系统

OpenClaw 构建了完整的层级化 agent 架构。它定义了三种角色：main（主 agent）、orchestrator（编排器，可继续 spawn 子 agent）和 leaf（叶节点，不可再 spawn）。默认配置下，全局最多支持 8 路并发，每个 agent 最多管理 5 个活跃子 agent，嵌套深度可配置。

OpenClaw 的核心创新在于异步事件驱动的通信模式。父 agent spawn 子 agent 后立即返回，继续处理其他任务。子 agent 完成后，通过 runSubagentAnnounceFlow() 将结果以“user message”形式推送回父 agent。这种推送式设计避免了轮询开销，文档明确声明：“不要调用 sessions\_list、sessions\_history 或任何轮询工具，等待完成事件自动到达。”

更强大的是 Steer 机制。父 agent 可以向运行中的子 agent 发送引导消息，触发其重新调整方向。这种能力有限流保护（每 2 秒最多一次），消息最大 4000 字符。这正是 Hermes 最缺失的能力——当用户在 Gateway 中发送新消息时，Hermes 只能暴力 interrupt 所有子 agent，而 OpenClaw 可以判断用户意图，选择性地引导或查询进度。

OpenClaw 还提供了完整的生命周期管理。runTimeoutSeconds 参数（默认 300 秒）提供时间维度的控制，runs.json 持久化运行记录支持 orphan recovery，resumeSessionId 允许恢复已有会话。这些机制使其适合长流程、多阶段的复杂任务。

但灵活性是有代价的。OpenClaw 的 announce 推送会将子 agent 的结果注入父 agent 的上下文，频繁的 steer 消息也会产生额外 token 开销。假设同样的 3 个子任务场景，每次 announce 约 1K token，父 agent 的上下文会从 10K 增长到 13K，总消耗约 28K token：比 Hermes 多 12%。

## 架构差异的深层逻辑

**两者最大的分水岭在于同步与异步。**

**Hermes** 的同步阻塞意味着父 agent 在子运行期间完全停滞，无法响应新需求；

**OpenClaw** 的异步推送则允许父 agent 同时管理多个子 agent，动态调整策略。

从信息论视角看，Hermes 的子 agent 是有损压缩器：将大量中间 tool 调用和推理链压缩为一个 summary 字符串。这种设计在“单点决策 + 批量执行”场景下极其高效：父 agent 做规划，子 agent 只负责执行和返回，上下文零膨胀。

OpenClaw 则更像分布式系统，需要更复杂的状态管理。它的 Steer 能力解决了 Hermes 的最大痛点：用户发送 ？查询进度时，Hermes 会误杀所有子 agent，而 OpenClaw 可以返回状态后让子 agent 继续运行。

从安全视角看，Hermes 的强隔离形成天然沙箱，子 agent 被剥夺所有“向外逃逸”的工具，适合安全敏感场景。OpenClaw 的子 agent 保留更多能力，但也意味着更大的攻击面。

![Image](https://pbs.twimg.com/media/HFoDHDUbsAAwfTp?format=jpg&name=large)

## 如何选择？场景决定架构

如果你需要并行处理 3 件互不相关的事情，且不希望中间数据污染主对话，Hermes 是最佳选择。它的天然并行、隔离干净、一个 tool call 搞定的特性，配合极致的 token 效率，在快速集成场景下无可替代。

如果你的任务需要在子 agent 运行中调整方向，或者需要超时控制防止无限运行，OpenClaw 的 Steer 机制和 runTimeoutSeconds 配置是刚需。当你需要调用外部编码 agent（如 Claude Code、Codex）作为子 agent，或者构建多层嵌套的 agent 树时，OpenClaw 的 ACP 支持和 orchestrator 角色是唯一选项。

对于超过 3 个 agent 并行工作的场景，OpenClaw 的 8 路并发 + 每 agent 5 子的配置明显更强。而对于长流程任务，OpenClaw 的持久化和恢复机制（runs.json + orphan recovery）可以避免重复工作。

## 混合架构：取长补短的实践路径

两者并非互斥。最佳实践是根据任务特性选择合适模式：在 OpenClaw 的异步编排层处理复杂工作流和多 agent 协作，当遇到需要隔离并行的子任务时，嵌入 Hermes 风格的 delegate 调用。

例如，main agent 通过 OpenClaw 的 sessions\_spawn 启动 research agent（ACP 调用 Claude Code）、analysis agent（embedded）和 writer agent。其中 analysis agent 内部使用 Hermes 的 delegate\_task 并行处理 3 个互不相关的数据分析任务，利用其上下文零膨胀的优势；而 research 和 writer agent 之间通过 OpenClaw 的 announce 和 steer 机制协作，保持灵活性。

![Image](https://pbs.twimg.com/media/HFoDU4fbYAAGhg-?format=jpg&name=large)

## 改进方向：Hermes 的进化空间

基于对比分析，Hermes 有几个明确的改进方向。最高优先级是加入超时机制：为 delegate\_task 增加 timeout\_seconds 参数，防止子 agent 无限运行。同样重要的是改进 Gateway 的 interrupt 逻辑：当用户发送 ？、进度 等短消息时，返回进度而非杀死所有 agent。

中等优先级的改进包括引入 Steer 能力，允许父 agent 向运行中的子 agent 发引导消息；放开并发上限，将 MAX\_CONCURRENT\_CHILDREN 改为可配置；以及支持持久化和恢复，保存子 agent 状态以便被 interrupt 后恢复。

长期来看，支持外部 Agent（通过 ACP 启动 Claude Code / Codex 作为子 agent）将显著扩展 Hermes 的能力边界。

## 结语

Hermes 和 OpenClaw 代表了多 Agent 协作的两种极端：一个追求极致的隔离和效率，一个追求最大的灵活性和控制力。理解它们的设计权衡，才能在实际项目中做出正确的架构选择。未来的 Agent 系统，很可能是这两种哲学的融合——在需要隔离时强隔离，在需要协作时强协作，让架构服务于任务本质。