# LLM Wiki 操作日志

---

## [2026-04-13 13:50] craft/draft | 一个 Agent 系统 99% 的代码都不是 Prompt

**触发**: 用户指令——选题写作

**文章结构**:
1. 反直觉开场：Claude Code 512,000 行代码，Prompt 只有几千行
2. 三层嵌套关系：Prompt ⊂ Context ⊂ Harness + Princeton 6x 数据
3. 12 组件拆解：Prompt 只覆盖其中一小部分
4. 四个关键 Harness 组件展开：上下文策展、压缩算法、记忆系统、安全沙箱
5. 认知纠偏：Harness ≠ Workflow（主导权归属）
6. Prompt 的正确定位：决定下限，Harness 决定上限
7. 行动指南：四步优先级排序

**使用的 Craft 素材**: 
- 概念卡片：harness、context-engineering、agent-memory、meta-harness、skill
- 认知缺口：prompt-is-not-harness、harness-is-not-workflow
- 设计笔记：openclaw-compaction-algorithm

**操作**: 新建草稿 [[craft/drafts/99-percent-is-not-prompt]]，更新 [[index.md]]（新增草稿分类）

---

## [2026-04-13 13:38] craft | 高优先级 Craft 卡片补建（3 张）

**触发**: 用户指令，基于 Craft 层审计报告的高优先级建议

**操作**:
- 新建概念卡片：[[craft/concepts/context-engineering|Context Engineering]] — 六大组件、Context Rot、策展原则、OpenClaw 实现案例
- 新建认知缺口：[[craft/gaps/harness-is-not-workflow|Harness ≠ Workflow]] — 主导权归属辨析、OpenClaw 源码对比表、模型能力分水岭
- 新建设计笔记：[[craft/design-notes/openclaw-compaction-algorithm|OpenClaw 压缩算法]] — 40% chunk ratio、三级摘要降级、5 分钟超时、保留项选择
- 更新 [[index.md]]（概念 7→8，设计笔记 4→5，认知缺口 3→4）

**卡片质量说明**:
1. **Context Engineering 概念卡片**：从 3 篇来源（prompt-vs-context-vs-harness、openclaw-prompt-context-harness、anatomy-of-agent-harness）提取六大组件框架，与 harness 概念卡片形成"子集→超集"的清晰边界。Context Rot 作为关键设计挑战独立成段。
2. **Harness ≠ Workflow 认知缺口**：以飞樰的源码辨析为核心素材，构建"修复 Bug"的具体对比场景。关键论断："模型能力越强，Harness 越优于 Workflow"——判断标准是"执行路径能否提前确定"。
3. **OpenClaw 压缩算法设计笔记**：从 openclaw 实体和源码分析来源提取完整参数，逐一分析工程权衡。核心启发："知道什么不该丢，比知道什么该留更重要。"

**Craft 层状态**: 8 概念 + 5 设计笔记 + 4 认知缺口 + 2 视角 = **19 个文件**

---

## [2026-04-13 13:27] ingest | OpenSpec、Superpowers 和 Harness：AI 工程化开发的三层拼图

**来源**: https://mp.weixin.qq.com/s/ssH_OtuLxy4RZD2tKiSKXA
**作者**: 岚天逸剑
**深度**: 实践框架

**操作**:
- 创建来源摘要：[[sources/openspec-superpowers-harness|OpenSpec、Superpowers 和 Harness：三层拼图]]
- 更新实体页面：[[entities/harness-engineering|Harness Engineering]]（来源 3→4，新增：三层拼图模型——规范层/纪律层/协作层 + 落地优先级 + 与三支柱的关系辨析）
- 更新主题页面：[[topics/agent-architecture|Agent 架构]]（来源 2→3，新增：Harness 职责分化趋势）
- 更新主题页面：[[topics/multi-agent-collaboration|多 Agent 协作]]（来源 3→4，新增来源引用）
- **Craft 增量更新**：
  - 更新 [[craft/concepts/harness|Harness 概念卡片]]：补充三层拼图模型作为新例子——Harness 职责随复杂度增加而分化
  - 更新 [[craft/concepts/skill|Skill 概念卡片]]：补充 Superpowers 作为"纪律型 Skill"——预定义规范注入而非从 trace 提炼
- 更新 [[index.md]]（来源 16→17）
- 新增交叉引用 5 处

