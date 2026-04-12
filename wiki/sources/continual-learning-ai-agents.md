# Continual Learning for AI Agents

**类型**: article
**作者**: [[entities/hwchase17|Harrison Chase]]
**日期**: 2026-04-05
**来源**: https://x.com/hwchase17/status/2040467997022884194
**标签**: #ai-agent #continual-learning #architecture

## 摘要

大多数关于持续学习的讨论聚焦于更新模型权重，但对于 AI Agent，学习可以发生在三个不同层次：模型层、Harness 层、上下文层。理解这一区别会改变我们对"可改进系统"的构建思路。

Agent 系统的三层架构：
- **Model（模型层）**：模型权重本身
- **Harness（Harness 层）**：驱动 Agent 的代码 + 固定的指令和工具
- **Context（上下文层）**：Harness 之外的配置性内容（指令、技能、工具），也称为"记忆"

## 关键要点

### 模型层持续学习

- 技术手段：SFT、RL（如 GRPO）
- 核心挑战：**灾难性遗忘**（Catastrophic forgetting）— 模型在新数据上更新后，会退化之前的能力
- 实践层面：通常在 Agent 整体层面进行（如 OpenAI Codex 模型），理论上可更细粒度（如每个用户一个 LoRA），但实践中少见

### Harness 层持续学习

- Harness = 驱动 Agent 的代码 + 固定指令/工具
- 优化方法：Meta-Harness — 让 Agent 在一批任务上运行 → 评估 → 存储日志 → 编码 Agent 分析 trace 并建议修改 Harness 代码
- 通常在 Agent 层面进行，理论上可细分到用户层面

### 上下文层持续学习

- 上下文 = Harness 外的配置（指令、技能、工具），也即"记忆"
- 更新可在不同粒度进行：
  - **Agent 层面**：Agent 自己的持久记忆（如 OpenClaw 的 [[entities/soul-md|SOUL.md]]）
  - **租户层面**：用户/组织/团队级别的独立上下文（如 Hex Context Studio、Decagon Duet、Sierra Explorer）
  - **混合模式**：Agent 层 + 用户层 + 组织层同时更新

- 更新方式：
  - **离线批处理**：分析近期 trace → 提取洞察 → 更新上下文（OpenClaw 称为"做梦" dreaming）
  - **热路径更新**：Agent 在执行任务过程中直接更新记忆

- 另一维度：更新是显式的（用户提示记住）还是隐式的（Agent 自主决定）

### Trace 是核心

所有流程都依赖 **trace** — Agent 完整执行路径的记录。

- 更新模型：收集 trace → 与 Prime Intellect 等合作训练模型
- 改进 Harness：用 LangSmith CLI + Skills 让编码 Agent 访问 trace → 分析并修改 Harness
- 学习上下文：Harness 需支持此功能，如 Deep Agents 支持用户级记忆、后台学习等

## 实例映射

**Claude Code**:
- Model: claude-sonnet 等
- Harness: Claude Code
- Context: CLAUDE.md, /skills, mcp.json

**OpenClaw**:
- Model: 多种
- Harness: Pi + 其他脚手架
- Context: SOUL.md, clawhub skills

## 三层对比

| 层级 | 更新对象 | 粒度 | 更新方式 | 挑战/特点 |
|------|----------|------|----------|-----------|
| Model | 模型权重 | 通常 Agent 级 | SFT、RL | 灾难性遗忘 |
| Harness | 代码 + 固定指令/工具 | 通常 Agent 级 | Meta-Harness（编码 Agent 分析 trace） | 需要 trace + 自动化改进流程 |
| Context | 外部配置（指令、技能） | Agent / 用户 / 组织 级 | 离线批处理 或 热路径更新 | 最灵活，最常用 |

## 关联实体

- [[entities/hwchase17|Harrison Chase]] - 作者，LangChain 创始人
- [[entities/langchain|LangChain]] - 相关框架
- [[entities/langsmith|LangSmith]] - trace 收集平台
- [[entities/openclaw|OpenClaw]] - 上下文层学习的案例
- [[entities/deep-agents|Deep Agents]] - LangChain 的开源 Harness
- [[entities/soul-md|SOUL.md]] - Agent 层面的持久记忆模式
- [[entities/meta-harness|Meta-Harness]] - Harness 层优化方法
- [[entities/catastrophic-forgetting|灾难性遗忘]] - 模型层学习的核心挑战

## 引用此来源的页面

*待其他页面创建后补充*

## 相关主题

- [[topics/agent-architecture|Agent 架构]]
- [[topics/memory-systems|记忆系统]]
- [[topics/continual-learning|持续学习]]
