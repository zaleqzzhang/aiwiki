# 写作指南：如何用 Craft 素材库写文章

## 写作路径（三步）

### 第一步：选切入角度 → 从 gaps/ 开始

认知缺口是最好的文章起点——读者觉得自己懂，其实搞混了。

当前可用的 7 个认知缺口：

| 缺口 | 一句话 | 适合的文章标题方向 |
|------|--------|-------------------|
| **RAG ≠ Agent Memory** | 查百科 ≠ 翻工作日志 | "为什么给 Agent 加了 RAG 还是不会学习" |
| **Prompt ≠ Harness** | 写封邮件 ≠ 搭工作环境 | "Prompt 工程的天花板在哪里" |
| **Skill ≠ Tool** | 手 ≠ 肌肉记忆 | "Agent 的工具和技能是两回事" |
| **Framework ≠ Harness** | 积木盒子 ≠ 搭好的房子 | "为什么用了 LangChain 还是搞不定 Agent" |
| **Agent 安全 ≠ Prompt 安全** | 模型说什么 ≠ Agent 做什么 | "Agent 安全比你想的远得多" |
| **Harness ≠ Workflow** | 给模型画路线 ≠ 给模型建围栏 | "Workflow 编排不是 Agent 架构" |
| **Agent 安全 ≠ 漏洞发现安全** | 防 Agent 被攻 ≠ Agent 太强 | "当 Agent 自己就是最大的安全威胁" |

**操作**：读一遍目标缺口卡片，找到"文章切入点"那段——那是开头的种子。

### 第二步：搭骨架 → 用 concepts/ 确定概念链

一篇好的技术文章有清晰的概念递进链。从概念卡片中选 2-4 个概念，排列成"先懂这个 → 才能懂那个"的顺序。

**当前 12 个概念卡片的概念链**：

```
Harness（基础）
├── Context Engineering（Harness 中"模型看到什么"的子集）
├── Hook 机制（Harness 的生命周期拦截器）
├── Agent 安全（Harness 的安全职责）
│
├── Trace（Harness 记录的执行路径）
│   ├── Skill（从 Trace 提炼的经验）
│   └── Agent Memory（跨会话的经验积累）
│       └── Three-Layer Learning（三层学习全景）
│           └── Meta-Harness（用 Agent 改进 Agent）
│
├── AGENTS.md 标准（Harness 的跨工具通用说明书）
├── 四动词框架（Harness 的完整闭环方法论）
├── Eval（衡量 Harness 改进的基准线）← NEW
└── ReAct / Plan-and-Execute（Agent 执行模式）← NEW
```

**示例文章骨架：**

**《一个 Agent 系统 99% 的代码都不是 Prompt》**（已写）
1. 开头：反直觉数据（Claude Code 512,000 行，Prompt 几千行）
2. 概念链：Prompt → Context Engineering → Harness
3. 深度：12 组件拆解 + 四个关键组件展开
4. 认知缺口：Harness ≠ Workflow
5. 结尾：Prompt 决定下限，Harness 决定上限

**《Agent 安全的三个象限》**（可写）
1. 开头：用 `gaps/agent-security-is-not-vuln-hunting.md` 的切入——FreeBSD 17 年 RCE
2. 概念 1：Agent 安全（`concepts/agent-security.md`）——三层纵深防御
3. 认知缺口 1：Agent 安全 ≠ Prompt 安全——第一象限
4. 认知缺口 2：Agent 安全 ≠ 漏洞发现安全——第二、三象限
5. 视角：控制发布 vs 开放发布——Glasswing 争议
6. 结尾：安全不是一个问题，而是三个

**《AI Agent 怎么从错误中学习》**
1. 开头：反常识——大多数 Agent 每天都在重复犯错
2. 概念 1：Trace（`concepts/trace.md`）—— 学习的原材料
3. 概念 2：Skill（`concepts/skill.md`）—— 经验的结晶（118 Skills + 40% 提升 + 不跨领域迁移）
4. 概念 3：Meta-Harness（`concepts/meta-harness.md`）—— 自动化闭环
5. 深度：用 `design-notes/skill-auto-generation-quality.md` 展示质量控制
6. 背景：用 `concepts/three-layer-learning.md` 放到更大图景
7. 结尾：从 trace 到 skill 到 meta-harness，Agent 学习的完整链路

**《为什么用了框架还是搞不定 Agent》**
1. 开头：用 `gaps/framework-is-not-harness.md` 的切入
2. 概念 1：Harness（`concepts/harness.md`）——"搭好的房子"
3. 概念 2：四动词框架（`concepts/four-verb-framework.md`）——Constrain/Inform/Verify/Correct
4. 深度：用 `design-notes/two-families-reasoning-vs-environment.md` 展示两大流派
5. 视角：Reasoning-First vs Environment-First
6. 结尾：Framework 给你积木，Harness 给你房子

### 第三步：填内容 → 用 design-notes/ 和 perspectives/ 增加深度

