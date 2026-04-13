# LLM Wiki 操作日志

---

## [2026-04-13 21:15] craft/draft | OpenClaw vs. Hermes Agent：两种 AI Agent 的生存哲学

第七篇文章草稿，写入 `wiki/craft/drafts/openclaw-vs-hermes-agent.md`。约 2,500 中文字。

**文章结构**（九节）：
1. 不是选哪个更好的问题——两条不同的技术路线
2. 数据对比表——Stars/平台/Skills/安全/成本/哲学
3. OpenClaw：连接一切——Android 类比、50+ 平台、ClawHub 生态、交响乐团协作
4. 安全：连接广度的结构性代价——9 CVE、135K 暴露、12-20% 恶意 Skill
5. Hermes：先学会一件事——学习循环四步、四层记忆、约束催生架构、安全副产品
6. 真正的分歧：时间复利——短期广度赢、长期深度赢
7. 多 Agent 协作：交响乐团 vs 总承包商——Steer/超时/Token 效率/隔离性
8. 记忆系统：策展 vs 堆叠——扁平文件 vs 分层策展
9. 三个判断：融合趋势但分化不消失、安全第一筛选器、Agent 活多久决定选什么

**使用素材**：hermes/openclaw 实体、cognitive-depth-vs-connectivity 视角、agent-memory 概念、hermes-memory-limit/openclaw-compaction 设计笔记、rag-is-not-memory 认知缺口、agent-security 概念、7 篇来源摘要

---

## [2026-04-13 21:02] craft/draft | 不懂 Harness Engineering，你的 AI 转型只是在浪费预算

第六篇文章草稿，写入 `wiki/craft/drafts/harness-engineering-budget-waste.md`。约 3,000 中文字。

**文章结构**（九节）：
1. 钱都花在哪了——95% 企业 AI 试点失败的典型路径
2. 什么是 Harness——马匹隐喻 + Vivek Trivedy 定义 + Claude Code 512K 行数据
3. 数据：Harness 差距有多大——Princeton 6x、LangChain Top 5、Hashline 10x、Codex 100 万行、Stripe 1000 PR
4. 四动词框架——Constrain/Inform/Verify/Correct 闭环 + 纠正 Fowler 偏见
5. Workflow ≠ Harness——主导权差异 + 修 Bug 对比场景
6. 三层嵌套——Prompt ⊂ Context ⊂ Harness + 投资策略
7. 三级落地框架——Level 1（1-2 小时）→ Level 2（1-2 天）→ Level 3（1-2 周）
8. 五个常见错误——过度工程化、静态配置、忽视文档、无反馈、只给人读
9. 真正的护城河——轨迹积累是复利

**使用素材**：harness 概念卡片、四动词框架、context-engineering、agents-md-standard（概念）+ prompt-is-not-harness、harness-is-not-workflow、framework-is-not-harness（认知缺口）+ two-families 设计笔记 + constraint-vs-unbounded 视角 + 6 篇来源摘要 + 已有草稿参考

---

## [2026-04-13 20:57] craft | 起草 Hermes 文章

**触发**：用户指令"请写一篇关于 Hermes 的文章，可以主动发现新素材"

- 联网搜索 Hermes 最新动态（v0.8.0 release notes、GitHub Stars 64K+）
- 综合 7 篇来源摘要、12 张概念卡片、8 张设计笔记、多张认知缺口和视角卡片
- 创建 [[craft/drafts/hermes-the-agent-that-learns|Hermes：一个会从错误中学习的 Agent]]
- 约 2,000 字，以学习循环为核心叙事线
- 覆盖：RAG ≠ Memory 切入 → 四层记忆 → Skill 自生成 → Profile 隔离 → 行业坐标 → 三个判断

---

## [2026-04-13 20:15] craft | 盘点建议全量执行（11 项）

**触发**：用户指令"全做"，基于系统盘点报告的全部建议

### 更新现有卡片（4 张）

