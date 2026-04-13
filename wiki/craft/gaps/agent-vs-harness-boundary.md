# 认知缺口：Agent 层 vs Harness 层的边界

## 读者"以为懂"的认知

"Agent 就是那个 AI 助手啊——会调用工具、会思考、会记忆、会执行任务。Harness 只是个运行环境，像个容器。"

## 实际区别

**"Agent"这个词在不同的架构模型中指代不同的层，这导致了持续的混淆。**

两种主流三层划分：

| | Harrison Chase 版 | Tom Foster 版 |
|--|-------------------|---------------|
| **第一层** | Model（统计引擎） | Model（统计引擎） |
| **第二层** | Harness（推理循环 + 执行平台） | Agent（推理循环） |
| **第三层** | Context（信息管理） | Harness（执行平台） |

核心分歧：**推理循环（Reasoning Loop）属于 Agent 还是 Harness？**

- **Chase 版**：推理循环是 Harness 的一部分。Harness = 推理循环 + 工具 + 记忆 + 上下文管理。Agent 只是一个笼统的称呼。
- **Foster 版**：推理循环是独立的 Agent 层。Agent = Thought → Action → Observation 循环。Harness = 约束、工具注册、权限控制、环境管理。

## 为什么误解常见

1. **"Agent"一词的滥用**：行业把从 ChatGPT 到 Claude Code 到自动驾驶的所有东西都叫"Agent"，这个词已经过度泛化
2. **营销驱动的模糊化**：框架提供商（LangChain、CrewAI）倾向于让用户觉得"用了我的框架就是在做 Agent"，模糊了架构层次
3. **实践中边界确实模糊**：Claude Code 里，推理循环和 Harness 是紧密耦合的——理论上是两层，实现上是一体的
4. **两个权威来源不一致**：Chase（LangChain 创始人）和 Foster（Contextua.dev 作者）都是该领域的思想领袖，但划分方式不同

## 正确心智模型

**没有"正确"的划分——两种都有道理，关键是知道在什么语境下用哪种。**

```
Chase 版（适合分析"持续学习"）：
┌─────────────┐
│   Context    │ ← 信息管理（Memory、Prompt、RAG）
├─────────────┤
│   Harness    │ ← 推理循环 + 工具 + 约束 + 环境
├─────────────┤
│    Model     │ ← 统计引擎
└─────────────┘
优势：三层各对应一种学习方式（Context 学习/Harness 学习/Model 学习）

Foster 版（适合分析"故障诊断"）：
┌─────────────┐
│   Harness    │ ← 执行平台（工具注册、权限、环境）
├─────────────┤
│    Agent     │ ← 推理循环（Thought → Action → Observation）
├─────────────┤
│    Model     │ ← 统计引擎
└─────────────┘
优势："大部分日常 Agent 失败是 Layer 3（Harness）问题"——诊断经验清晰
```

**实用判断标准**：
- 谈"怎么让 Agent 越用越好" → 用 Chase 版（三层学习框架清晰）
- 谈"为什么 Agent 出了 Bug" → 用 Foster 版（故障定位到具体层）
- 谈"怎么搭建 Agent 系统" → 都可以，但要**声明你用的是哪种划分**

## 文章切入点

开头用"同一个词，两种含义"引入：

> "当 Harrison Chase 说'Harness'的时候，他包含了推理循环。当 Tom Foster 说'Harness'的时候，他不包含推理循环。两个人都对——但如果你不知道他们用的是不同的地图，你就会迷路。"

然后用具体场景展示两种划分各自的优势，帮读者建立"根据语境选择模型"的心智模型，而非"哪种是对的"。

## 相关页面

- [[craft/concepts/harness|概念：Harness]] - Harness 的完整概念卡片
- [[craft/concepts/three-layer-learning|概念：三层学习]] - Chase 版划分的核心应用
- [[craft/concepts/execution-modes|概念：Agent 执行模式]] - Agent 层的具体内容
- [[sources/three-layers-model-agent-harness|Model, Agent, Harness: Three Layers]] - Foster 版来源
- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - Chase 版来源
- [[sources/frameworks-runtimes-harnesses|Agent Frameworks, Runtimes, and Harnesses]] - Chase 版补充
- [[entities/hwchase17|Harrison Chase]] - Chase 版作者
- [[entities/tom-foster|Tom Foster]] - Foster 版作者
