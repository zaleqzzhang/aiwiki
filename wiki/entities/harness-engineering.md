# Harness Engineering

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-13
**来源数量**: 4

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

## 层级关系：Prompt ⊂ Context ⊂ Harness

三层工程方法论不是平行选择，而是嵌套关系：

| 层 | 核心问题 | 范围 |
|----|---------|------|
| Prompt Engineering | "How should I phrase this?" | 单次交互 |
| Context Engineering | "What does the model need to know?" | 多轮信息流 |
| Harness Engineering | "How do agents operate reliably over thousands of inferences?" | 跨天/周的多步骤系统 |

- Prompt 处理指令措辞，是 Context 的子集
- Context 决定模型看到什么信息，是 Harness 的子集
- Harness 还管"系统防止什么、度量什么、控制什么、修复什么"

实践策略：先 Prompt 快速见效 → 加 Context 处理复杂工作流 → 上 Harness 保障生产。

## Boeckeler 三支柱

Birgitta Boeckeler 提出的另一个三支柱框架（与 Hermes vs MemOS 对比版有重叠但侧重不同）：

1. **Context Engineering**：持续增强的知识库 + 动态可观测数据
2. **Architectural Constraints**：确定性 Linter 和结构化测试强制边界
3. **Garbage Collection**：周期性 Agent 扫描文档漂移和约束违规

差异：Boeckeler 版本强调 Garbage Collection（自动修复漂移），Hermes/MemOS 版本强调评估闭环（质量度量）。两者互补。

## 三层拼图模型（岚天逸剑）

当 Harness 进入多 Agent 开发场景，其职责可以进一步分解为三层：

| 层次 | 工具 | 管什么 | 类比 |
|------|------|--------|------|
| **规范层** | OpenSpec | 做什么——需求、接口、验收标准 | 施工图纸 |
| **纪律层** | Superpowers | 怎么做——TDD、代码审查、验证流程 | 施工规范手册 |
| **协作层** | Harness / Agent Team | 谁来做——角色分工、任务调度、权限管控 | 项目经理 + 工地管理 |

**配合原则**："规范不变、流程不变、团队可弹性扩展"——OpenSpec 和 Superpowers 定义稳定的标准和纪律，Harness 定义灵活的团队结构和调度策略。

**落地优先级**：
1. 先上 OpenSpec（需求结构化）→ 2. 再上 Superpowers 核心技能（TDD + verification）→ 3. 最后上 Harness / Agent Team（多 Agent 协作）

**与前述三支柱的关系**：Harness Engineering 三支柱（记忆治理 / 架构约束 / 评估闭环）描述的是 Harness 的**内部组成**；三层拼图描述的是 Harness 在多 Agent 开发场景中的**外部分工**。两者互补。

## 核心洞察

**量化证据——Harness > Prompt**：
- Princeton 研究：仅改变 harness 配置（不改模型），solve rate 提升 64%
- 同一 Claude Opus 4.5，不同 harness：2% vs 12%——6x 性能差距完全归因于环境设计
- MIT 统计：约 95% 大企业 AI 试点未能产出可度量回报，根源是架构缺口而非 Prompt 质量

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

## 长时运行实践

### 跨上下文窗口挑战

**核心问题**：上下文窗口有限，复杂项目无法在单个窗口完成。每个新会话从零记忆开始。

**两大失败模式**：
| 模式 | 表现 | 后果 |
|------|------|------|
| One-shot 症候群 | 试图一次性完成所有任务 | 上下文耗尽，功能半实现 |
| 过早宣布完成 | 看到进展就宣布项目完成 | 遗漏功能，质量不达标 |

### 两阶段解决方案

**Initializer Agent（首次会话）**：
- 创建 `init.sh` 脚本（运行开发服务器）
- 创建进度文件（如 `claude-progress.txt`）
- 初始 Git commit（展示新增文件）
- 编写 Feature List（JSON 格式功能清单）

**Coding Agent（后续会话）**：
- 增量推进：每次只做一个功能
- 保持清洁状态：代码可合并到主分支
- 会话结束：Git commit + 进度更新

### 会话启动例程

```
1. pwd → 确认工作目录
2. 读 Git logs 和进度文件 → 了解最近工作
3. 读 Feature List → 选最高优先级未完成功能
4. 运行 init.sh → 启动开发服务器
5. 基础端到端测试 → 验证现有功能正常
6. 开始新功能开发
```

### 设计原则

- **增量优于一次性**：避免上下文耗尽
- **清洁状态**：每次会话结束时代码可合并
- **JSON 优于 Markdown**：模型更难不当修改结构化文件
- **显式优于隐式**：强指令防止模型偷懒

## 相关来源

- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]] - 基于 Harness Engineering 三根支柱的深度对比
- [[sources/llm-training-pipeline|大模型训练：原理、路径与新实践]] - 将 Harness Engineering 作为独立优化对象，引用 Meta-Harness 论文
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - 系统化 Harness 的 12 个组件和三层工程定义
- [[sources/long-running-agents|Effective Harnesses for Long-Running Agents]] - Anthropic 官方长时运行 Agent 实践
- [[sources/prompt-vs-context-vs-harness|Prompt vs Context vs Harness Engineering]] - 三层嵌套层级关系 + Princeton 64% 数据 + Boeckeler 三支柱
- [[sources/openspec-superpowers-harness|OpenSpec、Superpowers 和 Harness]] - 多 Agent 开发场景的三层拼图模型

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