1. **[[craft/concepts/agent-security|Agent 安全]]** — 补 Glasswing/Mythos 受控发布案例、第四种安全解决方式、新例子
2. **[[craft/concepts/skill|Skill]]** — 补 Hermes 118 Skills + 40% 提升 + 不跨领域迁移、ClawHub 13,000+ 和 12-20% 恶意率
3. **[[craft/gaps/agent-security-is-not-prompt-security|Agent 安全 ≠ Prompt 安全]]** — 补第三维度"能力治理"、关联新 gap 和 perspective
4. **[[craft/drafts/writing-guide|写作指南]]** — 全面重写，反映 13 概念 + 8 设计笔记 + 9 缺口 + 6 视角的完整素材库

### 新建卡片（7 张）

**概念 (2)**：
- [[craft/concepts/eval|Eval]] — 衡量改进的基准线，Harness Engineering 三支柱之一
- [[craft/concepts/execution-modes|Agent 执行模式]] — ReAct vs Plan-and-Execute

**设计笔记 (1)**：
- [[craft/design-notes/self-generation-vs-marketplace|Skill 自生成 vs 外部市场]] — 三条路径的安全/质量/进化权衡

**认知缺口 (3)**：
- [[craft/gaps/agent-security-is-not-vuln-hunting|Agent 安全 ≠ 漏洞发现安全]] — 三象限安全模型
- [[craft/gaps/agent-vs-harness-boundary|Agent 层 vs Harness 层边界]] — Chase vs Foster 两种划分
- [[craft/gaps/skill-two-paths|Skill 不止一条路径]] — 积累提炼 vs 市场安装 vs 规范注入

**视角 (1)**：
- [[craft/perspectives/controlled-vs-open-release|控制发布 vs 开放发布]] — Glasswing 模式的争论

### 更新后规模

- 来源：33 | 实体：25 | 主题：4 | 综合：1
- Craft：**13 概念 + 8 设计笔记 + 9 认知缺口 + 6 视角 + 1 草稿 = 37 个文件**（+6）

---

## [2026-04-13 19:55] ingest | 第四批摄入（4 篇）

### 新增来源摘要（4 篇）

1. **[[sources/hermes-agent-tutorial|Hermes Agent Tutorial]]**
   - v0.8.0 实操教程：一键安装、模型配置（OpenRouter/Anthropic/Ollama）、Gateway、Cron、MCP
   - 更新 [[entities/hermes]]（安装与运维段）、[[entities/skill-system]]

2. **[[sources/openclaw-vs-hermes-race|OpenClaw vs Hermes: The Race]]**
   - The New Stack 深度对比：常驻 Agent 新品类、生态优先 vs 学习优先
   - OpenClaw 安全危机：ClawHavoc 攻击（341/2,857 恶意 Skills）、CVE-2026-25253（CVSS 8.8）
   - 更新 [[entities/hermes]]（行业定位段）、[[entities/openclaw]]（定义段 + 安全危机段）

3. **[[sources/project-glasswing-mythos|Project Glasswing]]** ⭐ 全新领域
   - Claude Mythos 自主发现数千零日漏洞，Firefox 147 exploit 90x 提升
   - 首个"太危险不公开发布"的模型，12 家合作组织防御性安全
   - 新建 [[entities/claude-mythos]]、[[entities/project-glasswing]]

4. **[[sources/hermes-agent-complete-guide|Hermes Agent Complete Guide]]**
   - 64.2K stars（10 天从 22K 到 64K）、118 Skills、定价模型、诚实局限性
   - 73% API 调用为固定开销、Skill 不跨领域迁移、Honcho 默认关闭
   - 更新 [[entities/hermes]]（Benchmark/成本/局限/安全段）、[[entities/skill-system]]

### 实体更新

- **Hermes**：来源 4→7，新增安装运维、行业定位、Benchmark 与成本、已知局限、安全记录段
- **OpenClaw**：来源 5→6，更新定义（345K stars、基金会治理），新增 ClawHub 安全危机段
- **Skill 系统**：来源 4→6

### 新增实体（2 个）

