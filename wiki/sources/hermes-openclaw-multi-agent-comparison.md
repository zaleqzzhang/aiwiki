# 同步阻塞 vs 异步编排：Hermes Delegate 与 OpenClaw 多 Agent 机制对比

**类型**: article
**作者**: [[entities/servasyy-ai|servasyy_ai]]
**日期**: 2026-04-11
**来源**: https://x.com/servasyy_ai/status/2042951017462169812
**标签**: #multi-agent #architecture #hermes #openclaw

## 摘要

本文深入对比两种多 Agent 协作设计哲学：Hermes 的 `delegate_task` 机制（同步阻塞、"总承包商-分包商"模式）与 OpenClaw 的 subagent 系统（异步编排、"交响乐团"模式）。两者在架构、通信模式、隔离性、灵活性上存在本质差异，适用于不同场景。

## 关键要点

### Hermes delegate：同步阻塞 + 强隔离

**设计哲学**："总承包商-分包商"模式，追求极致隔离性和 token 效率。

**核心特征**：
- 同步阻塞：父 agent 等待所有子 agent 完成后返回
- 强隔离：子 agent 的中间推理不污染父 agent 上下文，只返回 summary
- 并行执行：支持最多 3 个任务并行（ThreadPoolExecutor）
- 深度限制：MAX_DEPTH = 2（父 → 子，不可再嵌套）

**子 Agent 构造**：
- 独立的 AIAgent 实例、对话历史、Terminal session
- 工具剥夺：禁用 delegate_task（防递归）、clarify（禁交互）、memory（保护共享记忆）、execute_code、send_message
- 配置继承：模型可继承或独立配置，工具集取交集，凭证可共享或独立

**生命周期控制**：
- 迭代上限：默认 50 轮
- **无超时机制**：future.result() 无 timeout，子 agent 可无限期运行
- 凭证管理：三级优先级解析，自动释放 lease

**通信模式**：纯粹单向返回（Request-Response），无语义通信，只有 UI 层面的进度显示。

**优势**：
- Token 效率极高：上下文零膨胀
- 天然沙箱：强隔离适合安全敏感场景
- 实现简洁：单工具调用完成委派

**劣势**：
- 无超时：可能无限运行
- 无 Steer：无法向运行中的子 agent 发送引导
- 并发上限硬编码为 3
- 用户查询进度会误杀所有子 agent（Gateway interrupt）

### OpenClaw Subagent：异步编排 + 灵活协作

**设计哲学**："交响乐团"模式，父 agent 定义全局拓扑，各 agent 异步协作。

**角色体系**：
- main：主 agent
- orchestrator：编排器，可继续 spawn 子 agent
- leaf：叶节点，不可再 spawn

**并发配置**：全局最多 8 路并发，每 agent 最多管理 5 个活跃子 agent。

**核心创新**：
1. **异步事件驱动**：spawn 后立即返回，结果通过 announce 推送（"user message"形式）
2. **Steer 机制**：向运行中的子 agent 发送引导消息（限流 2 秒/次，最大 4000 字符）
3. **生命周期管理**：
   - runTimeoutSeconds：时间维度超时（默认 300 秒）
   - runs.json：持久化运行记录，支持 orphan recovery
   - resumeSessionId：恢复已有会话

**通信模式**：事件推送 + Steer 引导，避免轮询开销。

**优势**：
- 灵活性：可动态调整方向
- 超时控制：时间 + 迭代双维度
- 用户查询不误杀：可返回进度后继续运行
- 支持外部 Agent：通过 ACP 调用 Claude Code / Codex

**劣势**：
- Token 开销更大：announce 推送注入上下文，steer 消息产生额外开销
- 状态管理更复杂
- 攻击面更大：子 agent 保留更多能力

### 架构差异对比

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 同步/异步 | 同步阻塞 | 异步事件驱动 |
| 通信模式 | 单向返回 | 推送 + Steer |
| 隔离性 | 强隔离（沙箱） | 较弱（保留更多能力） |
| Token 效率 | 极高 | 较低（多 12%+） |
| 并发上限 | 3（硬编码） | 8（可配置） |
| 嵌套深度 | 1 层 | 可配置 |
| 超时控制 | ❌ 无 | ✅ runTimeoutSeconds |
| Steer 能力 | ❌ 无 | ✅ 支持 |
| 持久化恢复 | ❌ 无 | ✅ runs.json + orphan recovery |
| 外部 Agent | ❌ 不支持 | ✅ ACP 协议 |

### 适用场景

**选择 Hermes**：
- 并行处理 3 个互不相关的任务
- 需要极致 token 效率
- 需要强隔离（安全敏感）
- 快速集成、简单决策-执行流程

**选择 OpenClaw**：
- 需要在子 agent 运行中调整方向
- 需要超时控制
- 调用外部编码 agent（Claude Code / Codex）
- 多层嵌套 agent 树
- 超过 3 个 agent 并行
- 长流程任务（需要持久化和恢复）

### 混合架构实践

两者可结合使用：
```
OpenClaw 异步编排层（处理复杂工作流）
    ├── research agent（ACP 调用 Claude Code）
    ├── analysis agent（内部使用 Hermes delegate_task 并行处理 3 个任务）
    └── writer agent（与 research 通过 announce/steer 协作）
```

### Hermes 改进方向

**高优先级**：
1. 加入超时机制：timeout_seconds 参数
2. 改进 Gateway interrupt 逻辑：区分查询进度 vs 真正中断

**中优先级**：
1. 引入 Steer 能力
2. 放开并发上限（可配置）
3. 支持持久化和恢复

**长期**：
1. 支持外部 Agent（ACP 协议）

## 关联实体

- [[entities/hermes|Hermes]] - 同步阻塞式多 Agent 框架
- [[entities/openclaw|OpenClaw]] - 异步编排式多 Agent 框架
- [[entities/delegate-task|delegate_task]] - Hermes 的任务委派机制
- [[entities/steer-mechanism|Steer 机制]] - OpenClaw 的运行时引导能力
- [[entities/acp-protocol|ACP 协议]] - Agent Communication Protocol，支持外部 Agent 调用

## 引用此来源的页面

*待其他页面创建后补充*

## 相关主题

- [[topics/multi-agent-collaboration|多 Agent 协作]]
- [[topics/agent-architecture|Agent 架构]]
