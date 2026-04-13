# Tom Foster

**类型**: person
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 2

## 定义

Contextua.dev 的作者，Agent 架构分析者。以清晰的三层定义（Model / Agent / Harness）和五款 Harness 对比分析著称。提出 Reasoning-First vs Environment-First 两大设计流派分类。

## 核心观点

- 大多数人用"Agent"泛指整个系统，导致架构讨论、故障排查、协作沟通时概念混乱
- 三层架构的价值在于**可组合性**（层间可独立替换）和**诊断性**（失败时精准定位到层）
- 大部分日常 Agent 失败是 Layer 3（Harness）问题——上下文组装不对、状态丢失、工具接口误导
- Harness 架构决定 Agent 能力的**天花板**和可靠性的**地板**
- 五款 Harness 归纳为两大流派：**Reasoning-First**（信任模型，最小脚手架）vs **Environment-First**（投资环境可读性）
- "Compare harnesses. The model is the engine. The harness is everything else."

## 与其他实体的关系

- [[entities/harness-engineering|Harness Engineering]] → 提供理论基础和实践对比
- [[entities/hwchase17|Harrison Chase]] → 与 Chase 的三层架构（Model/Harness/Context）形成互补视角

## 相关来源

- [[sources/three-layers-model-agent-harness|Model, Agent, Harness: Three Layers]] - 三层架构精确定义
- [[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]] - 五款 Harness 对比分析、两大设计流派

## 开放问题

- Foster 的"Agent 层"与 Chase 的"Harness 层"存在边界差异——Chase 把推理循环归入 Harness，Foster 把推理循环独立为 Agent 层。哪种划分更有实践价值？
