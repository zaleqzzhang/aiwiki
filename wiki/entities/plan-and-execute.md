# Plan-and-Execute

**类型**: concept
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 定义

Plan-and-Execute 是一种将战略推理与战术执行分离的 Agent 设计模式。Planner 先生成完整任务分解（通常是 DAG），Executor 逐步执行子任务，Re-planner 在执行偏离时动态调整计划。

## 核心特征

- **规划先行**：不像 ReAct 边走边看，而是先全局规划再动手
- **DAG 任务图**：支持依赖追踪和并行执行（LangChain LLMCompiler 实现）
- **异构执行**：Planner 用强模型，Executor 可用更小更便宜的模型
- **动态调整**：Re-planner 在执行偏离时重新规划，不死守原计划

## 性能数据

| 指标 | Plan-and-Execute | ReAct |
|------|-----------------|-------|
| 任务完成率 | 92% | 85% |
| 执行速度 | 3.6x 加速 | 基准 |
| Token 消耗 | 3000-4500 | 2000-3000 |
| API 调用次数 | 5-8 | 3-5 |

## 架构

```
User Task
  → Planner (强模型，生成 DAG)
    → Executor 1 (子任务 A)
    → Executor 2 (子任务 B, 依赖 A)
    → Executor 3 (子任务 C, 可与 B 并行)
  → Re-planner (检查执行结果，决定是否调整)
    → 完成 / 生成后续计划
```

## 局限

- 前期规划的 token 开销固定存在，简单任务不划算
- 规划质量高度依赖 Planner 模型能力
- Re-planning 机制的触发条件需要精心设计

## 与其他实体的关系

- [[ReAct]] → 互补关系：简单任务用 ReAct，复杂任务用 Plan-and-Execute
- [[Multi-Agent Collaboration]] → 可嵌套：每个 Executor 可以是独立 Agent
- [[Harness]] → Plan-and-Execute 本身就是一种 Harness 架构——LLM 的能力由"规划+执行+重规划"的编排逻辑决定

## 相关来源

- [[sources/agent-design-patterns-2026|5 Agent Design Patterns]] - 含性能对比数据

## 开放问题

- Re-planner 应该在什么粒度上触发？每步检查 vs 偏差阈值触发？
- Plan-and-Execute 与 Harness 概念的关系——是否所有 Harness 本质上都是 Plan-and-Execute 的变体？
