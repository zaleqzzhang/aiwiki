# Agent 架构

**创建日期**: 2026-04-12
**最后更新**: 2026-04-13
**来源数量**: 3

## 概述

AI Agent 架构正在从简单的"模型 + 提示词"演变为多层可配置系统。当前主流的架构模式是三层划分：Model（模型层）→ Harness（Harness 层）→ Context（上下文层）。每一层都可以独立优化和更新，形成了 Agent 持续学习的完整路径。

## 核心概念

### 三层架构

**Model（模型层）**
- 定义：模型权重本身
- 更新方式：SFT、RL（如 GRPO）
- 挑战：[[entities/catastrophic-forgetting|灾难性遗忘]]

**Harness（Harness 层）**
- 定义：驱动 Agent 的代码 + 固定指令/工具
- 更新方式：[[entities/meta-harness|Meta-Harness]]
- 特点：通常在 Agent 整体层面更新

**Context（上下文层）**
- 定义：Harness 外的可配置内容（指令、技能、工具），也称为"记忆"
- 更新方式：离线批处理（"做梦"）或热路径更新
- 特点：最灵活，可在 Agent/用户/组织不同粒度更新

### 工程方法论的嵌套关系

三层架构对应三层工程方法论，构成嵌套层级（非平行选择）：

**Prompt Engineering ⊂ Context Engineering ⊂ Harness Engineering**

| 工程方法论 | 核心问题 | 对应架构层 |
|-----------|---------|-----------|
| Prompt Engineering | "How should I phrase this?" | 单次交互指令 |
| Context Engineering | "What does the model need to know?" | Context 层管理 |
| Harness Engineering | "How do agents operate reliably?" | Harness 层设计 |

量化证据（Princeton）：仅改变 harness 配置，solve rate 提升 64%。同一 Claude Opus 4.5 在不同 harness 下：2% vs 12%。

### 核心组件

- [[entities/soul-md|SOUL.md]] - Agent 层面的持久记忆模式
- [[entities/langsmith|LangSmith]] - Trace 收集平台
- [[entities/deep-agents|Deep Agents]] - 开源 Harness 实现

## 演进趋势

1. **从单一模型到多层系统**：早期 Agent 只是模型 + 提示词，现在分化为三层
2. **从固定到可演进**：每一层都支持持续学习，不是静态配置
3. **从通用到个性化**：上下文层支持用户/组织级别的定制
4. **Trace 驱动**：所有改进都基于执行 trace，形成数据闭环
5. **Harness 职责分化**：随工程复杂度增加，Harness 的不同职责分化为独立层——规范层（OpenSpec）、纪律层（Superpowers）、协作层（Harness/Agent Team）。三层遵循"规范不变、流程不变、团队可弹性扩展"的配合原则

## 典型案例

| 系统 | Model | Harness | Context |
|------|-------|---------|---------|
| Claude Code | claude-sonnet | Claude Code | CLAUDE.md, skills, mcp.json |
| OpenClaw | 多种 | Pi + 脚手架 | SOUL.md, clawhub skills |
| Deep Agents | 多种 | Deep Agents | 用户级记忆、后台学习 |

## 与其他主题的关系

- [[topics/continual-learning|持续学习]] → Agent 架构的演进动力
- [[topics/memory-systems|记忆系统]] → 上下文层的核心功能

## 相关来源

- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - Harrison Chase 对三层架构的系统阐述
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - Harness 的 12 组件拆解
- [[sources/harness-memory-lockin|Your Harness, Your Memory]] - Harness 与 Memory 的绑定关系
- [[sources/prompt-vs-context-vs-harness|Prompt vs Context vs Harness Engineering]] - 三层嵌套层级关系 + Princeton 64% 数据
- [[sources/openspec-superpowers-harness|OpenSpec、Superpowers 和 Harness]] - 多 Agent 开发场景的三层拼图模型

## 待探索方向

- 三层之间的边界在哪里？什么内容应该放在 Harness 而非 Context？
- 不同粒度（Agent vs 用户 vs 组织）的上下文如何协同？
- 如何评估架构改进的效果？
- Trace 分析的自动化程度可以多高？

## 实践启发

对于 Atlas / WorkBuddy：
- 当前架构：Model（底层 LLM）+ Harness（WorkBuddy 核心）+ Context（SOUL.md, IDENTITY.md, USER.md）
- 可改进：引入更系统的 trace 收集和分析
- 可改进：明确 Context 层的更新机制（显式 vs 隐式、离线 vs 在线）
