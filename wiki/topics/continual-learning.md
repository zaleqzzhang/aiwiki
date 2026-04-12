# 持续学习

**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 概述

持续学习指系统在生命周期内不断改进的能力。对于 AI Agent，持续学习不应局限于模型权重更新，而应扩展到 Harness 和 Context 层。每一层有不同的学习方式、粒度、挑战和适用场景。

## 核心概念

### 三层持续学习

**模型层**
- 方式：SFT、RL（GRPO）
- 粒度：通常 Agent 级
- 挑战：[[entities/catastrophic-forgetting|灾难性遗忘]]
- 成本：高（需要训练资源）

**Harness 层**
- 方式：[[entities/meta-harness|Meta-Harness]]
- 粒度：通常 Agent 级
- 成本：中（需要 trace 分析）
- 特点：Agent 改进 Agent

**上下文层**
- 方式：离线批处理或热路径更新
- 粒度：Agent / 用户 / 组织 级
- 成本：低（更新配置文件）
- 特点：最灵活，最常用

### 更新粒度

| 粒度 | 说明 | 案例 |
|------|------|------|
| Agent 级 | 所有用户共享的改进 | OpenClaw 的 SOUL.md |
| 组织级 | 团队/公司级别的定制 | Sierra Explorer |
| 用户级 | 个人级别的记忆和偏好 | Hex Context Studio |

### 更新时机

- **离线批处理**：分析近期 trace → 提取洞察 → 更新（OpenClaw 称为"做梦"）
- **热路径更新**：Agent 在执行任务过程中直接更新记忆

### 更新触发

- **显式**：用户提示 Agent 记住某事
- **隐式**：Agent 根据核心指令自主决定更新

## 关键洞察

1. **Trace 是数据源**：所有层的持续学习都依赖执行 trace
2. **不一定要改模型**：Context 层学习成本低、风险小、效果明显
3. **粒度很重要**：不同粒度适合不同场景，可混合使用
4. **自动化程度**：Harness 和 Context 层可实现高度自动化

## 与其他主题的关系

- [[topics/agent-architecture|Agent 架构]] → 持续学习的载体
- [[topics/memory-systems|记忆系统]] → Context 层学习的实现

## 相关来源

- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - Harrison Chase 对三层持续学习的系统阐述

## 待探索方向

- 如何平衡离线更新和在线更新的效率？
- 如何防止上下文层记忆的膨胀和噪声积累？
- 不同层的更新频率应该如何设计？
- 如何评估持续学习的效果？

## 实践启发

对于 Atlas / WorkBuddy：
- 当前：主要依赖 Context 层学习（SOUL.md, MEMORY.md, 日志文件）
- 可增强：
  - 引入更系统的 trace 收集（LangSmith 或自建）
  - 实现"做梦"机制：定期分析 trace，提取模式，更新配置
  - 支持显式和隐式两种记忆更新方式