**关键洞察**:
1. **Harness 不是铁板一块**：随工程复杂度增加，Harness 的规范/纪律/协作三种职责会分化为独立的层
2. **两种 Skill 路径**：从 trace "积累→提炼"（Hermes/MemOS）vs 从最佳实践"规范→注入"（Superpowers），各有适用场景
3. **落地优先级精准**：规范 → 纪律 → 协作，因为纪律没有规范则无标准可循，协作没有规范和纪律则越多越乱
4. **"规范不变、流程不变、团队可弹性扩展"**——三层中前两层追求稳定，第三层追求灵活，这是工程化的核心

**建议新增**:
- 可考虑新增 OpenSpec 和 Superpowers 实体页面（当有更多来源时）
- 可考虑新增 `craft/gaps/skill-两条路径.md`：读者以为 Skill 只有"从经验中学习"一条路，实际还有"预定义纪律注入"

---

## [2026-04-13 13:21] ingest | How to Build a Multi-Agent Team in Hermes

**来源**: https://x.com/neoaiforecast/status/2043455838459920718
**作者**: @neoaiforecast
**深度**: 实践指南

**操作**:
- 创建来源摘要：[[sources/hermes-multi-agent-team|How to Build a Multi-Agent Team in Hermes]]
- 更新实体页面：[[entities/hermes|Hermes]]（来源 2→3，新增：Profile 隔离七维度、SOUL.md vs AGENTS.md 分离、四角色团队模型、Profile + Gateway 跨平台控制面）
- 更新实体页面：[[entities/soul-md|SOUL.md]]（来源 2→3，新增：Hermes Profile 用法 + SOUL.md vs AGENTS.md 分离原则 + 实践启发更新）
- 更新主题页面：[[topics/multi-agent-collaboration|多 Agent 协作]]（来源 2→3，新增：Profile 持久隔离作为第三种多 Agent 模式 + 三模式对比表）
- 更新 [[index.md]]（来源 15→16）
- 新增交叉引用 5 处

**关键洞察**:
1. **三种多 Agent 模式的光谱**：delegate_task（临时子 Agent）→ OpenClaw 编排（任务级隔离）→ Profile（持久角色隔离），维度是"生命周期"
2. **"隔离是起点，不是更好的 prompting"**——记忆污染和上下文混乱比 prompt 质量更影响多 Agent 效果
3. **SOUL.md vs AGENTS.md 的分离**本质上回答了之前的开放问题"SOUL.md 与 CLAUDE.md/AGENTS.md 的关系"
4. **Atlas 已在实践**：SOUL.md + IDENTITY.md + USER.md（身份层）vs AGENTS.md（项目层），天然符合分离原则

---

## [2026-04-13 13:17] ingest | Prompt Engineering vs Context Engineering vs Harness Engineering

**来源**: https://dev.to/ljhao/prompt-engineering-vs-context-engineering-vs-harness-engineering-whats-the-difference-in-2026-37pb
**作者**: PrivOcto
**深度**: 对比综述

**操作**:
- 创建来源摘要：[[sources/prompt-vs-context-vs-harness|Prompt vs Context vs Harness Engineering]]
- 更新实体页面：[[entities/harness-engineering|Harness Engineering]]（来源 2→3，新增：嵌套层级关系、Princeton 64% 数据、Boeckeler 三支柱、统计数据）
- 更新主题页面：[[topics/agent-architecture|Agent 架构]]（新增：工程方法论嵌套关系段落）
- **Craft 增量更新**：
  - 更新 [[craft/concepts/harness|Harness 概念卡片]]：补充 Princeton 精确数据（2% vs 12%）、Context Engineering 与 Harness 的边界
  - 更新 [[craft/gaps/prompt-is-not-harness|Prompt ≠ Harness 认知缺口]]：补充三层嵌套关系、Princeton 数据、MIT 95% 统计
- 更新 [[index.md]]（来源 14→15）
- 新增交叉引用 4 处

**关键洞察**:
1. **嵌套而非平行**：Prompt ⊂ Context ⊂ Harness 是层级关系，不是三选一
2. **Princeton 量化**：64% solve rate 提升 + 6x 差距，全归因于 harness 环境设计
3. **Boeckeler 三支柱**补充：Context Engineering + Architectural Constraints + Garbage Collection——强调自动修复漂移
4. **Context Rot**：token 增加 → 召回能力下降，因此 Context Engineering 要策展最小可行高信号集

---

