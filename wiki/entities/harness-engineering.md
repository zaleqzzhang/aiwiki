# Harness Engineering

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 定义

Harness Engineering 是构建可靠 Agent 系统的方法论框架。Harness 是 Agent 的"操作系统"——管理记忆、约束架构、评估质量的底层系统。核心理念：**竞争优势不再是 prompt，而是 Harness 捕获的轨迹。**

## 三根支柱

### 1. 记忆治理 → 解决 Agent 失忆症

**问题**：Agent 上下文窗口是"内存"，关机就没了。每次对话都要重新解释项目背景、偏好、历史。

**解决**：给 Agent 搭一套持久化记忆系统。

**实现路径**：
- 有界热记忆 + 冷检索（Hermes）
- 无界记忆 + 自动捕获（MemOS）

### 2. 架构约束 → 明确 Agent 职责边界

**问题**：单个 Agent 什么都干，结果什么都干不好。上下文爆炸，任务混乱。

**解决**：用架构约束明确职责边界。

**实现路径**：
- 主 Agent + 子 Agent 并行（Hermes）
- 多常驻 Agent + 工蜂系统（MemOS）

### 3. 评估闭环 → 保证 Agent 质量

**问题**：Agent 做完了任务，但不知道做得对不对。时间长了，质量漂移。

**解决**：建立评估闭环，让 Agent 能自检、能从失败中学习。

**实现路径**：
- Cron 调度 + 消息推送（Hermes）
- 心跳检查 + 智库审核 + 反思系统（MemOS）

## 核心洞察

**轨迹是核心资产**：
- 每次 Agent 的成功和失败都是训练下一代的数据
- Harness 捕获的轨迹 = 竞争优势
- 这不是靠换模型能追上的

**系统强制 vs Agent 自觉**：
- 依赖 Agent 自觉 → 捕获率只有 60%
- 系统 Hook 强制 → 捕获率 100%

**长期 ROI**：
- 短期使用（< 3 个月）→ 用现成框架（Hermes）
- 长期陪伴（> 6 个月）→ 自建 Harness 积累轨迹

## 与其他概念的关系

- [[entities/hermes|Hermes]] → Harness Engineering 的一种实现
- [[entities/memos|MemOS]] → Harness Engineering 的另一种实现
- [[entities/hook-mechanism|Hook 机制]] → 实现轨迹捕获的关键
- [[entities/skill-system|Skill 系统]] → 轨迹提炼为可复用经验
- [[topics/agent-architecture|Agent 架构]] → Harness 是架构的底层系统

## 相关来源

- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]] - 基于 Harness Engineering 三根支柱的深度对比
- [[sources/llm-training-pipeline|大模型训练：原理、路径与新实践]] - 将 Harness Engineering 作为独立优化对象，引用 Meta-Harness 论文
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - 系统化 Harness 的 12 个组件和三层工程定义

## 实践启发

对于 Atlas / WorkBuddy：
- 当前：有 SOUL.md、IDENTITY.md、USER.md 作为记忆
- 缺失：没有系统的轨迹捕获机制
- 改进：
  1. 引入 Hook 机制（每次任务结束自动捕获）
  2. 建立评估闭环（心跳检查、质量审核）
  3. 自动提炼 Skill（从轨迹中学习）

## 开放问题

- 三根支柱的优先级如何排序？
- 如何量化 Harness 的 ROI？
- 不同规模团队应该如何选择实现路径？
