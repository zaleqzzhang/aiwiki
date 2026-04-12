# LLM Wiki 索引

最后更新：2026-04-12

---

## 概览

- [[overview|知识库概览]]

---

## 来源摘要 (3)

- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - Harrison Chase 论述 Agent 持续学习的三层架构：Model、Harness、Context
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - Hermes delegate_task 与 OpenClaw subagent 的多 Agent 协作机制对比
- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]] - 基于 Harness Engineering 三根支柱的 Hermes vs MemOS 深度对比

---

## 实体页面 (9)

### 人物
- [[entities/hwchase17|Harrison Chase]] - LangChain 创始人，Agent 架构领域活跃思考者
- [[entities/servasyy-ai|servasyy_ai]] - 多 Agent 协作机制研究者，Harness Engineering 实践者

### 组织
*暂无*

### 概念
- [[entities/soul-md|SOUL.md]] - Agent 层面的持久记忆模式，定义 Agent 身份并持续积累经验
- [[entities/catastrophic-forgetting|灾难性遗忘]] - 模型层持续学习的核心挑战，更新权重导致旧能力退化
- [[entities/meta-harness|Meta-Harness]] - Harness 层持续学习方法，用 Agent 分析 trace 改进 Harness
- [[entities/steer-mechanism|Steer 机制]] - 运行时引导能力，向运行中的子 agent 发送消息调整方向
- [[entities/harness-engineering|Harness Engineering]] - 构建可靠 Agent 系统的方法论框架，三根支柱：记忆治理、架构约束、评估闭环
- [[entities/hook-mechanism|Hook 机制]] - Agent 生命周期的事件拦截器，实现系统级强制执行
- [[entities/skill-system|Skill 系统]] - 将任务轨迹提炼为可复用经验的机制

### 工具
- [[entities/hermes|Hermes]] - 同步阻塞式多 Agent 框架，追求极致隔离性和 token 效率
- [[entities/openclaw|OpenClaw]] - 异步编排式多 Agent 框架，支持 Steer、持久化、ACP 协议

---

## 主题页面 (3)

- [[topics/agent-architecture|Agent 架构]] - AI Agent 的三层架构模式：Model → Harness → Context
- [[topics/continual-learning|持续学习]] - Agent 系统在三层架构上的持续改进能力
- [[topics/multi-agent-collaboration|多 Agent 协作]] - 同步阻塞 vs 异步编排两种协作模式

---

## 综合分析 (1)

- [[synthesis/ai-agent-systems-overview|AI Agent 系统架构全景图]] - 综合三篇核心文章，构建完整的 Agent 架构、持续学习、多 Agent 协作、Harness Engineering 方法论体系

---

## 统计

- 来源总数：3
- 实体总数：9
- 主题总数：3
- 综合分析数：1
- 最后更新：2026-04-12
