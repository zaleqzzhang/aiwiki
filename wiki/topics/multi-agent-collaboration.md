# 多 Agent 协作

**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 概述

多 Agent 协作是现代 AI Agent 系统的核心命题：如何让多个 agent 高效协作，共同完成复杂任务。当前主要有两种设计哲学：同步阻塞式（Hermes）和异步编排式（OpenClaw），它们在隔离性、灵活性、token 效率上存在本质差异。

## 核心模式

### 同步阻塞式（Hermes 模式）

**"总承包商-分包商"模式**

```
父 agent（项目经理）
    ├── 拆解任务
    ├── 分发给子 agent
    └── 阻塞等待结果返回
        ├── 子 agent A（独立执行，中间过程隔离）
        ├── 子 agent B（独立执行，中间过程隔离）
        └── 子 agent C（独立执行，中间过程隔离）
```

**特点**：
- 强隔离：子 agent 中间推理不污染父 agent 上下文
- 高效并行：最多 3 个任务并行
- Token 节省：只返回 summary，上下文零膨胀
- 缺乏灵活性：无法调整方向，无法 Steer

### 异步编排式（OpenClaw 模式）

**"交响乐团"模式**

```
父 agent（指挥家）
    ├── 定义全局拓扑
    └── 异步运行，持续监听
        ├── agent A（固定角色，事件驱动）
        ├── agent B（固定角色，事件驱动）
        └── agent C（固定角色，事件驱动）
            ↓
        announce 推送结果
            ↓
        父 agent 接收并处理
            ↓
        可选：发送 Steer 调整方向
```

**特点**：
- 异步事件驱动：spawn 后立即返回
- Steer 能力：可向运行中的子 agent 发送引导
- 持久化恢复：支持中断后恢复
- Token 开销大：announce 和 steer 产生额外开销

## 核心概念

### 角色体系

| 角色 | 职责 | 可否再 spawn |
|------|------|-------------|
| main | 主 agent，负责任务分发和结果整合 | ✅ |
| orchestrator | 编排器，负责子任务编排 | ✅ |
| leaf | 叶节点，执行具体任务 | ❌ |

### 通信模式

**单向返回（Hermes）**
```
父 → 子：goal + context
子 → 父：summary
```

**事件推送 + Steer（OpenClaw）**
```
父 → 子：goal + context + Steer（可选）
子 → 父：announce（结果以 user message 形式推送）
```

### 生命周期管理

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 迭代控制 | 默认 50 轮 | 可配置 |
| 时间控制 | ❌ 无超时 | ✅ runTimeoutSeconds（默认 300s） |
| 持久化 | ❌ 无 | ✅ runs.json |
| 恢复 | ❌ 无 | ✅ resumeSessionId |

### 隔离性 vs 灵活性

```
强隔离 ←─────────────────────────────→ 弱隔离
   Hermes                                OpenClaw
   ↓                                        ↓
安全、高效                               灵活、可控
无法调整                                 可 Steer
上下文零膨胀                             上下文膨胀
```

## 架构选择

### 选择 Hermes 模式

- 并行处理 3 个互不相关的任务
- 需要极致 token 效率
- 需要强隔离（安全敏感场景）
- 任务目标明确，无需调整方向
- 快速集成、简单决策-执行流程

### 选择 OpenClaw 模式

- 需要在子 agent 运行中调整方向
- 需要超时控制防止无限运行
- 调用外部编码 agent（Claude Code / Codex）
- 构建多层嵌套 agent 树
- 超过 3 个 agent 并行工作
- 长流程任务（需要持久化和恢复）

### 混合架构

```
OpenClaw 异步编排层（处理复杂工作流）
    ├── research agent（ACP 调用 Claude Code）
    ├── analysis agent
    │   └── 内部使用 Hermes delegate_task 并行处理 3 个任务
    └── writer agent
        └── 与 research 通过 announce/steer 协作
```

**原则**：在需要隔离时强隔离，在需要协作时强协作。

## 核心组件

- [[entities/hermes|Hermes]] - 同步阻塞式框架
- [[entities/openclaw|OpenClaw]] - 异步编排式框架
- [[entities/delegate-task|delegate_task]] - 任务委派机制
- [[entities/steer-mechanism|Steer 机制]] - 运行时引导能力
- [[entities/acp-protocol|ACP 协议]] - 外部 Agent 调用协议

## 与其他主题的关系

- [[topics/agent-architecture|Agent 架构]] → 多 Agent 是架构的扩展
- [[topics/continual-learning|持续学习]] → 多 Agent 系统的持续改进

## 相关来源

- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - 详细对比分析

## 待探索方向

- 如何量化"隔离性"和"灵活性"的权衡？
- 混合架构的最佳实践是什么？
- 如何评估多 Agent 系统的协作效率？
- Trace 分析如何帮助改进协作模式？

## 实践启发

对于 Atlas / WorkBuddy：
- 当前：主要是单 Agent 模式
- 可扩展：支持多 Agent 协作（如 coding agent + research agent）
- 选择：根据任务特性选择同步阻塞或异步编排
- 改进：引入 Steer 能力，支持运行时调整
