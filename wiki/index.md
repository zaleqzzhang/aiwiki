# LLM Wiki 索引

最后更新：2026-04-13

---

## 概览

- [[overview|知识库概览]]

---

## 来源摘要 (11)

- [[sources/hermes-agent-inside|Inside Hermes Agent]] - Hermes 自我改进 AI Agent 完整架构：学习循环、四层记忆、Gateway
- [[sources/long-running-agents|Effective Harnesses for Long-Running Agents]] - Anthropic 官方长时运行 Agent 实践：两阶段方案、增量推进
- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - Harrison Chase 论述 Agent 持续学习的三层架构：Model、Harness、Context
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - Hermes delegate_task 与 OpenClaw subagent 的多 Agent 协作机制对比
- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]] - 基于 Harness Engineering 三根支柱的 Hermes vs MemOS 深度对比
- [[sources/llm-training-pipeline|大模型训练：原理、路径与新实践]] - 2026 年大模型训练链路全景，强调后训练和 Harness 优化
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - Agent Harness 的 12 组件深度拆解
- [[sources/harness-memory-lockin|Your Harness, Your Memory]] - Harness 与 Memory 的绑定关系，警告封闭 Harness 的锁定风险
- [[sources/coding-agents-reshape-epd|Coding Agents 重塑 EPD]] - Coding Agents 对工程、产品、设计角色的影响
- [[sources/building-claude-skills-guide|如何构建真正有效的 Claude Skills]] - Skill 设计的五组件结构和测试方法
- [[sources/agent-builder-memory-system|Agent Builder 的记忆系统]] - LangSmith Agent Builder 记忆系统的设计与实现

---

## 实体页面 (12)

### 人物
- [[entities/hwchase17|Harrison Chase]] - LangChain 创始人，Agent 架构领域活跃思考者，5 篇核心文章作者
- [[entities/servasyy-ai|servasyy_ai]] - 多 Agent 协作机制研究者，Harness Engineering 实践者
- [[entities/HiTw93|@HiTw93]] - 大模型训练链路研究者，《你不知道》系列文章作者
- [[entities/akshay-pachaar|Akshay Pachaar]] - Agent Harness 实践者，12 组件拆解作者
- [[entities/eng-khairallah1|@eng_khairallah1]] - Claude Skills 设计专家

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

- [[synthesis/ai-agent-systems-overview|AI Agent 系统架构全景图]] - 综合六篇核心文章，构建完整的 Agent 架构、持续学习、多 Agent 协作、Harness Engineering 方法论体系

---

## 创作层 (craft/)

### 概念讲解卡片 (7)
- [[craft/concepts/harness|Harness]] - Agent 的运行时系统，模型"跑"在什么上面
- [[craft/concepts/trace|Trace]] - Agent 完整执行路径的记录，所有改进的数据源
- [[craft/concepts/skill|Skill]] - 从成功和失败中提炼的结构化可复用经验
- [[craft/concepts/agent-memory|Agent Memory]] - Agent 积累自身经验的系统，不是 RAG
- [[craft/concepts/three-layer-learning|三层学习]] - Model / Harness / Context 三个层次的持续改进
- [[craft/concepts/hook-mechanism|Hook 机制]] - Agent 生命周期的自动拦截器，100% 捕获保障
- [[craft/concepts/meta-harness|Meta-Harness]] - 用 Agent 改进 Agent 的 Harness

### 设计笔记 (4)
- [[craft/design-notes/hermes-memory-limit|Hermes 记忆上限]] - 为什么选择 3,575 字符？约束即特性
- [[craft/design-notes/memos-triple-dedup|MemOS 三层去重]] - 哈希→向量→LLM 的成本梯度设计
- [[craft/design-notes/hermes-subagent-sync-blocking|子 Agent 同步阻塞]] - 为什么不用异步？上下文污染是更大的问题
- [[craft/design-notes/skill-auto-generation-quality|Skill 质量双关卡]] - confidence + 质量评分缺一不可

### 认知缺口 (3)
- [[craft/gaps/rag-is-not-memory|RAG ≠ Agent Memory]] - 查百科 vs 翻工作日志
- [[craft/gaps/prompt-is-not-harness|Prompt ≠ Harness]] - 一封邮件 vs 整个工作环境
- [[craft/gaps/skill-is-not-tool|Skill ≠ Tool]] - 手 vs 肌肉记忆

### 视角 (2)
- [[craft/perspectives/memory-autonomy-vs-system-enforcement|记忆自治 vs 系统强制]] - Hermes 路径 vs MemOS 路径
- [[craft/perspectives/constraint-vs-unbounded-design|约束式 vs 无界式设计]] - 有限空间强制策展 vs 无限存储智能管理

---

## 统计

- 来源总数：11
- 实体总数：12
- 主题总数：3
- 综合分析数：1
- 创作层：7 概念 + 4 设计笔记 + 3 认知缺口 + 2 视角
- 最后更新：2026-04-13
