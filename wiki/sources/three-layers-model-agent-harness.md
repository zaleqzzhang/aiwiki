# Model, Agent, Harness: The Three Layers Most People Collapse Into One

**类型**: article
**日期**: 2026-03-22
**来源**: https://contextua.dev/model-agent-harness-the-three-layers-most-people-collapse-into-one/
**作者**: Tom Foster
**标签**: #three-layer-architecture #harness #agent #model #composability

## 摘要

精确定义 AI Agent 系统的三层架构——Model / Agent / Harness——以及为什么大多数人把三层混为一谈。文章论证：Model 是统计引擎（预测下一个 token），Agent 是推理循环（模型 + 目标导向行为），Harness 是执行平台（给 Agent 一个"身体"）。核心论点：**大部分 Agent 失败是 Layer 3（Harness）问题，不是 Layer 1（模型）问题。**

## 关键要点

### 1. 三层精确定义

| 层 | 是什么 | 做什么 | 类比 |
|----|-------|--------|------|
| Layer 1: Model | 原始语言模型（Claude Opus 4.6, GPT-5.3） | 预测序列中的下一个 token | 引擎 |
| Layer 2: Agent | 模型 + 推理循环 | 观察→规划→选工具→行动→评估→决定下一步 | 驾驶员 |
| Layer 3: Harness | 执行平台（Claude Code, Codex CLI） | 文件系统访问、终端执行、权限处理、会话持久化、上下文组装、安全护栏 | 整辆车 |

**关键区分**：
- Model 没有记忆、看不到文件系统、不知道时间、不能执行命令——它只做 text completion
- Agent 层是"智能遇见意图"的地方——模型提供认知马力，Agent 层决定往哪用
- Harness 不是纯被动基础设施，而是**编排者**：决定 Agent 接收什么上下文、能调用什么工具、循环何时停止

### 2. 具象案例：Claude Code

- API 同时交付 Layer 1 和 Layer 2（模型权重 + 推理循环内置在 API tool-use 接口中）
- Claude Code 清晰地坐在 Layer 3：文件系统访问、终端执行、权限提示、会话持久化、上下文组装
- 但边界不总是这么干净——Claude Code 也做上下文注入、代码库索引分块、中断决策，深刻影响 Agent 行为

### 3. Harness vs Framework

| | Harness | Framework |
|--|---------|-----------|
| 代表 | Claude Code, Codex CLI, Gemini CLI | LangGraph, CrewAI |
| 本质 | 完整、有主见、开箱即用的执行平台 | 构建自定义 Harness 的工具包 |
| 适用 | 想立即开始使用 Agent 的开发者 | 构建特定领域 Agent 系统的团队 |

Framework 给你积木（Chain 抽象、Tool 接口、Memory 模块、检索模式），但你仍需自己接线——推理策略、权限模型、上下文组装、会话持久化。最终产物是一个 Harness，但 Framework 本身不是。

### 4. 三层价值：可组合性 + 诊断性

**可组合性**：
- 换模型不改 Harness（如 Claude Code 从 Sonnet 切 Opus）
- 换推理策略不改执行基础设施
- 换 Harness 保持同一模型和推理模式

**诊断性**：Agent 失败时，三层模型提供精准的排查词汇：
- 模型推理错误？→ Layer 1
- Agent 选错工具/追错计划？→ Layer 2
- 上下文组装不对/状态丢失/工具接口误导？→ Layer 3

**实际经验**："most failures I encounter in day-to-day use are Layer 3 problems. The model is smart enough. The reasoning loop is sound. The harness simply did not assemble the right context."

### 5. 核心论断

> "Compare harnesses. The model is the engine. The harness is everything else. And everything else is where the work actually gets done."

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 本文为三层架构提供精确定义
- [[entities/react-pattern|ReAct Pattern]] - 文中提到的推理循环范式
- [[entities/openclaw|OpenClaw]] - 可对比的 Harness 实现
- [[entities/hermes|Hermes]] - 可对比的 Harness 实现

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]]
- [[entities/harness-engineering|Harness Engineering]]
