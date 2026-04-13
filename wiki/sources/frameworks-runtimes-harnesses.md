# Agent Frameworks, Runtimes, and Harnesses: oh my!

**类型**: article
**日期**: 2025-10-26
**来源**: https://blog.langchain.com/agent-frameworks-runtimes-and-harnesses-oh-my/
**作者**: Harrison Chase (LangChain)
**标签**: #framework #runtime #harness #langchain #langgraph #deepagents #taxonomy

## 摘要

Harrison Chase 首次系统区分 Agent 生态中三个易混淆的概念：**Framework**（抽象层）、**Runtime**（生产基础设施）、**Harness**（开箱即用的完整系统）。以 LangChain 自家产品线为例——LangChain 是 Framework，LangGraph 是 Runtime，DeepAgents 是 Harness。文章坦承边界模糊（"Part of the fun of developing in an early space is coming up with the mental models"），但这个分类学为行业讨论提供了有价值的起点。

## 关键要点

### 1. 三层分类学

| 类别 | 代表 | 核心价值 | 抽象层次 |
|------|------|---------|---------|
| **Agent Framework** | LangChain, Vercel AI SDK, CrewAI, OpenAI Agents SDK, Google ADK, LlamaIndex | 抽象——心智模型、标准化构建方式、快速起步 | 中层 |
| **Agent Runtime** | LangGraph, Temporal, Inngest | 生产基础设施——持久执行、流式处理、Human-in-the-Loop、线程级/跨线程持久化 | 底层 |
| **Agent Harness** | DeepAgents, Claude Code, Coding CLIs | 开箱即用——默认 Prompt、有主见的工具调用处理、规划工具、文件系统访问 | 高层 |

### 2. 层级关系

```
Harness（DeepAgents）→ 构建在 Framework（LangChain）之上 → Framework 构建在 Runtime（LangGraph）之上
```

**Runtime 是最底层**：提供持久执行、状态管理等基础设施
**Framework 在中间**：提供抽象和心智模型
**Harness 在最上层**：batteries included，"general purpose version of Claude Code"

### 3. Framework 的优劣

- **优势**：标准化抽象、快速上手、开发者可以在项目间迁移
- **风险**：抽象做不好会遮蔽底层工作方式、缺乏高级用例的灵活性

### 4. 关于"Agent Harness"这个术语

- Chase 承认这个概念不是他发明的——引用了 Vivek Trivedy 的文章
- 当时（2025-10）看到的通用 Harness 只有 DeepAgents 和 Claude Agent SDK
- 所有 Coding CLI 某种意义上都是 Agent Harness

## 与知识库中其他分类的对比

| 分类维度 | Chase 版本 | Foster 版本 |
|---------|-----------|-------------|
| 三层内容 | Framework / Runtime / Harness | Model / Agent / Harness |
| 关注点 | 软件组件的分类学（你用什么工具？） | 架构层的分类学（系统怎么分层？） |
| Harness 定义 | 比 Framework 更高层、开箱即用 | 给 Agent 一个"身体"的执行平台 |

两者互补：Chase 回答"我该选什么工具"，Foster 回答"我的系统该怎么分层"。

## 关联实体

- [[entities/hwchase17|Harrison Chase]] - 作者
- [[entities/harness-engineering|Harness Engineering]] - 术语源头之一

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]]
- [[entities/hwchase17|Harrison Chase]]
