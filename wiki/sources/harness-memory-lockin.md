# Your Harness, Your Memory

**类型**: article
**日期**: 2026-04-04
**来源**: https://x.com/hwchase17/status/2042978500567609738
**作者**: [[entities/hwchase17|Harrison Chase]]
**标签**: #AgentHarness #Memory #锁定效应 #开放标准

## 摘要

Harrison Chase 论述 Agent Harness 与 Memory 的紧密关系，提出核心警告：**如果你不拥有 Harness，你就不拥有 Memory**。当 Harness 背后的 API 是封闭的，你的 Memory 就被锁定在第三方平台。Memory 是 Agent 个性化和差异化的关键，Model Provider 有强烈动机通过 Memory 制造锁定。解决方案是使用开放 Harness（如 Deep Agents）。

## 关键要点

### Harness 不会消失

- 2023: RAG chains（LangChain）
- 2024: Complex flows（LangGraph）
- 2025-2026: Agent Harnesses（Claude Code, Deep Agents, OpenCode, Codex）
- Claude Code 源码泄露时显示 **512k lines of code** — 那就是 Harness

**反驳 "模型会吸收 Harness"**：
- 不是 Harness 消失，而是旧 Harness 被新 Harness 替代
- Agent 定义上就需要工具和数据交互的系统
- 内置 web search 不是 "模型的一部分"，是 API 背后的轻量 Harness

### Harness 与 Memory 的绑定

> "Asking to plug memory into an agent harness is like asking to plug driving into a car." — Sarah Wooders

**Harness 的 Memory 职责**：
- 短期记忆（对话消息、工具调用结果）
- 长期记忆（跨会话记忆）
- AGENTS.md / CLAUDE.md 如何加载进 context
- Skill 元数据如何展示
- 什么在 compaction 中保留，什么丢失
- 交互是否存储和可查询

**当前状态**：Memory 还在早期，没有通用抽象。长期记忆往往不在 MVP 中。

### 三种锁定程度

**轻度锁定**：使用有状态 API（OpenAI Responses API, Anthropic server-side compaction）
- 状态存在他们服务器
- 想换模型并恢复旧对话 → 不可能

**中度锁定**：使用封闭 Harness（Claude Agent SDK）
- Harness 如何与 memory 交互未知
- 生成的 artifacts 形状未知
- 无法从一个 Harness 迁移到另一个

**重度锁定**：整个 Harness + 长期记忆都在 API 后面
- 零 ownership 或 visibility
- 不拥有 memory，不知道 harness
- 示例：Claude Managed Agents

### Memory 的价值与锁定

**为什么 Memory 重要**：
- Agent 随用户交互变好
- 建立数据飞轮
- 个性化到每个用户
- 形成用户粘性

**锁定机制**：
- 没有 Memory → Agent 容易被复制
- 有 Memory → 构建私有数据集 → 差异化体验
- 换 Model Provider 容易（API 相似）
- 但如果 Memory 绑定在某个 Provider → 换不了

**个人案例**：Harrison 的邮件助手被删后重建，体验差很多 — 因为失去了积累的记忆。

### 解决方案：开放 Harness

**Deep Agents 设计**：
- 开源
- 模型无关
- 使用开放标准（agents.md, skills）
- 支持多种存储后端（Mongo, Postgres, Redis）
- 可部署（LangSmith Deployment 或任何 web hosting）

## 核心框架

```
Memory 锁定程度
├── 轻度：API 有状态
│   └── 换模型无法恢复旧对话
├── 中度：封闭 Harness
│   └── Memory artifacts 形状未知，不可迁移
└── 重度：API 后的完整 Harness
    └── 零 ownership，完全锁定

开放 Harness 的要求：
├── 开源
├── 模型无关
├── 开放标准（agents.md, skills）
├── 可选存储后端
└── 可部署
```

## 关联实体

- [[entities/hwchase17|Harrison Chase]] - 作者，LangChain 创始人
- [[entities/soul-md|SOUL.md]] - 文章提到 CLAUDE.md 作为 memory 实例
- [[entities/skill-system|Skill 系统]] - 文章提到 skills 作为开放标准
- [[entities/openclaw|OpenClaw]] - 文章提到 Pi（powers OpenClaw）作为开放 Harness 示例

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]] - 补充 Memory 与 Harness 的绑定关系
- [[entities/soul-md|SOUL.md]] - 作为 CLAUDE.md 的对比案例

## 扩展阅读

1. Sarah Wooders: "Memory isn't a plugin (it's the harness)"
2. Deep Agents Documentation
3. agents.md Specification