## [2026-04-13 13:10] ingest | 深度解析 OpenClaw 在 Prompt/Context/Harness 三个维度中的设计哲学与实践

**来源**: https://mp.weixin.qq.com/s/JycTfNd7EnmWCnJK-QCf0Q
**作者**: 飞樰（阿里云开发者）
**深度**: 源码分析级

**操作**:
- 创建来源摘要：[[sources/openclaw-prompt-context-harness|深度解析 OpenClaw Prompt/Context/Harness 设计哲学]]
- 大幅更新实体页面：[[entities/openclaw|OpenClaw]]（来源数 3→4，新增：System Prompt 23 模块组装、上下文压缩算法、精细化修剪、Harness 实践含 7 Hook + 三层沙箱 + Harness vs Workflow）
- **Craft 增量更新**：
  - 更新 [[craft/concepts/agent-memory|Agent Memory 概念卡片]]：补充 OpenClaw 时间衰减公式（半衰期 30 天）、Memory Flush 隐式写入、200 行截断、BM25+向量双路召回
  - 更新 [[craft/concepts/skill|Skill 概念卡片]]：补充 OpenClaw Skills 安全隐患（可执行脚本植入风险）和 ClawHub 鉴权
  - 更新 [[craft/concepts/harness|Harness 概念卡片]]：补充 OpenClaw 7 种 Hook 全生命周期钩子、三层安全沙箱、Harness vs Workflow 辨析（主导权归属）
- 更新 [[index.md]]（来源 13→14）
- 新增交叉引用 6 处

**关键洞察**:
1. **System Prompt 是积木而非文本**：23 个模块按固定顺序拼装，三种 PromptMode 适配不同场景。极简主义措辞（"Quality > quantity"）节省 Context Window
2. **上下文压缩的分块算法**：40% chunk ratio + 三级摘要降级 + 5 分钟超时 + 写锁。这是 Context Engineering 从理论到工程的关键细节
3. **Harness vs Workflow 的核心区别是主导权**：Workflow 中模型是节点（人主导），Harness 中模型自主规划（AI 主导但受约束）。模型越强，Harness 越优
4. **OpenClaw 记忆的时间衰减**：半衰期 30 天的指数衰减，配合 MEMORY.md 不衰减的"常青"层，模拟人类"核心事实+渐忘细节"的记忆结构
5. **Skills 安全是开放生态的根本张力**：可执行脚本 = 能力无限 + 安全风险，ClawHub 鉴权是当前答案，但问题远未解决

---

## [2026-04-13 13:06] ingest | 5 Agent Design Patterns Every Developer Needs to Know in 2026

**来源**: https://dev.to/ljhao/5-agent-design-patterns-every-developer-needs-to-know-in-2026-17d8
**作者**: PrivOcto
**深度**: 概览级

**操作**:
- 创建来源摘要：[[sources/agent-design-patterns-2026|5 Agent Design Patterns (2026)]]
- 创建实体页面 3 个：
  - [[entities/react-pattern|ReAct Pattern]]（Thought → Action → Observation 循环）
  - [[entities/plan-and-execute|Plan-and-Execute]]（规划与执行分离，92% 完成率）
  - [[entities/reflection-pattern|Reflection Pattern]]（自我反思，HumanEval 91%）
- 创建主题页面：[[topics/agent-design-patterns|Agent 设计模式]]（五种模式对比框架 + 选择指南）
- 更新 [[index.md]]（来源 12→13，实体 12→15，主题 3→4）
- **Craft 增量更新**：无直接命中。建议后续考虑新增概念卡片：ReAct、Plan-and-Execute、Reflection（当前 craft 侧重 Harness/Memory/Skill，缺少执行模式层面的概念卡片）
- 新增交叉引用 8 处

**关键洞察**:
1. 五种模式不互斥而是可组合：ReAct + Tool Use 是最基础组合，Plan-and-Execute + Multi-Agent 支持复杂编排
2. 从 Harness Engineering 视角看，这些设计模式本质上就是不同的 Harness 架构
3. 文章质量偏科普级，数据多为二三手引用，但提供的模式选择框架有实用价值

---

## [2026-04-13 13:05] ingest | How Does OpenClaw Work? A Beginner's Guide

**来源**: https://dev.to/ljhao/how-does-openclaw-work-a-beginners-guide-21cj
**作者**: PrivOcto

