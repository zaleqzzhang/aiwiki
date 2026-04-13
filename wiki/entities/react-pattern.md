# ReAct Pattern

**类型**: concept
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 定义

ReAct（Reasoning + Acting）是一种 Agent 设计模式，让 LLM 在推理与行动之间交替执行：先思考当前状态和下一步策略，再调用工具获取真实数据，观测结果后继续推理。核心循环：Thought → Action → Observation → ... → Final Answer。

## 核心特征

- **透明可审计**：每步决策生成 Thought 日志，可追踪、可调试
- **自适应工具选择**：Agent 自主决定用哪个工具、什么顺序，而非固定脚本
- **观测纠错**：用真实数据（Observation）纠正推理偏差，减少幻觉
- **短周期最优**：3-5 步 Action 内效率最高，长周期任务应考虑 Plan-and-Execute

## 工作机制

```
Thought: "我需要查 X 才能回答 Y"
  → Action: 调用搜索工具，查询 X
    → Observation: 搜索返回结果
      → Thought: "结果显示 Z，我可以综合回答了"
        → Final Answer
```

## 性能数据

- 任务完成率约 85%（vs Plan-and-Execute 92%）
- Token 消耗 2000-3000/任务（vs Plan-and-Execute 3000-4500）
- API 调用 3-5 次/任务（vs Plan-and-Execute 5-8 次）
- 优势在于**单步成本低**，劣势在于**复杂任务效率低**

## 局限

- 延迟随循环次数线性增长
- 可能陷入无限循环（需设迭代上限）
- 缺乏长期规划能力——每步只看一步

## 与其他实体的关系

- [[Plan-and-Execute]] → 互补关系：ReAct 处理短周期，Plan-and-Execute 处理长周期
- [[Reflection]] → 可组合：ReAct 循环内嵌入 Reflection 提升输出质量
- [[Tool Use]] → 依赖关系：ReAct 的 Action 阶段本质就是 Tool Use

## 相关来源

- [[sources/agent-design-patterns-2026|5 Agent Design Patterns]] - 首个详细介绍，含对比数据

## 开放问题

- ReAct + Plan-and-Execute 的混合架构（短任务 ReAct，长任务自动切换规划模式）是否已有成熟实现？
- 循环上限设多少合适？不同任务类型是否需要不同的上限策略？
