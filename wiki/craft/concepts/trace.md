# 概念：Trace（轨迹）

## 一句话

Agent 完整执行路径的记录——它做了什么、怎么做的、结果如何——是所有改进的数据源。

## 为什么需要这个概念

在软件工程中，日志（log）用于排查问题。在 Agent 系统中，trace 的作用远不止于此：**它是改进系统的原材料**。

没有 trace，你不知道 Agent 为什么成功或失败。有了 trace，你可以：分析失败模式、训练更好的模型、优化 Harness 代码、提炼可复用的 Skill。trace 之于 Agent 系统，就像数据之于机器学习——是一切改进的起点。

## 与相邻概念的边界

- **vs Log**：Log 通常记录系统事件（错误、警告），面向运维人员。Trace 记录 Agent 的完整决策路径（思考过程、工具调用、中间结果），面向改进流程。
- **vs 对话历史**：对话历史是 trace 的子集——只记录用户和 Agent 之间的消息。Trace 还包括 Agent 的内部推理、工具调用链、中间状态变化。
- **vs Telemetry**：Telemetry 是聚合性的统计数据（平均响应时间、错误率）。Trace 是细粒度的单次执行记录。

## 设计直觉

Trace 的设计有一个核心张力：**完整性 vs 成本**。

- 完整 trace：记录 Agent 的每一步思考、每一次工具调用、每一个中间结果。信息量大，但存储成本高、分析复杂。
- 精简 trace：只记录关键节点（开始、结束、错误）。成本低，但丢失了"为什么"的信息。

MemOS 选择了完整 trace + 智能去重：先全量捕获，再用三层机制（哈希去重、相似度检测、LLM 裁决）压缩噪音。Hermes 选择了精简 trace + 策展：Agent 自己决定什么值得记录，牺牲完整性换取效率。

更深层的设计问题是：**谁来捕获 trace？**

依赖 Agent 自觉捕获（"记得调用 memory 工具"），捕获率约 60%。系统 Hook 强制捕获（在关键生命周期节点自动触发），捕获率接近 100%。这个差距不仅仅是数字——60% 的 trace 意味着你无法做可靠的 Eval，因为你不知道那丢失的 40% 里有没有关键失败模式。

## 理解的关键转折

**Trace 不只是"回头看"的工具，它是"向前改进"的引擎。**

传统思路：trace 用于事后排查（"为什么出了 bug？"）。Agent 系统思路：trace 用于持续改进——

1. Model 层：收集 trace → 作为 RL/SFT 训练数据 → 改进模型
2. Harness 层：收集 trace → 编码 Agent 分析 → 修改 Harness 代码（Meta-Harness）
3. Context 层：收集 trace → 提取模式 → 更新记忆和 Skill

这三条改进路径都以 trace 为起点。所以 Harrison Chase 说："Trace 是所有持续学习流程的核心数据源。" Phil Schmid 更进一步："竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹。"

一个推论：**你积累 trace 的速度和质量，决定了你的 Agent 系统改进的速度和上限**。这是一个飞轮效应——更多 trace → 更好的改进 → 更多用户 → 更多 trace。

## 展开时可用的例子

- **LangSmith**：trace 收集平台。让编码 Agent 通过 CLI 访问 trace，分析并修改 Harness 代码。这是 Meta-Harness 工作流的基础设施。
- **MemOS 的 Skill 自动生成**：从 trace 中自动识别有价值的模式 → 混合检索已有 Skill → LLM 决策（升级还是新建）→ 质量评分。整个流程的输入就是 trace。
- **Anthropic 的评估器**：长时运行 Agent 的生成器-评估器架构中，评估器需要完整 trace 才能对照标准打分。没有 trace，评估就无从谈起。

## 相关页面

- [[entities/hook-mechanism|Hook 机制]] - 保证 trace 捕获完整性
- [[entities/meta-harness|Meta-Harness]] - 基于 trace 改进 Harness
- [[entities/skill-system|Skill 系统]] - 从 trace 中提炼可复用经验
- [[topics/continual-learning|持续学习]] - trace 驱动的三层学习
