# LLM Wiki 索引

最后更新：2026-04-13

---

## 概览

- [[overview|知识库概览]]

---

## 来源摘要 (33)

- [[sources/hermes-agent-complete-guide|Hermes Agent Complete Guide]] - 64K stars、118 Skills、定价模型、局限性、哲学定位"Agent as a mind to develop"
- [[sources/hermes-agent-tutorial|Hermes Agent Tutorial]] - v0.8.0 实操教程：一键安装、模型配置、Gateway、Cron 调度器、MCP 集成
- [[sources/openclaw-vs-hermes-race|OpenClaw vs Hermes: The Race]] - The New Stack 深度对比：生态优先 vs 学习优先、常驻 Agent 新品类、安全危机详情
- [[sources/project-glasswing-mythos|Project Glasswing]] - Claude Mythos 自主发现零日漏洞、90x exploit 开发提升、12 家合作组织防御性安全
- [[sources/hermes-vs-openclaw-2026|Hermes vs OpenClaw 2026]] - 全维度对比：认知深度 vs 连接广度、安全差距（9 CVE vs 0）、ClawHub 12-20% 恶意率、自我改进 vs 静态 Skill、迁移趋势
- [[sources/agents-md-glossary|AGENTS.md — Glossary]] - 跨工具开放标准指令文件，发现层级、标准化历程（AAIF/Linux Foundation）、symlink 桥接
- [[sources/claude-md-glossary|CLAUDE.md — Glossary]] - Claude Code 六层记忆层级（Managed Policy → Auto Memory）、Scoped Rules、按需加载
- [[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]] - 五款 Harness 对比（Claude Code/Codex/Gemini/Mistral Vibe/OpenCode）、Reasoning-First vs Environment-First 两大流派、Hashline 10x
- [[sources/three-layers-model-agent-harness|Model, Agent, Harness: Three Layers]] - 三层架构精确定义（Model=统计引擎/Agent=推理循环/Harness=执行平台）、Harness vs Framework、诊断框架
- [[sources/harness-engineering-complete-guide|Harness Engineering: Complete Guide]] - 马匹隐喻、四动词框架（Constrain/Inform/Verify/Correct）、三大支柱、三级落地框架、OpenAI 百万行/Stripe 千 PR/LangChain Top 5 案例
- [[sources/frameworks-runtimes-harnesses|Agent Frameworks, Runtimes, and Harnesses]] - Harrison Chase 定义三层分类学：Framework（抽象）→ Runtime（生产基础设施）→ Harness（开箱即用）
- [[sources/openspec-superpowers-harness|OpenSpec、Superpowers 和 Harness：三层拼图]] - 多 Agent 开发的三层工程基础设施：规范（OpenSpec）→ 纪律（Superpowers）→ 协作（Harness）
- [[sources/hermes-multi-agent-team|How to Build a Multi-Agent Team in Hermes]] - Profile 持久隔离 + 四角色团队模型 + SOUL.md/AGENTS.md 分离原则
- [[sources/prompt-vs-context-vs-harness|Prompt vs Context vs Harness Engineering]] - 三层嵌套层级关系（Prompt ⊂ Context ⊂ Harness）+ Princeton 64% 数据 + Boeckeler 三支柱
- [[sources/openclaw-prompt-context-harness|深度解析 OpenClaw Prompt/Context/Harness 设计哲学]] - 源码级分析：23 模块 Prompt 组装、压缩算法、Hook 系统、三层沙箱
- [[sources/agent-design-patterns-2026|5 Agent Design Patterns (2026)]] - 五种 Agent 设计模式综合概览：ReAct、Plan-and-Execute、Multi-Agent、Reflection、Tool Use
- [[sources/openclaw-beginners-guide|How Does OpenClaw Work?]] - OpenClaw 四层架构入门指南：Gateway、Sessions/Memory、Provider/Model、Plugins/Tools
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
- [[sources/claude-code-to-web-api|Claude Code: From CLI to Web API]] - Claude Code 从命令行到 Web API 的演进路径
- [[sources/cli-comeback-ai-agents|The CLI Comeback: AI Agents]] - AI Agent 时代 CLI 的回归与重新定义
- [[sources/codex-team-builds-with-codex|How OpenAI's Codex Team Builds with Codex]] - OpenAI Codex 团队自身使用 Codex 的实践
- [[sources/harness-engineering-business-case|The Business Case for Harness Engineering]] - Harness Engineering 的商业价值论证
- [[sources/hierarchy-to-intelligence|From Hierarchy to Intelligence]] - 从层级到智能的组织演化视角

---

## 实体页面 (25)

### 人物
- [[entities/hwchase17|Harrison Chase]] - LangChain 创始人，Agent 架构领域活跃思考者，5 篇核心文章作者
- [[entities/tom-foster|Tom Foster]] - Contextua.dev 作者，Model/Agent/Harness 三层精确定义
- [[entities/servasyy-ai|servasyy_ai]] - 多 Agent 协作机制研究者，Harness Engineering 实践者
- [[entities/HiTw93|@HiTw93]] - 大模型训练链路研究者，《你不知道》系列文章作者
- [[entities/akshay-pachaar|Akshay Pachaar]] - Agent Harness 实践者，12 组件拆解作者
- [[entities/eng-khairallah1|@eng_khairallah1]] - Claude Skills 设计专家
- [[entities/baoyu|宝玉]] - AI 技术博主，翻译与内容创作者
- [[entities/lewis-kuo|Lewis Kuo]] - Agent 系统设计研究者
- [[entities/nash-su|Nash Su]] - Agent 架构实践者

### 组织
*暂无*

### 概念
- [[entities/agents-md|AGENTS.md]] - 跨工具开放标准指令文件（AAIF/Linux Foundation）
- [[entities/claude-md|CLAUDE.md]] - Claude Code 专有配置文件，六层记忆层级
- [[entities/soul-md|SOUL.md]] - Agent 层面的持久记忆模式，定义 Agent 身份并持续积累经验
- [[entities/catastrophic-forgetting|灾难性遗忘]] - 模型层持续学习的核心挑战，更新权重导致旧能力退化
- [[entities/meta-harness|Meta-Harness]] - Harness 层持续学习方法，用 Agent 分析 trace 改进 Harness
- [[entities/steer-mechanism|Steer 机制]] - 运行时引导能力，向运行中的子 agent 发送消息调整方向
- [[entities/harness-engineering|Harness Engineering]] - 构建可靠 Agent 系统的方法论框架，三根支柱：记忆治理、架构约束、评估闭环
- [[entities/hook-mechanism|Hook 机制]] - Agent 生命周期的事件拦截器，实现系统级强制执行
- [[entities/skill-system|Skill 系统]] - 将任务轨迹提炼为可复用经验的机制
- [[entities/react-pattern|ReAct Pattern]] - Thought → Action → Observation 循环，透明可审计的推理-行动模式
- [[entities/plan-and-execute|Plan-and-Execute]] - 规划与执行分离，92% 任务完成率，3.6x 加速
- [[entities/reflection-pattern|Reflection Pattern]] - Generate → Reflect → Refine 自我反思循环，HumanEval 91%

### 工具
- [[entities/hermes|Hermes]] - 自我改进 AI Agent，64K+ stars，学习循环 + 持久化记忆 + Gateway
- [[entities/openclaw|OpenClaw]] - 生态优先 AI Agent 平台，345K+ stars，50+ messaging 集成
- [[entities/claude-mythos|Claude Mythos]] - Anthropic 模型，自主发现零日漏洞，90x exploit 提升，未公开发布
- [[entities/project-glasswing|Project Glasswing]] - Anthropic 防御性网络安全项目，12 家合作组织

---

## 主题页面 (4)

- [[topics/agent-architecture|Agent 架构]] - AI Agent 的三层架构模式：Model → Harness → Context
- [[topics/agent-design-patterns|Agent 设计模式]] - 五种核心模式：ReAct、Plan-and-Execute、Multi-Agent、Reflection、Tool Use
- [[topics/continual-learning|持续学习]] - Agent 系统在三层架构上的持续改进能力
- [[topics/multi-agent-collaboration|多 Agent 协作]] - 同步阻塞 vs 异步编排两种协作模式

---

## 综合分析 (1)

- [[synthesis/ai-agent-systems-overview|AI Agent 系统架构全景图]] - 综合六篇核心文章，构建完整的 Agent 架构、持续学习、多 Agent 协作、Harness Engineering 方法论体系

---

## 创作层 (craft/)

### 概念讲解卡片 (13)
- [[craft/concepts/harness|Harness]] - Agent 的运行时系统，模型"跑"在什么上面
- [[craft/concepts/context-engineering|Context Engineering]] - 决定模型"看到什么信息"的工程学科，策展最小可行高信号 token 集
- [[craft/concepts/four-verb-framework|四动词框架]] - Constrain / Inform / Verify / Correct，Harness Engineering 的完整闭环
- [[craft/concepts/agents-md-standard|AGENTS.md 标准]] - 给 AI Agent 的跨工具通用说明书
- [[craft/concepts/agent-security|Agent 安全]] - Agent 安全 ≠ 模型安全，系统层威胁面远大于 Prompt 层（含 Glasswing 受控发布）
- [[craft/concepts/trace|Trace]] - Agent 完整执行路径的记录，所有改进的数据源
- [[craft/concepts/skill|Skill]] - 从成功和失败中提炼的结构化可复用经验（118 Skills，40% 提升，三条路径）
- [[craft/concepts/agent-memory|Agent Memory]] - Agent 积累自身经验的系统，不是 RAG
- [[craft/concepts/three-layer-learning|三层学习]] - Model / Harness / Context 三个层次的持续改进
- [[craft/concepts/hook-mechanism|Hook 机制]] - Agent 生命周期的自动拦截器，100% 捕获保障
- [[craft/concepts/meta-harness|Meta-Harness]] - 用 Agent 改进 Agent 的 Harness
- [[craft/concepts/eval|Eval]] - 衡量 Agent 系统改进是否有效的基准线，Harness Engineering 三支柱之一
- [[craft/concepts/execution-modes|Agent 执行模式]] - ReAct（边想边做）vs Plan-and-Execute（先想后做）

### 设计笔记 (8)
- [[craft/design-notes/two-families-reasoning-vs-environment|两大流派]] - Reasoning-First vs Environment-First，Harness 架构的核心分野
- [[craft/design-notes/openclaw-time-decay-memory|OpenClaw 时间衰减记忆]] - e^(-λ × 天数) 衰减函数 + MEMORY.md 分层绕过
- [[craft/design-notes/openclaw-compaction-algorithm|OpenClaw 压缩算法]] - 40% chunk ratio + 三级摘要降级 + 5 分钟超时保护
- [[craft/design-notes/hermes-memory-limit|Hermes 记忆上限]] - 为什么选择 3,575 字符？约束即特性
- [[craft/design-notes/memos-triple-dedup|MemOS 三层去重]] - 哈希→向量→LLM 的成本梯度设计
- [[craft/design-notes/hermes-subagent-sync-blocking|子 Agent 同步阻塞]] - 为什么不用异步？上下文污染是更大的问题
- [[craft/design-notes/skill-auto-generation-quality|Skill 质量双关卡]] - confidence + 质量评分缺一不可
- [[craft/design-notes/self-generation-vs-marketplace|Skill 自生成 vs 外部市场]] - 三条 Skill 获取路径的安全、质量、进化权衡

### 认知缺口 (9)
- [[craft/gaps/framework-is-not-harness|Framework ≠ Harness]] - 积木盒子 vs 搭好的房子
- [[craft/gaps/agent-security-is-not-prompt-security|Agent 安全 ≠ Prompt 安全]] - 模型说什么 vs Agent 做什么（含能力治理维度）
- [[craft/gaps/rag-is-not-memory|RAG ≠ Agent Memory]] - 查百科 vs 翻工作日志
- [[craft/gaps/prompt-is-not-harness|Prompt ≠ Harness]] - 一封邮件 vs 整个工作环境
- [[craft/gaps/harness-is-not-workflow|Harness ≠ Workflow]] - 给模型画路线图 vs 给模型建安全围栏
- [[craft/gaps/skill-is-not-tool|Skill ≠ Tool]] - 手 vs 肌肉记忆
- [[craft/gaps/agent-security-is-not-vuln-hunting|Agent 安全 ≠ 漏洞发现安全]] - 防 Agent 被攻 vs Agent 太强（三象限模型）
- [[craft/gaps/agent-vs-harness-boundary|Agent 层 vs Harness 层边界]] - Chase 版 vs Foster 版三层划分
- [[craft/gaps/skill-two-paths|Skill 不止一条路径]] - 积累提炼 vs 市场安装 vs 规范注入

### 视角 (6)
- [[craft/perspectives/open-standard-vs-deep-integration|开放标准 vs 深度集成]] - AGENTS.md 路线 vs CLAUDE.md 路线
- [[craft/perspectives/cognitive-depth-vs-connectivity|认知深度 vs 连接广度]] - Hermes 路线 vs OpenClaw 路线
- [[craft/perspectives/reasoning-first-vs-environment-first|Reasoning-First vs Environment-First]] - 信任模型 vs 信任环境
- [[craft/perspectives/memory-autonomy-vs-system-enforcement|记忆自治 vs 系统强制]] - Hermes 路径 vs MemOS 路径
- [[craft/perspectives/constraint-vs-unbounded-design|约束式 vs 无界式设计]] - 有限空间强制策展 vs 无限存储智能管理
- [[craft/perspectives/controlled-vs-open-release|控制发布 vs 开放发布]] - Glasswing 模式：能力太强时的治理争论

### 草稿 (1)
- [[craft/drafts/99-percent-is-not-prompt|一个 Agent 系统 99% 的代码都不是 Prompt]] - 从 Prompt → Context → Harness 三层嵌套论述系统工程的重要性

---

## 统计

- 来源总数：33
- 实体总数：25
- 主题总数：4
- 综合分析数：1
- 创作层：13 概念 + 8 设计笔记 + 9 认知缺口 + 6 视角 + 1 草稿
- 最后更新：2026-04-13
