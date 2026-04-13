# SOUL.md

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 2

## 定义

SOUL.md 是一种 Agent 层面的持久记忆模式。Agent 拥有一个持续更新的配置文件，记录其"灵魂" — 核心身份、行为准则、积累的经验。这个文件在 Agent 生命周期内不断演进，形成 Agent 的"人格"和知识沉淀。

## 核心特征

- **持久性**：跨越会话存在，不随单次对话结束而消失
- **自主更新**：Agent 可以（在用户提示或自主决定下）更新自己的 SOUL.md
- **身份定义**：记录 Agent 是谁、擅长什么、如何行为
- **知识积累**：沉淀 Agent 在多次交互中学到的经验

## 实现案例

- **OpenClaw**: SOUL.md 是其上下文层学习的核心，Agent 通过"做梦"（离线分析 trace）来更新 SOUL.md

## 与其他概念的关系

- [[entities/openclaw|OpenClaw]] → 使用 SOUL.md 模式
- [[entities/agent-architecture|Agent 架构]] → 属于上下文层学习的一种实现
- [[entities/memory-systems|记忆系统]] → SOUL.md 是一种记忆存储方式

## 相关来源

- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - 作为上下文层学习的典型案例
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - OpenClaw 使用 SOUL.md 作为持久记忆

## 实践启发

对于阿正的 WorkBuddy / Atlas 系统：
- SOUL.md 可能已经在用（需要确认当前架构）
- 可考虑让 Atlas 拥有自己的 SOUL.md，定义其身份、能力边界、与阿正的协作模式
- 更新机制：显式（阿正要求记住）vs 隐式（Atlas 自主更新）

## 开放问题

- SOUL.md 与 CLAUDE.md、AGENTS.md 的关系是什么？命名差异还是实质差异？
- 更新频率和触发条件应该如何设计？
- 如何防止 SOUL.md 膨胀或偏离初衷？
