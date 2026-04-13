# 概念：Meta-Harness

## 一句话

用 Agent 改进 Agent 的 Harness——让编码 Agent 分析执行轨迹，自动建议 Harness 代码修改。

## 为什么需要这个概念

Agent 系统上线后，怎么持续变好？传统方式是人工分析日志、手动修改代码、重新部署。但当 Agent 每天处理上百个任务时，人工分析根本跟不上——你不可能逐条阅读所有 trace，找出失败模式，然后手写修复代码。

Meta-Harness 的核心思路是：**既然 Agent 擅长阅读和分析文本，那就让它来分析自己的 trace。** 这不是抽象的"AI 自我改进"概念，而是一个具体的工程流程：运行任务 → 收集 trace → 编码 Agent 读取 trace → 编码 Agent 提出代码修改 → 人类审核（或自动应用）→ Harness 变好 → 下一轮任务表现更好。

Harrison Chase 的原始论文给出了一个令人震惊的数据：**同一个底层模型（如 Claude 3.5 Sonnet），在不同的 Harness 下运行，SWE-Bench 上的性能差距可以达到 6 倍。** OpenCode 的 Hashline 实验进一步验证了这一点：仅改变编辑接口（给每行代码打唯一哈希，模型引用哈希而非复制原文），Grok Code Fast 从 6.7% 跳到 68.3%——**10 倍改进，只改 Harness 不改模型**。这说明 Harness 的质量对系统表现的影响可能远超模型本身的差异。Meta-Harness 就是系统化地缩小这个差距的方法。

## 与相邻概念的边界

- **vs Fine-tuning**：Fine-tuning 改的是模型权重（Model 层），Meta-Harness 改的是 Harness 代码（Harness 层）。前者需要训练资源、有灾难性遗忘风险；后者只需要编码 Agent + trace 数据，可回滚。
- **vs Prompt Engineering**：Prompt Engineering 手动调整给模型的指令，Meta-Harness 自动化这个过程——且不只改 Prompt，还改工具编排、错误处理、状态管理等 Harness 层代码。
- **vs Eval（评估）**：Eval 回答"这个 Harness 好不好"，Meta-Harness 回答"怎么让这个 Harness 变好"。Eval 是测量，Meta-Harness 是改进。两者互补：Eval 提供信号，Meta-Harness 利用信号行动。
- **vs Self-Improvement（如 Hermes 的学习循环）**：Hermes 的改进发生在 Context 层（更新记忆、创建 Skill），Meta-Harness 的改进发生在 Harness 层（修改代码）。Context 层改进是"换笔记"，Harness 层改进是"换工具箱"——后者更深层、影响更大，但风险也更高。

## 设计直觉

Meta-Harness 的工作流可以理解为一个**自动化的代码审查循环**：

```
正常运行 → 产生 trace（执行数据）
                ↓
        trace 存入文件系统
                ↓
     编码 Agent 读取 trace（通过 LangSmith CLI）
                ↓
     分析失败模式、性能瓶颈
                ↓
     生成 Harness 代码修改建议
                ↓
     人类审核 / 自动应用
                ↓
        Harness 更新
                ↓
        下一轮运行（表现更好）
```

关键设计选择是：**编码 Agent 和被改进的 Agent 不是同一个。**

被改进的 Agent 负责执行任务（写代码、搜索信息、处理数据），编码 Agent 负责分析 trace 并修改 Harness 代码。两者使用不同的工具、不同的 System Prompt、甚至可以使用不同的模型。这种分离的好处是编码 Agent 可以拥有完整的开发环境（读写文件、运行测试、提交代码），而执行 Agent 保持专注。

更深层的直觉是：**这是 Agent 版的"DevOps"。** 就像 DevOps 工程师不直接写业务代码，但通过优化 CI/CD、监控、基础设施来提升整个团队的产出——Meta-Harness 中的编码 Agent 不直接执行业务任务，但通过优化 Harness 来提升执行 Agent 的表现。

## 理解的关键转折

**改进 Harness 比改进模型更高杠杆。**

SWE-Bench 上 6 倍性能差距的数据已经说明了这一点。但更深层的逻辑是：

1. **模型改进有天花板**：每一代模型的性能提升越来越难、成本越来越高。GPT-4 到 GPT-5 的提升大概率不如 GPT-3 到 GPT-4。
2. **Harness 改进有复利**：每一次 Harness 优化对所有后续任务持续生效。一次修复错误处理逻辑，永远不会在同类错误上浪费 token。
3. **模型是别人的，Harness 是自己的**：你不能改 Claude 的权重，但你能改自己的 Harness 代码。Meta-Harness 让你把改进的焦点放在你能控制的部分。

这也解释了为什么 Harrison Chase 在 LangSmith 上投入了大量资源让编码 Agent 能够通过 CLI 直接访问 trace——因为他认为**未来 Agent 系统的核心竞争力不在模型，而在 Harness 的持续优化能力**。Meta-Harness 是实现这个优化的方法论。

## 展开时可用的例子

- **Meta-Harness 论文实验**：在 SWE-Bench 上，研究者让编码 Agent 分析失败 trace，建议修改 Harness 代码，然后重新运行。经过几轮迭代，系统性能显著提升——且提升不是来自换模型，纯粹来自 Harness 代码优化。
- **LangSmith + 编码 Agent 工作流**：LangSmith 收集 trace 并提供 CLI 访问。编码 Agent 用 CLI 查看 trace → 识别"这类任务总是在第 3 步失败" → 修改 Harness 中第 3 步的错误处理逻辑 → 重新运行 → 成功率提升。
- **三层学习的 Harness 层应用**：Harrison Chase 的三层框架中，Model 层改进交给模型厂商，Context 层改进靠记忆和 Skill，Harness 层改进正是 Meta-Harness 的战场——它是三层中投入产出比最高的一层。

## 相关页面

- [[entities/meta-harness|Meta-Harness]] - 实体页面（研究背景与数据）
- [[craft/concepts/trace|概念：Trace]] - Meta-Harness 的数据源
- [[craft/concepts/harness|概念：Harness]] - 被改进的目标
- [[craft/concepts/three-layer-learning|概念：三层学习]] - Meta-Harness 在三层中的位置
