# LLM Wiki 操作日志

---

## [2026-04-12 11:08] synthesis | AI Agent 系统架构全景图

**操作**:
- 创建 [[synthesis/ai-agent-systems-overview|综合分析]]
- 整合三篇核心文章的核心框架
- 构建 Agent 架构、持续学习、多 Agent 协作、Harness Engineering 的完整方法论体系
- 提炼选择框架和对 Atlas/WorkBuddy 的改进方向
- 更新 [[index.md]]

**关键产出**:
1. 三层架构模型（Model → Harness → Context）
2. 持续学习三层路径
3. 多 Agent 协作两种模式（同步阻塞 vs 异步编排）
4. Harness Engineering 三根支柱（记忆治理、架构约束、评估闭环）
5. 选择框架（按场景、时间、需求）
6. 对 Atlas/WorkBuddy 的短期/中期/长期改进方向

---

## [2026-04-12 10:54] ingest | 给你的 Agent 搭操作系统

**来源**: https://x.com/servasyy_ai/status/2042473092036116847
**作者**: servasyy_ai

**操作**:
- 创建 [[sources/hermes-vs-memos-harness-engineering|来源摘要]]
- 创建实体页面：
  - [[entities/harness-engineering|Harness Engineering]]（概念）
  - [[entities/hook-mechanism|Hook 机制]]（概念）
  - [[entities/skill-system|Skill 系统]]（概念）
- 更新 [[entities/servasyy-ai|servasyy_ai]]（新增来源引用）
- 更新 [[index.md]]
- 新增交叉引用 6 处

**关键洞察**:
Harness Engineering 三根支柱：记忆治理、架构约束、评估闭环。两种实现路径：Hermes（有界热记忆 + 主动进化）vs MemOS（无界记忆 + 系统强制）。轨迹是核心资产，Hook 是 100% 捕获的关键。

---

## [2026-04-12 10:48] ingest | 同步阻塞 vs 异步编排

**来源**: https://x.com/servasyy_ai/status/2042951017462169812
**作者**: servasyy_ai

**操作**:
- 创建 [[sources/hermes-openclaw-multi-agent-comparison|来源摘要]]
- 创建实体页面：
  - [[entities/servasyy-ai|servasyy_ai]]（人物）
  - [[entities/hermes|Hermes]]（工具）
  - [[entities/openclaw|OpenClaw]]（工具）
  - [[entities/steer-mechanism|Steer 机制]]（概念）
- 创建主题页面：
  - [[topics/multi-agent-collaboration|多 Agent 协作]]
- 更新 [[entities/soul-md|SOUL.md]]（新增来源引用）
- 更新 [[index.md]]
- 新增交叉引用 8 处

**关键洞察**:
多 Agent 协作有两种设计哲学：Hermes 的同步阻塞式（强隔离、高效、无 Steer）vs OpenClaw 的异步编排式（灵活、可控、开销大）。混合架构可取长补短。

---

## [2026-04-12 10:38] ingest | Continual Learning for AI Agents

**来源**: https://x.com/hwchase17/status/2040467997022884194
**作者**: Harrison Chase

**操作**:
- 创建 [[sources/continual-learning-ai-agents|来源摘要]]
- 创建实体页面：
  - [[entities/hwchase17|Harrison Chase]]（人物）
  - [[entities/soul-md|SOUL.md]]（概念）
  - [[entities/catastrophic-forgetting|灾难性遗忘]]（概念）
  - [[entities/meta-harness|Meta-Harness]]（概念）
- 创建主题页面：
  - [[topics/agent-architecture|Agent 架构]]
  - [[topics/continual-learning|持续学习]]
- 更新 [[index.md]]
- 新增交叉引用 12 处

**关键洞察**:
Agent 持续学习应区分三层：模型层（挑战：灾难性遗忘）、Harness 层（方法：Meta-Harness）、上下文层（最灵活，支持多粒度）。Trace 是所有改进的数据源。

---

## [2026-04-12 10:27] init | 知识库初始化

- 创建目录结构
- 创建 `AGENTS.md` schema 文件
- 创建 `index.md` 索引文件
- 创建 `log.md` 日志文件
- 创建 `overview.md` 概览页面

知识库基于 LLM Wiki 方法论构建，采用三层架构：
- `raw/` 原始素材（只读）
- `wiki/` 知识库主体（LLM 维护）
- `AGENTS.md` LLM 行为规范

下一步：将第一份素材放入 `raw/` 目录，开始摄入流程。