**操作**:
- 创建来源摘要：[[sources/openclaw-beginners-guide|How Does OpenClaw Work?]]
- 更新实体页面：[[entities/openclaw|OpenClaw]]（来源数 2→3，大幅扩充：四层架构、本地优先哲学、记忆系统、Skills 生态）
- **Craft 增量更新**：
  - 更新 [[craft/concepts/skill|Skill 概念卡片]]：补充 OpenClaw "安装即用" vs Hermes "积累即学"的对比例子
  - 更新 [[craft/concepts/agent-memory|Agent Memory 概念卡片]]：补充 OpenClaw "文件即记忆"模式，指出与 LangSmith Agent Builder 独立收敛到相同结构
- 更新 [[index.md]]
- 新增交叉引用 4 处

**关键洞察**:
1. **OpenClaw 四层架构**：Gateway（WebSocket）→ Sessions/Memory → Provider/Model → Plugins/Tools。比之前只了解多 Agent 层面更完整
2. **"文件即记忆"收敛**：OpenClaw 和 LangSmith Agent Builder 独立采用了几乎相同的 Markdown 文件记忆结构（MEMORY.md + 日期文件），这可能是这类系统的自然均衡点
3. **两种 Skill 哲学**：OpenClaw 的"安装即用"（ClawHub 分发）vs Hermes 的"积累即学"（trace 提炼），对应不同的 Agent 进化路径

---

## [2026-04-13 12:15] craft | v3 创作层改造实施

**概念讲解卡片（新增 2 个，总计 7 个）**：
- 创建 [[craft/concepts/hook-mechanism]] - Hook 机制概念卡片
- 创建 [[craft/concepts/meta-harness]] - Meta-Harness 概念卡片

**设计笔记（新增 3 个，总计 4 个）**：
- 创建 [[craft/design-notes/memos-triple-dedup]] - MemOS 三层去重设计
- 创建 [[craft/design-notes/hermes-subagent-sync-blocking]] - 子 Agent 同步阻塞设计
- 创建 [[craft/design-notes/skill-auto-generation-quality]] - Skill 质量双关卡设计

**认知缺口（新增 3 个）**：
- 创建 [[craft/gaps/rag-is-not-memory]] - RAG ≠ Agent Memory
- 创建 [[craft/gaps/prompt-is-not-harness]] - Prompt ≠ Harness
- 创建 [[craft/gaps/skill-is-not-tool]] - Skill ≠ Tool

**视角库（新增 2 个）**：
- 创建 [[craft/perspectives/memory-autonomy-vs-system-enforcement]] - 记忆自治 vs 系统强制
- 创建 [[craft/perspectives/constraint-vs-unbounded-design]] - 约束式 vs 无界式设计

**架构更新**：
- 更新 AGENTS.md：三层架构 → 四层架构，增加 Craft 工作流定义和页面格式约定
- 更新 index.md：增加 craft 创作层索引
- 创建 gaps/、perspectives/、drafts/ 目录

## [2026-04-13 09:44] ingest | 两篇新文章摄入

**来源**: 2 篇新文章
**作者**: Justin Young (Anthropic), Mr. Ånand

**操作**:
- 创建来源摘要：
  - [[sources/long-running-agents|Effective Harnesses for Long-Running Agents]]
  - [[sources/hermes-agent-inside|Inside Hermes Agent]]
- 更新实体页面：
  - [[entities/harness-engineering|Harness Engineering]]（新增长时运行实践）
  - [[entities/hermes|Hermes]]（大幅更新：四层记忆、学习循环、Gateway 架构）
- 更新主题页面：
  - [[topics/multi-agent-collaboration|多 Agent 协作]]（新增单 Agent vs 多 Agent 哲学对比）
- 更新 [[index.md]]
- 新增交叉引用 10 处

**关键洞察**:
1. **长时运行 Agent**：两阶段方案（Initializer + Coding Agent）、增量推进、会话启动例程
2. **Hermes 四层记忆**：Prompt Memory（始终在线）→ Session Search（按需检索）→ Skills（渐进加载）→ Honcho（用户建模）
3. **学习循环**：Periodic Nudge → Skill Creation → Skill Improvement → Session Search
4. **单 Agent vs 多 Agent**：Hermes 核心是单 Agent 自我改进，多 Agent 是可选能力；OpenClaw 核心是多 Agent 协作编排

---

## [2026-04-12 19:35] ingest | 五篇新文章批量摄入（第三批）

**来源**: 5 篇新文章
**作者**: Lewis爆肝战神, servasyy_ai, @nash_su, 宝玉

