# 概念：Skill（Agent 技能）

## 一句话

从 Agent 的成功和失败中提炼出来的、可复用的结构化经验——"训练手册"而非代码。

## 为什么需要这个概念

Agent 每天处理大量任务，其中积累了很多有价值的经验："这种类型的文件用 X 方法处理效果好"、"遇到 Y 错误应该先检查 Z"。但如果这些经验只存在于某次对话的上下文里，下次遇到同样问题时，Agent 又要从零摸索。

Skill 就是把这些散落在 trace 中的经验，**提炼为结构化的、可检索的、可注入的知识单元**。它让 Agent 的经验可以跨会话、跨任务、甚至跨 Agent 传递。

@eng_khairallah1 给出了一个精准的定义："A Skill is a training manual for a new employee."（Skill 是给新员工的培训手册。）它告诉 Agent 在特定场景下该怎么做，而不是一段可执行的代码。

## 与相邻概念的边界

- **vs 工具（Tool）**：Tool 是 Agent 的"手"——执行具体操作（搜索、读文件、调 API）。Skill 是 Agent 的"肌肉记忆"——知道什么时候用哪个 Tool、按什么顺序用、常见陷阱是什么。
- **vs Prompt**：Prompt 是泛化的指令（"你是一个有帮助的助手"），Skill 是针对特定任务的专业流程（"写推文时，每个技术概念必须配一句大白话解释"）。
- **vs 记忆（Memory）**：Memory 记录"发生了什么"（语义记忆、情景记忆），Skill 提炼"怎么做"（程序性记忆）。Skill 是 Memory 的价值升级——从原始数据到可操作的知识。

## 设计直觉

Skill 的核心设计问题是**粒度**和**质量控制**。

**粒度**：太粗（"如何做内容营销"）→ 不够具体，Agent 不知道怎么执行。太细（"在第三段用一个反问句"）→ 不够通用，适用面窄。好的 Skill 介于两者之间——@eng_khairallah1 的五组件结构是一个实用的标准：YAML Trigger（触发条件）+ Overview（一句话说明）+ Workflow（步骤化流程）+ Output Format（输出规范）+ Examples（具体示例）。

**质量控制**：两种流派。

Hermes 路径——Agent 主动创建，触发条件是"5+ 工具调用、错误恢复、用户纠正"。Agent 有主动权，但会重复造 Skill（社区高频反馈问题）。**量化数据**：Hermes 生态当前有 118 个 Skills（截至 2026-04-12），使用自创 Skill 的 Agent 完成研究任务比新实例快 **40%**。但这种提升**不跨领域迁移**——一个在编程任务上积累的 Skill 库对写作任务几乎无帮助。本质上，Hermes 的"grows with you"是**结构化笔记 + 检索**，而非真正的能力泛化。

MemOS 路径——系统自动生成，四步流程：规则过滤 → 混合检索 → LLM 决策（confidence ≥ 0.7 升级，< 0.3 新建）→ 质量评分（≥ 6 分才写入）。100% 捕获率，但依赖系统设计的好坏。

## 理解的关键转折

**Skill 的真正价值不在于"当前 Agent 变强"，而在于"经验可传递"。**

一个 Agent 犯了错、从错误中学到了教训、这个教训被提炼成 Skill、下一个 Agent 在开始任务前自动注入这个 Skill——于是它从第一步就避开了同样的坑。

MemOS 的 Auto-recall 机制实现了这一点：`before_agent_start` Hook 自动检索相关 Skill → 注入到 System Prompt → Agent 开始时已"看到"所有相关经验。文章里的描述很生动："前一只工蜂的失败，变成后一只工蜂的本能。"

这也是为什么 Skill 系统是 Harness Engineering 的核心组件之一——它把 trace（原始数据）转化为可操作的知识（Skill），再通过 Auto-recall 实现知识的自动分发。整个链路是：trace → 提炼 → Skill → 注入 → 改进。

## 展开时可用的例子

- **Claude Code Skills**：社区注册表有 80,000+ Skills，但大多数质量堪忧。质量问题不在于数量不够，而在于缺乏结构化的设计方法和测试协议。
- **Hermes 的渐进式加载**：启动时只注入 Skill 的名称和摘要（索引），需要时才加载完整内容。这是用架构解决"Skill 太多导致上下文膨胀"的问题。
- **OpenClaw Skills 生态**：ClawHub 13,000+ community skills（但 12-20% 恶意率——ClawHavoc 攻击中 341/2,857 Skills 被确认含恶意代码，分发 Atomic macOS Stealer），三级加载优先级（Workspace > User > Bundled），同名 Workspace skill 可覆盖全局。这是"安装即用"路径——Skill 是打包分发的目录（SKILL.md + 脚本），而非从 trace 中提炼的经验。与 Hermes"积累即学"形成对比。**安全隐患**（飞樰分析）：Skill 包含可执行脚本，恶意开发者可植入病毒/后门/WebShell——类似下载带毒 APP。OpenClaw 近期强化了 ClawHub 来源管控和鉴权，但"能力无限 vs 运行安全"的平衡仍在演进中。这是所有开放 Skill 生态面临的根本张力。
- **CLI vs MCP vs Skills**：宝玉的分析——"CLI 是手，MCP 是另一种手，Skills 是肌肉记忆。" 三者配合工作：Skills 告诉 Agent 该用什么命令、什么场景该用什么参数、出错了怎么处理。
- **Superpowers（纪律型 Skill）**（obra 开源）：一种特殊的 Skill 定义方式——纯 Markdown 描述的**工程最佳实践流程**（TDD、代码审查、计划先行、完成前验证）。与 Hermes/OpenClaw 的"从 trace 提炼经验"型 Skill 不同，Superpowers 是**预定义的纪律规范**，按需加载到 Agent 上下文中。核心特点：零依赖、任意 AI 会话可用、避免全部加载导致上下文膨胀。这说明 Skill 不止一条路径——除了"积累→提炼"还有"规范→注入"。

## 相关页面

- [[entities/skill-system|Skill 系统]] - 详细实现对比
- [[entities/hook-mechanism|Hook 机制]] - Skill 的自动注入机制
- [[sources/building-claude-skills-guide|如何构建真正有效的 Claude Skills]] - 五组件结构
- [[sources/hermes-vs-memos-harness-engineering|Hermes vs MemOS]] - 两种 Skill 生成路径对比
