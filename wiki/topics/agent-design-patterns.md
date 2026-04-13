# Agent 设计模式

**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 概述

Agent 设计模式是构建 AI Agent 系统的可复用架构方案。不同模式解决不同问题：推理透明性、多步协调、专业化分工、输出质量、真实世界交互。选择正确的模式是 Agent 项目成败的关键——预测 40% 的 Agent 项目可能因架构选择失误在 2027 年被取消。

## 五种核心模式

### 1. ReAct（推理-行动循环）
- **解决**：需要透明可审计的推理 + 工具调用
- **机制**：Thought → Action → Observation 循环
- **最佳场景**：3-5 步内的工具辅助推理任务
- 详见 [[entities/react-pattern|ReAct Pattern]]

### 2. Plan-and-Execute（规划与执行分离）
- **解决**：复杂多步任务的高效协调
- **机制**：Planner 生成 DAG → Executor 执行 → Re-planner 调整
- **最佳场景**：有明确依赖结构的多步工作流
- 详见 [[entities/plan-and-execute|Plan-and-Execute]]

### 3. Multi-Agent Collaboration（多 Agent 协作）
- **解决**：超出单 Agent 能力的复杂问题
- **三种子模式**：
  - Sequential（顺序管道）
  - Parallel（并行扇出/汇入）
  - Loop（迭代精炼循环）
- **最佳场景**：需要专业化分工的大型系统

### 4. Reflection（自我反思）
- **解决**：输出质量不够高
- **机制**：Generate → Reflect → Refine 循环
- **最佳场景**：质量重于速度的场景（代码生成、内容创作）
- 详见 [[entities/reflection-pattern|Reflection Pattern]]

### 5. Tool Use（工具使用）
- **解决**：LLM 训练数据的边界限制
- **机制**：函数调用（schema → 选择 → 结构化调用 → 执行 → 回注）
- **判断**："对生产级 Agent 几乎不可谈判"

## 模式选择框架

| 最大瓶颈 | 推荐模式 | 理由 |
|---------|---------|------|
| 推理不透明，难调试 | ReAct | 每步有 Thought 日志 |
| 多步任务效率低 | Plan-and-Execute | 前置规划 + 并行执行 |
| 单 Agent 能力不够 | Multi-Agent | 专业化分工 + 混用模型 |
| 输出质量不达标 | Reflection | 自动 QA 循环 |
| 需要真实世界数据 | Tool Use | 函数调用扩展能力 |

## 模式间的关系

这五种模式不是互斥的，而是可组合的：
- ReAct + Tool Use：最基础的组合，几乎所有 Agent 都在用
- Plan-and-Execute + Multi-Agent：Executor 可以是独立 Agent
- Multi-Agent (Loop) ≈ Reflection 的多 Agent 版本
- ReAct + Reflection：在 Final Answer 前加自评

## 与 Harness 概念的关系

从 Harness Engineering 的视角看，这些设计模式本质上都是**不同的 Harness 架构**——它们定义了 LLM 的调用方式、上下文管理和执行流程。选择设计模式就是选择 Harness。

详见 [[entities/harness|Harness]]、[[topics/harness-engineering|Harness Engineering]]

## 相关来源

- [[sources/agent-design-patterns-2026|5 Agent Design Patterns (2026)]] - 五种模式综合概览，含性能对比

## 待探索方向

- 模式的自动切换：Agent 能否根据任务复杂度自动选择 ReAct vs Plan-and-Execute？
- Reflection 的过拟合问题：何时自我反思会降低质量？
- 混合架构的最佳实践：如何优雅地组合多种模式？
- 各模式在不同领域（编码、写作、研究、客服）的实际效果对比
