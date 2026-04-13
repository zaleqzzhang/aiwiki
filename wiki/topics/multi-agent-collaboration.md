# 多 Agent 协作

**创建日期**: 2026-04-12
**最后更新**: 2026-04-13
**来源数量**: 4

## 概述

多 Agent 协作是现代 AI Agent 系统的核心命题：如何让多个 agent 高效协作，共同完成复杂任务。当前主要有三种设计哲学：

1. **临时子 Agent + 同步阻塞**（Hermes delegate_task）：任务级隔离，用完即弃
2. **异步编排 + 事件驱动**（OpenClaw）：模块化分工，运行时可调
3. **持久化 Profile 隔离**（Hermes Profiles）：角色级隔离，各 Agent 长期运行、独立积累

前两种面向"任务协作"（临时的），第三种面向"团队协作"（持久的）。

## 两种哲学

### 单 Agent 架构（Hermes 核心）

**"知识积累"模式**

```
单 Agent（持续运行）
    ├── 学习循环
    │   ├── Periodic Nudge → 策展记忆
    │   ├── Skill Creation → 提取流程
    │   ├── Skill Improvement → 优化流程
    │   └── Session Search → 检索历史
    ├── 四层记忆
    │   ├── Prompt Memory（始终在线）
    │   ├── Session Search（按需检索）
    │   ├── Skills（渐进加载）
    │   └── Honcho（用户建模）
    └── 可选多 Agent
        └── delegate_task（同步阻塞）
```

**特点**：
- 通过使用积累知识和技能
- 越用越聪明（自我改进闭环）
- Token 效率高（渐进式加载）
- 多 Agent 是可选能力，非核心

### 多 Agent 架构（OpenClaw）

**"模块化分工"模式**

```
编排层（外部控制）
    ├── Agent A（固定角色）
    ├── Agent B（固定角色）
    └── Agent C（固定角色）
        ↓
    事件驱动协作
    ├── announce 推送结果
    └── Steer 调整方向
```

**特点**：
- 外部编排控制
- 模块化分工
- 灵活性高（可 Steer）
- 需要设计协作拓扑

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

### Profile 持久隔离（Hermes Profiles）

**"专业团队"模式**

```
Profile 层（持久化角色隔离）
    ├── Hermes（Orchestrator）—— 规划、分解、综合
    ├── Alan（Research）—— 证据、验证、怀疑精神
    ├── Mira（Writer）—— 清晰、结构、受众意识
    └── Turing（Builder）—— 实现、调试、测试
        ↓
    每个 Profile 完全独立：memory / sessions / skills / personality / cron / gateway
        ↓
    SOUL.md（身份）独立 + AGENTS.md（项目上下文）共享
        ↓
    Gateway + Messaging → 跨平台控制面
```

**与前两种模式的本质区别**：

| 维度 | delegate_task（临时） | OpenClaw 编排（任务级） | Profile（持久） |
|------|---------------------|----------------------|----------------|
| 生命周期 | 任务结束即销毁 | 任务/流程级 | 持久存在 |
| 记忆积累 | ❌ 无 | 有限 | ✅ 各自独立积累 |
| 身份定义 | 无（通用子 agent） | 角色配置 | SOUL.md（完整身份） |
| 适用场景 | 并行子任务 | 复杂工作流 | 长期专业团队 |

**关键洞察**："从单 Agent 到多 Agent 团队的起点是隔离，不是更好的 prompting。"

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

- [[sources/hermes-multi-agent-team|How to Build a Multi-Agent Team in Hermes]] - Profile 持久隔离 + 四角色团队 + SOUL.md/AGENTS.md 分离
- [[sources/openspec-superpowers-harness|OpenSpec、Superpowers 和 Harness]] - 多 Agent 开发的三层拼图：规范→纪律→协作
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - 多 Agent 协作模式对比
- [[sources/hermes-agent-inside|Inside Hermes Agent]] - Hermes 单 Agent 架构、四层记忆、学习循环

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
