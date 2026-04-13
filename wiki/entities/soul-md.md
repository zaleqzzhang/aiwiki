# SOUL.md

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-13
**来源数量**: 3

## 定义

SOUL.md 是一种 Agent 层面的持久记忆模式。Agent 拥有一个持续更新的配置文件，记录其"灵魂" — 核心身份、行为准则、积累的经验。这个文件在 Agent 生命周期内不断演进，形成 Agent 的"人格"和知识沉淀。

## 核心特征

- **持久性**：跨越会话存在，不随单次对话结束而消失
- **自主更新**：Agent 可以（在用户提示或自主决定下）更新自己的 SOUL.md
- **身份定义**：记录 Agent 是谁、擅长什么、如何行为
- **知识积累**：沉淀 Agent 在多次交互中学到的经验

## 实现案例

- **OpenClaw**: SOUL.md 是其上下文层学习的核心，Agent 通过"做梦"（离线分析 trace）来更新 SOUL.md
- **Hermes**: 每个 Profile 拥有独立的 SOUL.md，是 Profile 隔离的核心维度之一

## SOUL.md vs AGENTS.md 分离原则

Hermes 多 Agent 团队实践中明确了分离原则：

| 文件 | 用途 | 粒度 | 稳定性 |
|------|------|------|--------|
| SOUL.md | "who the agent is" — 身份、语气、优先级、行为边界 | 每个 Profile/Agent 独立 | 高 |
| AGENTS.md | "shared project context" — 仓库结构、编码规范、工具规则 | 项目级共享 | 中 |

**核心洞察**：如果只改 Profile 名字而不改 SOUL.md，你没有真正的团队，只有贴了标签的克隆。

## 与其他概念的关系

- [[entities/openclaw|OpenClaw]] → 使用 SOUL.md 模式
- [[entities/agent-architecture|Agent 架构]] → 属于上下文层学习的一种实现
- [[entities/memory-systems|记忆系统]] → SOUL.md 是一种记忆存储方式

## 相关来源

- [[sources/hermes-multi-agent-team|How to Build a Multi-Agent Team in Hermes]] - SOUL.md vs AGENTS.md 分离原则在多 Agent 团队中的实践
- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - 作为上下文层学习的典型案例
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - OpenClaw 使用 SOUL.md 作为持久记忆

## 实践启发

对于阿正的 WorkBuddy / Atlas 系统：
- ✅ 已在实践：Atlas 拥有 SOUL.md（身份定义）、IDENTITY.md（身份记录）、USER.md（用户画像），项目级用 AGENTS.md
- 当前架构天然符合 SOUL.md vs AGENTS.md 分离原则
- 如果未来扩展为多 Agent 团队，每个 Agent 应有独立 SOUL.md

## 开放问题

- ~~SOUL.md 与 CLAUDE.md、AGENTS.md 的关系是什么？~~ → 已明确：SOUL.md 定义身份（per-agent），AGENTS.md 定义项目上下文（per-project）
- 更新频率和触发条件应该如何设计？
- 如何防止 SOUL.md 膨胀或偏离初衷？
