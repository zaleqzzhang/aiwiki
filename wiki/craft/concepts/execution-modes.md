# 概念：Agent 执行模式（ReAct / Plan-and-Execute）

## 一句话

Agent 怎么"做事"的两种基本模式——边想边做（ReAct）还是先想后做（Plan-and-Execute）。

## 为什么需要这个概念

当你理解了 Harness 是"Agent 跑在什么上面"之后，下一个问题自然是：Agent 在 Harness 上怎么执行任务？

执行模式决定了 Agent 的**思考-行动节奏**。选错模式的代价很高：用 ReAct 做复杂规划任务会陷入局部最优（每步只看一步），用 Plan-and-Execute 做简单查询任务会过度规划（花 30 秒计划一个 5 秒能做完的事）。

从 Harness Engineering 的视角看，执行模式本质上就是不同的 Harness 架构——它定义了 Agent Loop 的循环结构、工具调用策略和终止条件。

## 与相邻概念的边界

- **vs Harness**：Harness 是运行时平台（沙箱、权限、上下文管理），执行模式是 Harness 内部的循环策略。一个 Harness 可以支持多种执行模式。
- **vs Prompt**：Prompt 影响模型"怎么想"，执行模式决定"想完之后怎么做"。你可以用同一个 Prompt 在 ReAct 和 Plan-and-Execute 两种模式下运行。
- **vs Multi-Agent**：Multi-Agent 是组织多个 Agent 协作，执行模式是单个 Agent 内部的工作方式。但 Plan-and-Execute 天然适合拆解为多 Agent（Planner Agent + Executor Agent）。

## 两种核心模式

### ReAct（Thought → Action → Observation 循环）

```
思考：我需要查找用户提到的文件
  ↓
行动：read_file("config.yaml")
  ↓
观察：文件内容是...
  ↓
思考：配置有误，需要修改第 12 行
  ↓
行动：edit_file(...)
  ↓
观察：修改成功
  ↓
完成
```

**核心特征**：
- **透明可审计**：每一步的推理过程都暴露在 Trace 中
- **实时适应**：每一步都基于最新的观察结果调整策略
- **适合不确定性高的任务**：探索性搜索、调试、信息收集
- **风险**：可能在局部循环中打转（没有全局视野）

### Plan-and-Execute（先规划，再执行）

```
规划阶段：
  1. 读取所有配置文件
  2. 分析依赖关系
  3. 按依赖顺序修改
  4. 运行测试验证
  ↓
执行阶段：
  按计划逐步执行，遇到偏差时可回到规划阶段修正
```

**核心特征**：
- **全局视野**：先看全貌再动手，避免局部最优陷阱
- **可并行化**：独立的执行步骤可以分给多个 Agent/线程
- **92% 任务完成率**，3.6x 加速（相比纯 ReAct，来源：design pattern 综述数据）
- **风险**：初始计划如果基于错误假设，后续执行会全错

## 设计直觉

选择执行模式的核心判断标准是：**执行路径能否提前确定？**

| 场景 | 推荐模式 | 原因 |
|------|---------|------|
| "帮我调查这个 Bug" | ReAct | 不知道 Bug 在哪，需要边查边推理 |
| "重构这个模块的 API" | Plan-and-Execute | 改动范围明确，需要全局协调 |
| "回答这个技术问题" | ReAct | 可能需要搜索、验证、综合 |
| "写一个完整的功能模块" | Plan-and-Execute | 需要考虑接口、测试、边界 |

**进阶**：最好的 Harness 不是二选一，而是**动态切换**。Anthropic 的长时运行 Agent 实践建议"两阶段方案"——先用 Plan-and-Execute 做高层规划，再用 ReAct 做每步的具体执行。

## 理解的关键转折

**执行模式不是 Agent 的属性，而是 Harness 的配置。**

同一个 LLM 底模，在 ReAct Harness 上表现像一个谨慎的调查员（每步验证），在 Plan-and-Execute Harness 上表现像一个果断的项目经理（先规划再执行）。"Agent 不会规划"——多数时候不是模型的问题，而是 Harness 没有提供规划的框架。

这回到了 Harness Engineering 的核心洞察：**模型是商品，Harness 是护城河**。执行模式的选择是 Harness 设计中最基础的架构决策之一。

## 展开时可用的例子

- **Claude Code 的 Agent Loop**：典型的 ReAct 模式——读文件 → 思考 → 编辑 → 运行测试 → 观察结果 → 继续。512,000 行代码中很大一部分在支撑这个循环的可靠运行。
- **Codex CLI 的两阶段方案**：先生成计划（Plan），展示给用户审批，然后执行（Execute）。在 Auto 模式下跳过审批直接执行。
- **Hermes 的 delegate_task**：父 Agent 做规划（哪些子任务、分给谁），子 Agent 用 ReAct 做执行。这是 Plan-and-Execute 的多 Agent 扩展。
- **Reflection Pattern 作为补充**：Generate → Reflect → Refine 可以叠加在任何执行模式上——ReAct 的每步可以加反思，Plan-and-Execute 的计划可以加审查。HumanEval 91% 的数据说明反思的价值。

## 相关页面

- [[entities/react-pattern|ReAct Pattern]] - 实体页面
- [[entities/plan-and-execute|Plan-and-Execute]] - 实体页面
- [[entities/reflection-pattern|Reflection Pattern]] - 补充模式
- [[topics/agent-design-patterns|Agent 设计模式]] - 五种模式全景
- [[craft/concepts/harness|概念：Harness]] - 执行模式是 Harness 的架构决策
- [[craft/gaps/harness-is-not-workflow|Harness ≠ Workflow]] - 主导权归属影响执行模式选择
- [[sources/agent-design-patterns-2026|5 Agent Design Patterns]] - 核心来源
