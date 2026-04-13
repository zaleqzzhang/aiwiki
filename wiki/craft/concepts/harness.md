# 概念：Harness

## 一句话

Agent 的运行时系统——不是模型本身，而是模型"跑"在什么上面。

## 为什么需要这个概念

大多数人谈 AI Agent 时，关注点在模型能力（GPT-4 vs Claude vs Gemini）。但实践中，同一个模型在不同 Harness 下的表现差异可以达到 6 倍（Meta-Harness 论文数据）。这说明**模型只是 Agent 的一部分**，而且可能不是最关键的部分。

更现实的问题是：当你只靠裸 API 调用大模型，模型每次运行完就"走了"——它不记得上次做了什么，不知道哪些方法有效，不会从失败中学习。Harness 就是解决这个问题的：让模型的每次运行都产生价值沉淀。

## 与相邻概念的边界

- **vs Prompt**：Prompt 是给模型的指令文本；Harness 包含 Prompt，但还包括记忆管理、工具调用、轨迹捕获、评估机制——所有模型"看不到"但影响模型表现的系统层。
- **vs Framework（如 LangChain）**：Framework 提供构建 Agent 的工具库和抽象层；Harness 是你用这些工具搭出来的、面向特定场景的运行时系统。LangChain 是积木盒子，Harness 是你搭出来的那栋房子。
- **vs Scaffold**：Scaffold 只管"怎么跑一次"，Harness 还管"这次跑的结果如何改进下次"。Harness = Scaffold + 轨迹捕获 + 持续学习闭环。这是 Harrison Chase 提出的核心区分。
- **vs Orchestration**：Orchestration 侧重多 Agent 的协调与调度；Harness 是单个 Agent 的完整运行时，可以包含 Orchestration 也可以不包含。

## 设计直觉

为什么叫 "Harness"（马具）而不是 "Wrapper" 或 "Shell"？因为 Harness 的隐喻精确：**不是限制马，而是让马的力量可以被使用**。裸奔的模型像脱缰的马——能力很强但不可控、不可积累、不可审计。

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

## 相关页面

- [[entities/harness-engineering|Harness Engineering]] - 方法论框架
- [[entities/meta-harness|Meta-Harness]] - Harness 层的持续学习
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - 12 组件拆解
- [[sources/harness-memory-lockin|Your Harness, Your Memory]] - Harness 与 Memory 的绑定
