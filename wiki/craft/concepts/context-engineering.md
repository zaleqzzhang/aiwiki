# 概念：Context Engineering

## 一句话

决定模型"看到什么信息"的工程学科——不是写 Prompt，而是策展 Prompt 周围的整个信息生态。

## 为什么需要这个概念

大多数人把注意力放在"怎么措辞指令"上（Prompt Engineering），但模型的表现更取决于它"看到了什么"。一个完美的指令，如果配上错误的上下文——过时的文档、无关的历史、缺失的约束——模型照样给出垃圾输出。

Andrej Karpathy 的判断：未来真正稀缺的能力不是写 Prompt，而是 Context Engineering——策展送入 LLM 上下文窗口的信息。这比措辞更难，也更有杠杆效应。

## 与相邻概念的边界

- **vs Prompt Engineering**：Prompt 是 Context 的子集。Prompt 关注指令措辞（"请用 JSON 格式回复"），Context Engineering 关注整个信息生态——系统指令只是其中一个组件。三者构成嵌套关系：**Prompt ⊂ Context ⊂ Harness**。
- **vs Harness Engineering**：Context 决定模型看到什么，Harness 还管模型看不到的系统层——防止什么（安全沙箱）、度量什么（可观测性）、控制什么（Human-in-the-Loop）、修复什么（错误恢复）。Context 是 Harness 的子集。
- **vs RAG**：RAG（检索增强生成）是 Context Engineering 的一种实现手段——处理"Retrieved Information"这一个组件。Context Engineering 的范围远大于 RAG。

## 六大组件

PrivOcto 的整理给出了 Context Engineering 的完整组件图：

| 组件 | 管什么 | 示例 |
|------|--------|------|
| **System Instructions** | 行为准则与边界 | "你是一个代码审查助手，只回复代码问题" |
| **Memory Management** | 短期会话 + 长期持久知识 | OpenClaw 的 MEMORY.md + 每日笔记双层管理 |
| **Retrieved Information** | 从外部数据库/API 拉取的动态信息 | RAG、知识库检索、API 调用结果 |
| **Tool Orchestration** | 工具访问与输出回流 | 函数调用结果注入上下文 |
| **Output Structuring** | 响应格式强制 | JSON Schema、Pydantic 模型 |
| **Query Augmentation** | 将粗糙输入转为可处理请求 | 意图解析、查询改写、多轮澄清 |

## 设计直觉

为什么叫"Engineering"而不是"Management"？因为这不是被动管理，而是主动策展。

**核心挑战是 Context Rot（上下文衰减）**：随着 token 数量增加，模型准确召回信息的能力下降。Chroma 的数据显示：关键内容放在上下文窗口中间位置时，性能下降超过 30%。这意味着 Context Engineering 的目标不是"把所有相关信息塞进去"，而是策展**最小可行的高信号 token 集**。

OpenClaw 的实现完美体现了这个原则：23 个模块按固定顺序拼装，但不是全部加载——`full` 模式加载所有模块，`minimal` 模式（子 Agent）只保留核心模块，`none` 模式极简到只有身份标识。**按需加载，而非一次性填满。**

## 理解的关键转折

**从"信息越多越好"到"信号越纯越好"的转变**：

- 直觉：给模型更多信息 → 模型更聪明
- 现实：给模型更多信息 → Context Rot → 关键信息被淹没 → 模型更蠢

这就是为什么 Vercel 删掉 80% 的工具后效果反而更好——不是工具不有用，而是工具描述占据了宝贵的上下文窗口，稀释了真正重要的信息。

OpenClaw 的 Compaction（压缩）机制是应对 Context Rot 的工程解法：当 token 接近上限时，自动分块摘要，保留活跃任务和关键决策，丢弃细节——本质上就是在做 Context Engineering 的自动化策展。

## 展开时可用的例子

- **OpenClaw System Prompt 23 模块**：每个模块有加载条件（full/minimal/none），Skills 只在"有技能时"才注入名称和描述。这是 Context Engineering 的实践范本——不是把所有能力都告诉模型，而是按需披露。
- **Context Rot 数据**：Chroma 研究显示中间位置性能降 30%+，Prompt 重排示例可导致准确率偏移超 40%。这说明信息的排列顺序本身就是 Context Engineering 的一部分。
- **Princeton 研究的另一个视角**：仅改变 harness 配置（不改模型、不改 Prompt），solve rate 提升 64%——这 64% 中有很大一部分来自 Context Engineering 层面的改进（给模型看到了正确的信息）。
- **OpenClaw Skills 渐进式披露**：默认不加载全部能力，任务需要时才将 Skill 名称和描述注入上下文。vs Hermes 的 Skill 加载策略——体现了不同的 Context Engineering 哲学。

## 相关页面

- [[craft/concepts/harness|概念：Harness]] - Context Engineering 的上层概念
- [[craft/gaps/prompt-is-not-harness|认知缺口：Prompt ≠ Harness]] - 为什么只关注 Prompt 不够
- [[craft/design-notes/openclaw-compaction-algorithm|设计笔记：OpenClaw 压缩算法]] - Context Engineering 从理论到工程的关键实现
- [[entities/harness-engineering|Harness Engineering]] - 三层嵌套关系的完整论述
- [[sources/prompt-vs-context-vs-harness|Prompt vs Context vs Harness]] - 三层对比的核心来源
