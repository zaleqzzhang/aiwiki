# 概念：持续学习的三层架构

## 一句话

Agent 可以在三个层次上"变聪明"：改模型权重、改运行代码、改配置和记忆——成本递减，灵活性递增。

## 为什么需要这个概念

"让 Agent 持续学习"听起来像一个单一问题，但实际上它包含三个完全不同的工程问题。混为一谈会导致要么过度工程（以为必须微调模型才能改进），要么过度简化（以为改改 Prompt 就够了）。

Harrison Chase 提出的三层划分——Model、Harness、Context——清晰地界定了"改进 Agent"的三条路径，每条路径的成本、风险、灵活性截然不同。理解这个划分，会从根本上改变你构建"可改进系统"的思路。

## 与相邻概念的边界

- **vs 传统机器学习的持续学习**：传统持续学习主要讨论模型权重更新和灾难性遗忘。Agent 的持续学习范围大得多——大部分改进发生在 Harness 和 Context 层，根本不需要碰模型权重。
- **vs Fine-tuning**：Fine-tuning 是三层中最"重"的一种（模型层），Agent 系统中更常用的是 Context 层的轻量更新。

## 设计直觉

三层的设计逻辑可以用一个类比理解：

| 层次 | 类比 | 改什么 | 成本 | 风险 |
|------|------|--------|------|------|
| Model 层 | 换大脑 | 模型权重 | 高（训练资源） | 高（灾难性遗忘） |
| Harness 层 | 换工具箱 | 代码 + 固定指令 | 中（trace 分析） | 中（可能引入 bug） |
| Context 层 | 换笔记 | 配置、记忆、技能 | 低（更新文件） | 低（随时可回退） |

**关键洞察**：大多数实际改进发生在 Context 层。你不需要重新训练 Claude 就能让它记住你的项目偏好——只需要更新 CLAUDE.md。

这不意味着 Model 层不重要。而是说，作为 Agent 系统的构建者，你应该**先把 Context 层和 Harness 层做好**，因为这是你能直接控制的。Model 层的改进交给模型提供商。

## 理解的关键转折

**Trace 是所有层改进的共同数据源。**

- 改模型：收集 trace → 用于 RL/SFT 训练数据
- 改 Harness：收集 trace → 编码 Agent 分析 → 建议修改代码（Meta-Harness）
- 改 Context：收集 trace → 提取洞察 → 更新记忆/技能

所以，不管你选择在哪一层改进，**第一步永远是：确保你在捕获高质量的 trace**。没有 trace，三层改进都无从谈起。

这也解释了为什么 Hook 机制如此重要——它保证了 trace 的完整性。

## 展开时可用的例子

- **Claude Code 的三层**：Model = Claude Sonnet；Harness = Claude Code 本体（512K 行代码）；Context = CLAUDE.md + /skills + mcp.json。用户通常改的是 Context 层（写 CLAUDE.md），Anthropic 改的是 Harness 层（升级 Claude Code），模型层的更新则是独立的模型发布。
- **OpenClaw 的"做梦"机制**：离线分析近期 trace → 提取洞察 → 更新 SOUL.md。这是 Context 层持续学习的典型实现——不改模型、不改代码，只更新 Agent 的配置文件。
- **Meta-Harness 的 6 倍差距**：同一个底模，只改 Harness 代码，SWE-Bench 上性能差 6 倍。这证明了 Harness 层改进的巨大潜力。

## 相关页面

- [[entities/harness-engineering|Harness Engineering]] - Harness 层的方法论
- [[entities/meta-harness|Meta-Harness]] - Harness 层学习的具体方法
- [[entities/catastrophic-forgetting|灾难性遗忘]] - Model 层学习的核心挑战
- [[topics/continual-learning|持续学习]] - 主题页面
- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - Harrison Chase 原文
