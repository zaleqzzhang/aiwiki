# OpenSpec、Superpowers 和 Harness：AI 工程化开发的三层拼图

**类型**: article
**日期**: 2026-04-09
**来源**: https://mp.weixin.qq.com/s/ssH_OtuLxy4RZD2tKiSKXA
**作者**: 岚天逸剑
**标签**: #harness #multi-agent #openspec #superpowers #spec-driven-development

## 摘要

AI 编程从"让模型写代码"到"让模型像团队一样开发"，中间差的不是更强的模型，而是三层工程基础设施。文章提出一个清晰的三层拼图模型：**OpenSpec 管"做什么"（规范层）、Superpowers 管"怎么做"（纪律层）、Harness 管"谁来做、怎么协同"（协作层）**。三者各管一层，合在一起才是完整的工程化开发体系。

文章核心论点：大多数 AI 编程的问题不在模型能力，而在于缺少系统化的工程基础设施。三层的配合原则是"规范不变、流程不变、团队可弹性扩展"——OpenSpec 和 Superpowers 定义稳定的标准和纪律，Harness 定义灵活的团队结构和调度策略。

## 关键要点

### 三层模型

| 层次 | 工具 | 管什么 | 类比 |
|------|------|--------|------|
| **规范层** | OpenSpec | 做什么——需求、接口、验收标准 | 施工图纸 |
| **纪律层** | Superpowers | 怎么做——TDD、代码审查、验证流程 | 施工规范手册 |
| **协作层** | Harness / Agent Team | 谁来做——角色分工、任务调度、权限管控 | 项目经理 + 工地管理 |

### OpenSpec（规范层）

- Fission-AI 开源的 Spec-Driven Development 框架
- 工作流：propose → spec → verify → archive
- 输出结构化的 `openspec/` 目录：提案、规格、任务拆解、验证结果
- 核心价值：需求从聊天记录变为结构化文档，多 Agent 共享统一规范，验收标准明确

### Superpowers（纪律层）

- obra 开源的 AI 编程技能系统
- 核心技能：TDD、writing-plans、code-review、verification-before-completion、brainstorming
- 纯 Markdown 定义，零依赖，按需加载（避免上下文膨胀）
- 核心价值：开发流程有纪律，强制遵循工程最佳实践

### Harness（协作层）

- 通过 `AGENTS.md` 定义角色（架构师、后端、测试、安全、Team Lead）
- Team Lead Agent 读取 OpenSpec 任务列表进行分派
- 权限隔离 + 硬约束（提交前必须跑测试/Lint/安全扫描）
- 反馈回路：测试失败自动回灌给开发 Agent

### 完整开发周期

```
Phase 1 规范：OpenSpec 定义需求/接口/验收标准 → openspec/ 目录
Phase 2 分工：Harness 组建 Agent Team → Architect 读 spec 输出架构 → Team Lead 分派
Phase 3 开发：各 Agent 按 Superpowers 纪律开发 → TDD + 审查 + 验证
Phase 4 验收：OpenSpec 验证产出 → 通过归档 / 不通过打回
```

### 缺层后果

| 缺什么 | 后果 |
|--------|------|
| 缺 OpenSpec | 需求理解不一致，返工率高 |
| 缺 Superpowers | Agent 走野路子，代码质量不可控 |
| 缺 Harness | 协作混乱，权限冲突，任务重复 |
| 三个都缺 | 裸模型写代码——能跑但不敢用 |

### 落地建议

优先级排序：
1. **先上 OpenSpec**——哪怕只是手写一份 spec
2. **再上 Superpowers 核心技能**——至少 TDD + verification-before-completion
3. **最后上 Harness / Agent Team**——等规范和纪律稳定了再上协作

避坑：Superpowers 按需加载，`AGENTS.md` 写精简，避免 Superpowers 与 Harness 能力重叠

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 三层拼图中的协作层
- [[entities/skill-system|Skill 系统]] - Superpowers 是 Skill 的一种表现形式（纪律型 Skill）

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]]
- [[topics/multi-agent-collaboration|多 Agent 协作]]
