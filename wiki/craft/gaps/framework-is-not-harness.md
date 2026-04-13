# 认知缺口：Framework ≠ Harness

## 读者"以为懂"的认知

"LangChain 就是一个 Agent Harness 啊，它能构建 Agent、管理工具调用、处理记忆——不就是你说的 Harness 吗？"

## 实际区别

**Framework 是积木盒子，Harness 是你搭出来的房子。**

Harrison Chase 自己的分类学讲得最清楚：

| | Framework | Harness |
|---|----------|---------|
| **代表** | LangChain, Vercel AI SDK, CrewAI | Claude Code, DeepAgents, Codex CLI |
| **给你什么** | 抽象、心智模型、标准化构建方式 | 开箱即用的完整系统，batteries included |
| **你需要做什么** | 用它搭建你的系统 | 配置后直接用 |
| **默认行为** | 没有——你自己写 | 有——已经包含默认 Prompt、工具调用策略、记忆管理 |
| **层级** | 中间层 | 最高层（构建在 Framework 之上） |

LangChain 是 Framework → LangGraph 是 Runtime → DeepAgents 是 Harness。三者是叠加关系：Harness 构建在 Framework 之上。

**关键测试**：拿到一个 Framework，你需要自己写 System Prompt、设计工具调用策略、实现记忆管理、处理错误恢复。拿到一个 Harness，这些都已经做好了——你只需要告诉它"做什么"。

## 为什么误解常见

1. **术语混乱**：在 Harness Engineering 这个概念流行之前，很多人用 "framework" 泛指所有 Agent 工具。甚至 Chase 自己也承认边界模糊——"Part of the fun of developing in an early space is coming up with the mental models"
2. **Framework 在向 Harness 演化**：LangChain 推出 DeepAgents 就是 Framework 厂商向上走做 Harness 的例子。界限在模糊
3. **Claude Code 的特殊性**：它是 Harness（开箱即用），但很多人叫它 "Agent Framework" 甚至 "coding tool"——名字不重要，关键看它给你什么

## 正确心智模型

```
Framework（LangChain）← 你用它构建 → 你的 Harness（自建系统）
                                          OR
Harness（Claude Code）← 你直接使用 → 配置后开工
```

**选择标准**：
- 你的场景独特、需要高度定制 → 用 Framework 自建 Harness
- 你的场景是通用的（代码生成、文档处理）→ 直接用现成 Harness
- 你想两者兼得 → 用 Harness 起步，理解了之后用 Framework 定制

**最重要的区分**：Framework 不包含"有主见的默认行为"（opinionated defaults）。Harness 包含。Claude Code 不是给你一堆 API 让你自己拼——它已经决定了怎么管理上下文、怎么处理工具调用、怎么做安全防护。你可以覆盖这些默认行为，但起点不是零。

## 文章切入点

开头可以用一个类比：
> "你不会管 React 叫'一个网站'——React 是 framework，你的网站是你用 React 搭出来的。同样，LangChain 不是 Harness——LangChain 是 framework，你的 Agent 系统是你用 LangChain 搭出来的 Harness。"

然后用 Chase 自己的分类学展开三层（Framework / Runtime / Harness），最后用 Foster 的三层（Model / Agent / Harness）补充"从系统视角看 Harness"。

## 相关页面

- [[craft/concepts/harness|概念：Harness]] - Harness 的完整定义
- [[sources/frameworks-runtimes-harnesses|Agent Frameworks, Runtimes, and Harnesses]] - Harrison Chase 的分类学
- [[sources/three-layers-model-agent-harness|Model, Agent, Harness: Three Layers]] - Tom Foster 的分层
- [[entities/harness-engineering|Harness Engineering]] - 方法论框架
