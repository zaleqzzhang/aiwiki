# Reflection Pattern

**类型**: concept
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 定义

Reflection 是一种让 Agent 自我审查和修正输出的设计模式。核心循环：Generate（生成初稿）→ Reflect（按标准自评，找出问题）→ Refine（根据评审修正），迭代至质量阈值。

## 核心特征

- **自动质控**：无需人工介入即可提升输出质量
- **系统性纠错**：捕捉逻辑不一致、遗漏、结构问题
- **可迭代**：固定次数或质量阈值终止
- **可组合**：可嵌入 ReAct 循环或 Plan-and-Execute 的执行阶段

## 性能数据

| 基准 | 无 Reflection | 有 Reflection | 提升 |
|------|-------------|--------------|------|
| HumanEval（编码） | 80% | 91% | +11pp |
| 通用任务（对话/数学/推理） | — | — | ~+20pp |
| 结合外部验证工具 | — | — | +10-30pp |

## 局限

- **自我评判的天花板**：Agent 仍在评判自己的输出，没有外部 grounding 时可能强化错误假设
- **延迟翻倍**：每轮 Reflection 至少多一次 LLM 调用
- **质量 vs 速度权衡**：只适合对质量要求高于响应速度的场景

## 与其他实体的关系

- [[ReAct]] → 可嵌入：ReAct 的 Final Answer 阶段加 Reflection
- [[Multi-Agent Collaboration]] → Loop 模式本质就是多 Agent 版 Reflection（Generator + Critic + Refiner）
- [[Trace]] → Reflection 产生的 critique 本身就是高价值 trace 数据

## 相关来源

- [[sources/agent-design-patterns-2026|5 Agent Design Patterns]] - 含 HumanEval 性能对比

## 开放问题

- 何时 Reflection 会降低质量？（过度迭代导致"过拟合"到自身偏见）
- 外部 Reflection（用另一个模型评审）vs 自我 Reflection 的成本效益对比？