- **Claude Mythos** — 首个自主发现零日漏洞的 AI 模型
- **Project Glasswing** — Anthropic 防御性网络安全项目

### 索引更新

- index.md 来源数 29→33，实体数 23→25

### Craft 增量检查

- 新素材涉及已有 Craft 卡片：
  - agent-security 概念 → Project Glasswing 提供了"模型太危险不公开发布"的极端案例（建议补充）
  - skill 概念 → 118 Skills 具体数据 + 不跨领域迁移的局限性（建议更新）
- 建议新增认知缺口："Agent 安全 ≠ 漏洞发现安全"（Mythos 案例）
- 建议新增视角："控制发布 vs 开放发布"（Glasswing 模式的争议）

---

## [2026-04-13 19:55] craft | 系统盘点 + 全面更新（级别三）

**触发**：用户指令"盘一下 craft" → "全做"

### 索引修正
- index.md 来源数 24→29（补齐 5 个遗漏）
- index.md 实体数 15→23（补齐 8 个遗漏：3 人物 + 5 概念/工具统计修正）
- overview.md 从"创建阶段"更新为"活跃构建阶段"，补充规模统计

### 增量更新（5 张四星概念卡片）
- [[craft/concepts/context-engineering]] - 补充 40% 上下文利用率阈值 + OpenClaw 时间衰减记忆例子
- [[craft/concepts/hook-mechanism]] - 补充 OpenClaw 7 种 Hook 钩子
- [[craft/concepts/meta-harness]] - 补充 Hashline 10x 数据
- [[craft/concepts/three-layer-learning]] - 补充 Hashline + 两大流派视角
- [[craft/concepts/trace]] - 补充 Hashline 发现过程作为 trace 驱动改进例子

### 新建卡片（10 张）
**概念** (3)：
- [[craft/concepts/agents-md-standard]] - AGENTS.md 跨工具标准
- [[craft/concepts/four-verb-framework]] - Constrain/Inform/Verify/Correct 四动词
- [[craft/concepts/agent-security]] - Agent 安全 ≠ 模型安全

**设计笔记** (2)：
- [[craft/design-notes/two-families-reasoning-vs-environment]] - Reasoning-First vs Environment-First
- [[craft/design-notes/openclaw-time-decay-memory]] - 时间衰减函数 + 双层记忆分层

**视角** (3)：
- [[craft/perspectives/open-standard-vs-deep-integration]] - AGENTS.md vs CLAUDE.md
- [[craft/perspectives/cognitive-depth-vs-connectivity]] - Hermes vs OpenClaw 核心哲学
- [[craft/perspectives/reasoning-first-vs-environment-first]] - 信任模型 vs 信任环境

**认知缺口** (2)：
- [[craft/gaps/framework-is-not-harness]] - Framework ≠ Harness
- [[craft/gaps/agent-security-is-not-prompt-security]] - Agent 安全 ≠ Prompt 安全

### 更新后规模
- 来源：29 | 实体：23 | 主题：4 | 综合：1
- Craft：11 概念 + 7 设计笔记 + 6 认知缺口 + 5 视角 + 2 草稿

---

## [2026-04-13 19:35] ingest | Hermes vs OpenClaw 2026 (NxCode)

**来源**: https://www.nxcode.io/resources/news/hermes-agent-vs-openclaw-2026-which-ai-agent-to-choose
**作者**: NxCode Team
**深度**: 全维度对比

**操作**:
- 创建来源摘要：[[sources/hermes-vs-openclaw-2026|Hermes vs OpenClaw 2026]]
- 更新实体页面：[[entities/hermes|Hermes]]（来源 3→4）
- 更新实体页面：[[entities/openclaw|OpenClaw]]（来源 4→5）
- 更新实体页面：[[entities/skill-system|Skill 系统]]（来源 3→4）
- 更新 [[index.md]]（来源 23→24）
- 新增交叉引用 3 处

