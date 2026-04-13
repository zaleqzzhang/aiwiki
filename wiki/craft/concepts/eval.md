# 概念：Eval（Agent 评估）

## 一句话

Eval 是衡量 Agent 系统改进是否真的有效的基准线——没有 Eval，所有优化都是猜测。

## 为什么需要这个概念

Agent 系统有大量可调参数：System Prompt 措辞、工具选择、上下文窗口管理、记忆策略、沙箱权限……每个参数的调整都可能影响性能。但"感觉变好了"和"确实变好了"之间有巨大的鸿沟。

Eval 是 Harness Engineering 三大支柱之一（记忆治理、架构约束、**评估闭环**）。没有 Eval 的改进是单向的——你不知道改动是真的提升了性能，还是只是在不同的失败模式之间切换。

Princeton SWE-bench 的量化数据让这一点无比清晰：**同一个底模，不同的 Harness 设计可以拉出 6x 性能差距**（2% vs 12%）。但这个数据的前提是——你需要一个 benchmark 来测量。

## 与相邻概念的边界

- **vs 单元测试**：单元测试验证代码的确定性行为（输入 X → 输出 Y）。Eval 验证 Agent 的概率性行为（同一个输入可能产生不同的工具调用链和最终结果）。单元测试追求 100% 通过率，Eval 追求统计显著性。
- **vs Benchmark**：Benchmark 是标准化的公共测试集（SWE-bench、HumanEval、CyberGym）。Eval 可以是 Benchmark，也可以是你自己场景的测试套件。**你的 Eval > 公共 Benchmark**——因为公共 Benchmark 衡量的是通用能力，你关心的是你的场景。
- **vs Trace**：Trace 记录"发生了什么"，Eval 衡量"做得好不好"。Trace 是输入，Eval 是判断。Meta-Harness 的完整链路是：Trace → 分析 → 改进 → **Eval 验证**。
- **vs Reflection Pattern**：Reflection 是 Agent 在运行时自我反思和改进（Generate → Reflect → Refine），Eval 是在开发时系统性地验证改进效果。一个是在线的，一个是离线的。

## 设计直觉

Eval 的核心张力是：**真实性 vs 可重复性**。

- 最真实的 Eval 是让 Agent 在生产环境中处理真实任务——但结果不可重复（用户输入不同、环境不同）
- 最可重复的 Eval 是固定的 Benchmark——但可能不反映你的真实场景

三种 Eval 策略：

1. **公共 Benchmark**：SWE-bench（代码修复）、HumanEval（代码生成）、CyberGym（安全）。优点：可对比、可重复。缺点：容易过拟合、可能不反映真实场景。
2. **自定义 Eval 套件**：从你的 Trace 中筛选有代表性的任务，固定输入和期望输出。优点：贴近真实场景。缺点：需要人工标注、维护成本。
3. **LLM-as-Judge**：用另一个 LLM 评估 Agent 的输出质量。Anthropic 的"生成器 + 评估器"模式。优点：可规模化。缺点：评估器本身可能有偏差。

**实践中最有效的是第 2 + 3 种的混合**：从 Trace 中提取任务作为 Eval 用例，用 LLM-as-Judge 做初筛，人工验证关键用例。

## 理解的关键转折

**Eval 不是"做完再加"的可选组件，而是"不做就无法改进"的核心基础设施。**

LangChain 仅改 Harness 就从 SWE-bench Top 30 升到 Top 5——但这个改进的前提是他们有 SWE-bench 作为衡量标准。Hashline 实现了 10x 改进（6.7% → 68.3%），同样依赖持续的 Eval 反馈。

没有 Eval 的 Agent 开发就像没有仪表盘的飞行——你不知道飞得多高、多快、油还剩多少。

**Eval 驱动开发（EDD）**可能是 Agent 时代的 TDD：
1. 定义期望行为（Eval 用例）
2. 运行 Agent（执行）
3. 对比结果（验证）
4. 改进 Harness（迭代）

## 展开时可用的例子

- **Princeton SWE-bench 6x 差距**：同一个底模（GPT-4），不同 Harness 环境设计，solve rate 从 2% 到 12%——只有 Eval 才能量化这个差距。这是"Harness 比 Prompt 重要"的最强量化证据。
- **Hashline 10x 改进**：通过优化单一组件（编辑工具），从 6.7% 提升到 68.3%。SWE-bench 作为 Eval 基准，让团队能精确定位瓶颈（不是模型的问题，是工具的问题）。
- **Anthropic 生成器 + 评估器**：两个 Agent 配合——一个生成结果，另一个评估质量。评估器的 System Prompt 包含评分标准、边界条件、常见失败模式。这是 LLM-as-Judge 的工业级实践。
- **Hermes 40% 速度提升**：Nous Research 声称使用自创 Skill 的 Agent 快 40%——但这个数据的可信度取决于 Eval 方法论是否公开、是否可重复。没有公开的 Eval 细节 = 缺少验证。

## 相关页面

- [[entities/harness-engineering|Harness Engineering]] - Eval 是三大支柱之一（评估闭环）
- [[craft/concepts/trace|概念：Trace]] - Eval 的输入数据来源
- [[craft/concepts/meta-harness|概念：Meta-Harness]] - Eval 验证 Meta-Harness 改进的效果
- [[craft/concepts/three-layer-learning|概念：三层学习]] - Eval 在三层学习中的角色
- [[sources/harness-engineering-complete-guide|Harness Engineering: Complete Guide]] - 三支柱框架来源
- [[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]] - Hashline 10x 数据来源