**写概念段落时**：
- 从概念卡片的"一句话"开始，这是读者的第一印象
- 用"与相邻概念的边界"防止读者混淆
- 用"展开时可用的例子"增加说服力

**写深度段落时**：
- 从设计笔记的"直觉反应"引起共鸣——这是读者心里的声音
- 用"设计考量"逐层展开——回答"为什么这么做"
- 用"对比视角"展示不同选择的代价

**写思辨段落时**：
- 从视角卡片的"核心问题"引入
- 展示两种视角各自的论据和代价
- 用"深层张力"揭示问题的本质
- 给出自己的判断（参考"使用建议"）

## 写作原则速查

| 原则 | 怎么做 | 反面 |
|------|--------|------|
| 概念先行 | 先讲清"是什么"和"为什么这么设计" | 一上来就贴代码 |
| 层层递进 | 从直觉反应 → 设计考量 → 深层启发 | 概念堆砌，没有递进 |
| 细节服务理解 | 每个技术细节回答"帮读者理解什么" | 堆 benchmark 数字 |
| 有观点有边界 | 有判断，同时说明适用范围 | 中立综述，什么都不敢说 |
| 填补认知缺口 | 击中"以为懂但搞混了"的地方 | 讲读者已经知道的 |

## 素材库速查表

```
wiki/craft/
├── concepts/ (12)       # 「这个概念是什么」
│   ├── harness.md              → Agent 运行时系统
│   ├── context-engineering.md  → 决定模型"看到什么"
│   ├── four-verb-framework.md  → Constrain/Inform/Verify/Correct
│   ├── agents-md-standard.md   → 跨工具通用说明书
│   ├── agent-security.md       → Agent 安全 ≠ 模型安全
│   ├── trace.md                → 执行轨迹，改进的数据源
│   ├── skill.md                → 结构化可复用经验（118 Skills，40% 提升）
│   ├── agent-memory.md         → 跨会话持久记忆
│   ├── hook-mechanism.md       → 生命周期拦截器
│   ├── meta-harness.md         → 用 Agent 改进 Agent
│   ├── three-layer-learning.md → 三层学习架构
│   └── eval.md                 → 衡量改进的基准线 ← NEW
│
├── design-notes/ (8)    # 「为什么这么做」
│   ├── two-families-reasoning-vs-environment.md → Reasoning-First vs Environment-First
│   ├── openclaw-time-decay-memory.md   → 时间衰减 + MEMORY.md 分层
│   ├── openclaw-compaction-algorithm.md → 40% chunk ratio + 三级降级
│   ├── hermes-memory-limit.md          → 3,575 字符约束即特性
│   ├── memos-triple-dedup.md           → 三层去重设计
│   ├── hermes-subagent-sync-blocking.md → 同步阻塞设计
│   ├── skill-auto-generation-quality.md → 双重质量关卡
│   └── self-generation-vs-marketplace.md → 自生成 vs 外部市场 ← NEW
│
├── gaps/ (8)            # 「你可能搞混了」
│   ├── rag-is-not-memory.md              → RAG ≠ 记忆
│   ├── prompt-is-not-harness.md          → Prompt ≠ 运行时
│   ├── skill-is-not-tool.md              → 技能 ≠ 工具
│   ├── framework-is-not-harness.md       → 框架 ≠ 运行时
│   ├── agent-security-is-not-prompt-security.md → Agent 安全 ≠ Prompt 安全
│   ├── harness-is-not-workflow.md        → Harness ≠ Workflow
│   ├── agent-security-is-not-vuln-hunting.md → Agent 安全 ≠ 漏洞发现安全 ← NEW
│   ├── agent-vs-harness-boundary.md      → Agent 层 vs Harness 层边界 ← NEW
│   └── skill-two-paths.md               → Skill 不止一条路径 ← NEW
│
├── perspectives/ (6)    # 「换个角度想」
│   ├── open-standard-vs-deep-integration.md  → AGENTS.md vs CLAUDE.md
│   ├── cognitive-depth-vs-connectivity.md    → Hermes vs OpenClaw
│   ├── reasoning-first-vs-environment-first.md → 信任模型 vs 信任环境
│   ├── memory-autonomy-vs-system-enforcement.md → 自治 vs 强制
│   ├── constraint-vs-unbounded-design.md     → 约束 vs 无界
│   └── controlled-vs-open-release.md         → 控制发布 vs 开放发布 ← NEW
│
└── drafts/ (1 + 本文件)  # 「草稿在这写」
    └── 99-percent-is-not-prompt.md → 第一篇完成的文章
```

## 写作命令

告诉我以下任何一种，我就开始起草：

1. **"写 [文章标题]"** → 我从素材库组装骨架 + 初稿
2. **"基于 [gap/concept 名] 写一篇"** → 我选切入角度 + 组装骨架
3. **"这个主题还缺什么素材"** → 我检查素材库，告诉你需要补充什么
4. **"把 [草稿文件] 润色一遍"** → 我基于素材库检查概念准确性和深度