**关键洞察**:
1. **两种 Agent 哲学的清晰定义**：认知深度（Hermes）vs 连接广度（OpenClaw）——不是好坏之分，而是优先级差异
2. **安全差距的根因是架构性的**：消费级本地工具 → 网络化 Agent 的信任假设不成立（这是通用 Agent 安全教训）
3. **ClawHub 12-20% 恶意 Skill 率**是 Skill 生态安全的最强量化数据——比之前 craft 中的定性讨论更有说服力
4. **自我改进 vs 静态 Skill 的长期复利**：运行两个月的 Hermes > 全新安装——知识积累是 Agent 的核心壁垒

---

## [2026-04-13 19:25] ingest | AGENTS.md + CLAUDE.md Glossary（两篇合并处理）

**来源**: 
- https://latentpatterns.com/glossary/agents-md
- https://latentpatterns.com/glossary/claude-md
**作者**: Latent Patterns
**深度**: 术语定义

**操作**:
- 创建来源摘要：[[sources/agents-md-glossary|AGENTS.md — Glossary]]
- 创建来源摘要：[[sources/claude-md-glossary|CLAUDE.md — Glossary]]
- 创建实体页面：[[entities/agents-md|AGENTS.md]]（发现层级、跨工具兼容、AAIF 治理）
- 创建实体页面：[[entities/claude-md|CLAUDE.md]]（六层记忆层级、Scoped Rules、按需加载）
- 更新 [[index.md]]（来源 21→23，实体 16→18）
- 新增交叉引用 4 处

**关键洞察**:
1. **AGENTS.md vs CLAUDE.md 不是竞争关系**——AGENTS.md 做跨工具底线，CLAUDE.md 做 Claude Code 深度集成，symlink 桥接
2. **CLAUDE.md 六层记忆层级**是目前最精细的 Agent 指令系统——从组织策略到自动学习，每层有明确的管理权限和加载时机
3. **按需加载**是精妙设计——子目录 CLAUDE.md 不占基础上下文，保持精简
4. **安全面是共同挑战**——恶意仓库的指令文件 = prompt injection 载体

---

## [2026-04-13 19:15] ingest | Agent Frameworks, Runtimes, and Harnesses (Harrison Chase)

**来源**: https://blog.langchain.com/agent-frameworks-runtimes-and-harnesses-oh-my/
**作者**: Harrison Chase
**深度**: 分类定义

**操作**:
- 创建来源摘要：[[sources/frameworks-runtimes-harnesses|Agent Frameworks, Runtimes, and Harnesses]]
- 更新实体页面：[[entities/hwchase17|Harrison Chase]]（来源 1→6，新增：三层分类学观点）
- 更新 [[index.md]]（来源 20→21）
- 新增交叉引用 2 处

**关键洞察**:
1. **Framework/Runtime/Harness 分类学**是从"工具选择"角度的分层——与 Foster 从"系统架构"角度的分层互补
2. **Harness = "general purpose version of Claude Code"**——高层、开箱即用、有主见
3. **所有 Coding CLI 某种意义上都是 Agent Harness**——这扩展了 Harness 概念的外延

---

## [2026-04-13 19:05] ingest | Why Agent Harness Architecture is Important (Tom Foster)

**来源**: https://contextua.dev/why-agent-harness-architecture-is-important/
**作者**: Tom Foster
**深度**: 深度对比分析

**操作**:
- 创建来源摘要：[[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]]
- 更新实体页面：[[entities/tom-foster|Tom Foster]]（来源 1→2，新增：两大流派分类）
- 更新实体页面：[[entities/harness-engineering|Harness Engineering]]（来源 5→6，新增：两大设计流派、40% 上下文阈值、Hashline 10x 数据）
- 更新主题页面：[[topics/agent-architecture|Agent 架构]]（来源 5→6，新增来源引用）
- 更新 [[index.md]]（来源 19→20）
- 新增交叉引用 4 处

