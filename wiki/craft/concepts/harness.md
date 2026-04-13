# 概念：Harness

## 一句话

Agent 的运行时系统——不是模型本身，而是模型"跑"在什么上面。

## 为什么需要这个概念

大多数人谈 AI Agent 时，关注点在模型能力（GPT-4 vs Claude vs Gemini）。但实践中，同一个模型在不同 Harness 下的表现差异可以达到 6 倍（Meta-Harness 论文数据）。Princeton 研究更精确地量化了这一点：仅改变 harness 配置，solve rate 提升 64%；同一 Claude Opus 4.5 在不同 harness 下得分 2% vs 12%——6x 性能差距完全归因于环境设计而非模型能力。这说明**模型只是 Agent 的一部分**，而且可能不是最关键的部分。

更现实的问题是：当你只靠裸 API 调用大模型，模型每次运行完就"走了"——它不记得上次做了什么，不知道哪些方法有效，不会从失败中学习。Harness 就是解决这个问题的：让模型的每次运行都产生价值沉淀。

## 与相邻概念的边界

- **vs Prompt**：Prompt 是给模型的指令文本；Harness 包含 Prompt，但还包括记忆管理、工具调用、轨迹捕获、评估机制——所有模型"看不到"但影响模型表现的系统层。三者构成嵌套关系：**Prompt ⊂ Context ⊂ Harness**。
- **vs Context Engineering**：Context 决定模型看到什么信息（六大组件：System Instructions、Memory Management、Retrieved Information、Tool Orchestration、Output Structuring、Query Augmentation）。Harness 包含 Context，但还管防止、度量、控制和修复。
- **vs Framework（如 LangChain）**：Framework 提供构建 Agent 的工具库和抽象层；Harness 是你用这些工具搭出来的、面向特定场景的运行时系统。LangChain 是积木盒子，Harness 是你搭出来的那栋房子。
- **vs Scaffold**：Scaffold 只管"怎么跑一次"，Harness 还管"这次跑的结果如何改进下次"。Harness = Scaffold + 轨迹捕获 + 持续学习闭环。这是 Harrison Chase 提出的核心区分。
- **vs Orchestration**：Orchestration 侧重多 Agent 的协调与调度；Harness 是单个 Agent 的完整运行时，可以包含 Orchestration 也可以不包含。

## 设计直觉

为什么叫 "Harness"（马具）而不是 "Wrapper" 或 "Shell"？因为 Harness 的隐喻精确：**不是限制马，而是让马的力量可以被使用**。裸奔的模型像脱缰的马——能力很强但不可控、不可积累、不可审计。NxCode 展开了这个隐喻：马是模型（快速强大但不知方向），Harness 是缰绳马具（约束、引导、反馈），骑手是工程师（提供方向，不亲自跑）。

**约束反直觉效应**：约束解空间反而让 Agent 更高效。无约束时浪费 token 探索死胡同，有约束时更快收敛到正确方案。OpenAI 的依赖分层（Types→Config→Repo→Service→Runtime→UI）就是一个例子——不是建议，而是 CI 强制执行。

Vivek Trivedy 给出过一个更直接的定义："If you're not the model, you're the harness." 模型之外的一切软件基础设施——Prompt 构建、工具编排、状态管理、错误处理、安全防护、验证循环——都是 Harness。

## 理解的关键转折

**从"工具思维"到"系统思维"的转变**：

- 工具思维：模型执行任务 → 返回结果 → 完了
- 系统思维：模型执行任务 → 返回结果 → **同时捕获轨迹 → 改进下一次**

这个转折的实践意义在于：你在 Harness 上投入的工程努力，会随着使用积累复利。模型可能被下一代替换，但 Harness 捕获的轨迹和积累的经验不会被重置。Phil Schmid 的判断——"竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹"——正是基于这个逻辑。

## 展开时可用的例子

- **Claude Code**：512,000+ 行代码，绝大部分是 Harness，模型调用只是其中一小部分。同一个 Claude 3.5 Sonnet，Anthropic 的 Harness vs 裸 API 调用，在 SWE-Bench 上的差距是倍数级别。
- **Codex**：OpenAI 3 名工程师，五个月没手写一行代码，指挥 Codex Agent 写了 100 万行代码。核心不是模型多强，而是 Harness 设计得当——文件系统重构、工具链、可观测性、Linter 重写（受众从人变成 AI）。
- **Akshay Pachaar 的 12 组件拆解**：将 Harness 系统化为 Orchestration Loop、Tools、Memory、Context Management 等 12 个组件，证明 Harness 是一个完整的工程学科。
- **OpenClaw 的 Harness 实践**（飞樰源码分析）：虽然 OpenClaw 没有显式宣称"Harness 框架"，但处处体现了 Harness Engineering 精髓：**7 种 Hook 钩子**贯穿全生命周期（before_prompt_build → before_tool_call → after_tool_call → before/after_compaction → message_received → message_sending），实战中用 `before_tool_call` 做参数校验——拦截错误格式一次配置全局生效；**三层安全沙箱**做纵深防御（文件系统沙箱 → 命令执行沙箱 → 网络访问沙箱）；**Human-in-the-Loop** 高风险操作暂停等人确认。这是 Harness 从抽象概念到具象实现的最佳案例。
- **Harness vs Workflow**（飞樰辨析）：核心区别是**主导权归属**——Workflow 是硬编码线性路径，模型只是节点（主导权在人）；Harness 是动态软约束，模型保留自主规划和迭代权（主导权在 AI，在约束内）。模型能力越强，Harness 越优于 Workflow。这个辨析很有文章价值——很多读者会混淆这两个概念。
- **三层拼图模型**（岚天逸剑）：当 Harness 进入多 Agent 开发场景，职责进一步分解为**规范层（OpenSpec）→ 纪律层（Superpowers）→ 协作层（Harness/Agent Team）**。OpenSpec 管"做什么"（spec-driven development），Superpowers 管"怎么做"（纯 Markdown 纪律技能），Harness 管"谁来做"（角色分工+权限管控）。配合原则："规范不变、流程不变、团队可弹性扩展。" 这个拆法说明 Harness 不是铁板一块——随着工程复杂度增加，Harness 的不同职责会分化为独立的层。

## 相关页面

- [[entities/harness-engineering|Harness Engineering]] - 方法论框架
- [[entities/meta-harness|Meta-Harness]] - Harness 层的持续学习
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - 12 组件拆解
- [[sources/harness-memory-lockin|Your Harness, Your Memory]] - Harness 与 Memory 的绑定
