# 概念：Hook 机制

## 一句话

Agent 生命周期的自动拦截器——在"启动前"和"结束后"等关键节点强制执行系统逻辑，不给 Agent "忘记"的机会。

## 为什么需要这个概念

Agent 系统有一个根本矛盾：**我们希望 Agent 自主行动，但又不能完全信任它的自觉性。**

实验数据说明了这一点：依赖 Agent 自觉调用 memory 工具存储记忆，捕获率约 60%。这意味着有 40% 的有价值经验被默默丢弃——Agent 忙着处理任务，"忘了"调用记忆工具；或者它觉得"这个不重要"，但其实很重要。

Hook 机制的核心价值是：**把"Agent 应该做但可能忘了做"的事情，变成"系统自动做、Agent 无法绕过"的事情。** 这不是不信任 Agent，而是用系统保障弥补概率性的疏漏。

## 与相邻概念的边界

- **vs 中间件（Middleware）**：中间件通常处理请求/响应的通用逻辑（认证、日志），Hook 聚焦于 Agent 的业务生命周期节点（启动、结束、工具调用、错误恢复）。Hook 是 Agent 领域的中间件，但粒度更精细。
- **vs 事件系统（Event System）**：事件系统是松耦合的"发布-订阅"模式，Hook 是紧耦合的"拦截-执行"模式。区别在于：事件订阅者可以选择忽略事件，Hook 的逻辑一定会执行。
- **vs Prompt 指令**：你可以在 Prompt 里写"每次任务结束后请存储记忆"，但这是请求，不是强制。Agent 可能"忘了"或"选择不做"。Hook 在代码层面保证执行，不依赖 LLM 的遵从性。

## 设计直觉

Hook 的设计灵感来自 Web 框架的请求生命周期（Express.js 的 middleware、Django 的 signals），但在 Agent 场景下有一个关键不同：**被拦截的对象是一个有自主意识的 LLM，不是一个确定性的程序。**

这带来了两个特殊的设计约束：

**1. Hook 必须在 Agent 的决策循环之外运行。**

如果 Hook 本身需要 Agent 参与（"请你决定要不要存记忆"），那 Hook 就退化成了另一条 Prompt 指令——Agent 照样可以忽略。MemOS 的 `agent_end` Hook 在 Agent 的决策循环完全结束后触发，由系统代码（不是 LLM）执行捕获逻辑。

**2. Hook 的数量和复杂度需要克制。**

每个 Hook 都会增加系统延迟。`before_agent_start` Hook 如果要做混合检索（BM25 + 向量 + RRF 融合）再注入 Skill，启动延迟可能增加 500ms-2s。如果再加上 `before_tool_call`、`after_tool_call` 等 Hook，每次工具调用都被拦截，Agent 的响应速度会显著下降。

Hermes 的选择很有意思：它只暴露了四个插件钩子（`pre_llm_call`、`post_llm_call`、`on_session_start`、`on_session_end`），而且没有 MemOS 那样的 100% 捕获机制。这是一种**性能优先于完整性**的取舍。

## 理解的关键转折

**Hook 不只是"自动化"，它改变了 Agent 系统的信任模型。**

没有 Hook 的系统，所有行为都依赖 Agent 的自觉——这是一种"信任 Agent 会做正确的事"的模型。有 Hook 的系统，关键行为由系统保障——这是一种"信任但要验证"的模型。

这个区分在实践中的影响比看起来大得多：

- 没有 Hook：你不确定 Agent 今天有没有存储记忆，你需要人工抽查
- 有 Hook：你确信每次对话的关键信息都被系统自动捕获，你可以专注于分析质量

更深层地看，Hook 机制实现了 Agent 系统的**"关注点分离"**：Agent 专注于任务执行（它擅长的事），系统 Hook 负责记忆捕获、Skill 注入、质量审计（Agent 不擅长但必须做的事）。这和操作系统中"用户态 vs 内核态"的分离是同一个逻辑。

## 展开时可用的例子

- **MemOS 的 `before_agent_start`**：每次 Agent 启动时，Hook 自动提取任务关键词 → 混合检索相关记忆和 Skill → 按相关度排序取 Top-N → 注入 System Prompt。结果是 Agent 开始工作时已经"看到"了所有相关经验——"前一只工蜂的失败，变成后一只工蜂的本能。"
- **MemOS 的 `agent_end`**：对话结束时自动触发三层去重（哈希 → 向量 → LLM 裁决）后写入数据库。捕获率从 ~60% 提升到 ~100%。
- **Hermes 的四个插件钩子**：更轻量的方案。`on_session_start` 和 `on_session_end` 允许自定义逻辑，但不强制执行——它是"你可以挂自己的逻辑"，而非"系统帮你保证"。

## 相关页面

- [[entities/hook-mechanism|Hook 机制]] - 实体页面（实现细节）
- [[craft/concepts/agent-memory|概念：Agent Memory]] - Hook 是实现记忆捕获的关键
- [[craft/concepts/skill|概念：Skill]] - Auto-recall 基于 before_agent_start Hook
- [[craft/design-notes/memos-triple-dedup|MemOS 三层去重]] - agent_end Hook 内部的去重逻辑