**关键洞察**:
1. **两大流派**：Reasoning-First（信任模型）vs Environment-First（投资环境），不是好坏之分而是哲学差异
2. **Hashline 是最强经验证据**：10x 改进（6.7%→68.3%），仅改编辑工具——比 Princeton 6x 数据更针对单一 Harness 组件
3. **40% 上下文利用率阈值**：超过 40% 后可靠性下降，解释了为什么 aggressive 压缩有必要
4. **MCP Code Mode 减 98.7% 工具上下文**——Claude Code 的闭源优势
5. **Harness 是新的 Build System**——类比 CI/CD 从"已解决"到"关键基础设施"的演变

---

## [2026-04-13 18:55] ingest | Model, Agent, Harness: Three Layers (Tom Foster)

**来源**: https://contextua.dev/model-agent-harness-the-three-layers-most-people-collapse-into-one/
**作者**: Tom Foster
**深度**: 架构定义

**操作**:
- 创建来源摘要：[[sources/three-layers-model-agent-harness|Model, Agent, Harness: Three Layers]]
- 创建实体页面：[[entities/tom-foster|Tom Foster]]（人物）
- 更新主题页面：[[topics/agent-architecture|Agent 架构]]（来源 4→5，新增：Foster 版三层模型、Harness vs Framework 区分、诊断框架）
- 更新 [[index.md]]（来源 18→19，实体 15→16）
- 新增交叉引用 4 处

**关键洞察**:
1. **两种三层划分的差异**：Chase（Model/Harness/Context）把推理循环归入 Harness；Foster（Model/Agent/Harness）把推理循环独立为 Agent 层。Chase 版利于持续学习分析，Foster 版利于故障诊断
2. **Harness vs Framework**是被忽略的重要区分——Framework 是积木，Harness 是建好的房子
3. **"大部分日常 Agent 失败是 Layer 3 问题"**——这个诊断经验和 NxCode 的"模型是商品、Harness 是护城河"形成呼应
4. **Harness 不是纯被动基础设施**：它决定上下文注入、工具可用性、循环终止——是"编排者"

**建议新增**:
- 考虑新增 `craft/gaps/agent-vs-harness-boundary.md`：Chase 和 Foster 的边界划分差异可能是一个有价值的认知缺口——读者会混淆"Agent"到底指什么

---

## [2026-04-13 18:45] ingest | Harness Engineering: Complete Guide (NxCode)

**来源**: https://www.nxcode.io/resources/news/harness-engineering-complete-guide-ai-agent-codex-2026
**作者**: NxCode Team
**深度**: 系统化综合指南

**操作**:
- 创建来源摘要：[[sources/harness-engineering-complete-guide|Harness Engineering: Complete Guide]]
- 更新实体页面：[[entities/harness-engineering|Harness Engineering]]（来源 4→5，新增：马匹隐喻、四动词框架、熵管理支柱、三级落地框架、LangChain/OpenAI/Stripe 量化数据、约束反直觉效应、可撕裂性、回答 2 个开放问题）
- 更新主题页面：[[topics/agent-architecture|Agent 架构]]（来源 3→4，新增来源引用）
- **Craft 增量更新**：
  - 更新 [[craft/concepts/harness|Harness 概念卡片]]：补充马匹隐喻展开、约束反直觉效应（解空间约束→token 收敛更快）
- 更新 [[index.md]]（来源 17→18）
- 新增交叉引用 4 处

**关键洞察**:
1. **四动词框架**（Constrain/Inform/Verify/Correct）是目前最精炼的 Harness 定义——比三支柱更抽象、更通用
2. **三级落地框架**解答了"不同规模团队怎么起步"的实际问题（1-2 小时 → 1-2 天 → 1-2 周）
3. **熵管理**是被低估的第三大支柱——AI 代码库的文档漂移、命名分裂、死代码比人类代码库更严重
4. **"可撕裂性"**是新视角：好的 Harness 组件应该能随模型能力提升而移除，过度工程化反而是负担
5. **LangChain 仅改 Harness 从 Top 30 → Top 5**是目前最直接的量化证据（比 Princeton 6x 数据更有叙事力）

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