**操作**:
- 创建来源摘要：
  - [[sources/harness-engineering-business-case|不懂 Harness Engineering，你所谓的 AI 转型只是在浪费预算]]
  - [[sources/claude-code-to-web-api|从 Claude Code 到 Web API]]
  - [[sources/hierarchy-to-intelligence|从层级到智能]]
  - [[sources/codex-team-builds-with-codex|Codex 团队如何用自己的产品构建产品]]
  - [[sources/cli-comeback-ai-agents|飞书 CLI 开源了]]
- 创建实体页面：
  - [[entities/lewis-kuo|Lewis爆肝战神]]（人物）
  - [[entities/nash-su|@nash_su]]（人物）
  - [[entities/baoyu|宝玉]]（人物）
- 更新实体页面：
  - [[entities/servasyy-ai|servasyy_ai]]（来源数 2→3，新增 Skills 体系贡献）
  - [[entities/skill-system|Skill 系统]]（来源数 2→3，新增 CLI/MCP 对比）
- 更新 [[index.md]]
- 新增交叉引用 15 处

**关键洞察**:
1. **Harness 商业化案例**：OpenAI 100 万行代码、Anthropic 生成器+评估器、VergeX 四核心模块
2. **Skills 本质**：菜谱（定义工作流程），Agent 是项目经理，Subagents 是临时工
3. **AI 替代层级**：Block 四层架构（能力→世界模型→智能层→界面），人在边缘
4. **Codex 团队哲学**：10 个要点 Spec、不做中期路线图、PM 是填空岗位
5. **CLI 回归**：自描述、文本交互、dry-run 安全网

---

## [2026-04-12 14:20] ingest | 三篇新文章批量摄入（第二批）

**来源**: 3 篇新文章
**作者**: Harrison Chase, @eng_khairallah1

**操作**:
- 创建来源摘要：
  - [[sources/coding-agents-reshape-epd|Coding Agents 重塑 EPD]]
  - [[sources/building-claude-skills-guide|如何构建真正有效的 Claude Skills]]
  - [[sources/agent-builder-memory-system|Agent Builder 的记忆系统]]
- 创建实体页面：
  - [[entities/eng-khairallah1|@eng_khairallah1]]（人物）
- 更新实体页面：
  - [[entities/hwchase17|Harrison Chase]]（来源数 2→5）
  - [[entities/skill-system|Skill 系统]]（新增来源引用）
- 更新 [[index.md]]
- 新增交叉引用 12 处

**关键洞察**:
1. **Coding Agents 影响**：瓶颈从实现转移到评审，Builder vs Reviewer 分化，通才价值提升
2. **Skill 五组件**：YAML Trigger + Overview + Workflow + Output Format + Examples
3. **记忆系统设计**：文件即记忆，热路径更新 + 后台反思，专用 Agent 比通用 Agent 更需要记忆

---

## [2026-04-12 12:20] ingest | 三篇新文章批量摄入

**来源**: 3 篇新文章
**作者**: @HiTw93, Akshay Pachaar, Harrison Chase

**操作**:
- 创建来源摘要：
  - [[sources/llm-training-pipeline|大模型训练：原理、路径与新实践]]
  - [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]]
  - [[sources/harness-memory-lockin|Your Harness, Your Memory]]
- 创建实体页面：
  - [[entities/HiTw93|@HiTw93]]（人物）
  - [[entities/akshay-pachaar|Akshay Pachaar]]（人物）
- 更新实体页面：
  - [[entities/harness-engineering|Harness Engineering]]（新增 2 个来源引用）
  - [[entities/meta-harness|Meta-Harness]]（新增 1 个来源引用）
  - [[entities/hwchase17|Harrison Chase]]（新增 1 个来源引用）
- 更新主题页面：
  - [[topics/agent-architecture|Agent 架构]]（新增 12 组件、Memory 绑定）
- 更新 [[index.md]]
- 新增交叉引用 18 处

**关键洞察**:
1. **大模型训练链路**：预训练只是底座，后训练、评测、奖励、Agent 训练、蒸馏才是差距来源
2. **Harness 12 组件**：Orchestration Loop + Tools + Memory + Context Management + ...
3. **Memory 锁定**：如果你不拥有 Harness，你就不拥有 Memory — 三种锁定程度
4. **Meta-Harness 效果**：同一个底模，只改 Harness，可能拉出 6x 性能差距

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
