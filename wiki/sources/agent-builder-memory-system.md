# Agent Builder 的记忆系统

**类型**: article
**日期**: 2026-01-15
**来源**: https://x.com/hwchase17/status/2011814697889316930
**作者**: [[entities/hwchase17|Harrison Chase]]
**标签**: #Memory #AgentBuilder #DeepAgents

## 摘要

LangSmith Agent Builder 是一个无代码 Agent 构建平台，基于 Deep Agents harness。文章详细阐述了为什么优先构建记忆系统、技术实现细节、经验教训以及未来方向。核心洞察：专用 Agent（反复做同一任务）比通用 Agent 更需要记忆，因为一次会话的经验可以立即应用到下一次。

## 关键要点

### 为什么优先构建记忆系统

**通用 Agent vs 专用 Agent**：

| 维度 | 通用 Agent（ChatGPT/Claude/Cursor） | 专用 Agent（Agent Builder） |
|------|--------------------------------------|----------------------------|
| 任务范围 | 各种不相关任务 | 反复做同一任务 |
| 经验复用率 | 低（上次会话可能无关） | 高（经验直接适用下次） |
| 记忆重要性 | 一般 | 关键 |
| 无记忆体验 | 尚可接受 | 非常糟糕（每次重新解释） |

**LangSmith Agent Builder 的定位**：
- 目标用户：技术轻量级的公民开发者
- 用途：自动化特定工作流（邮件助手、文档助手等）
- 基础：Deep Agents harness

### 记忆系统的技术实现

**基于文件的设计**：
```
memory/
├── preferences/
│   ├── communication-style.md
│   └── output-format.md
├── learnings/
│   ├── successful-approaches.md
│   └── mistakes-to-avoid.md
└── context/
    ├── user-background.md
    └── project-context.md
```

**设计原则**：
1. 文件即记忆（易移植、易理解、易调试）
2. 使用标准约定（agents.md, skills）
3. Agent 直接读写文件（不依赖外部服务）

**记忆类型**：
- Semantic Memory（语义记忆）：事实、偏好、知识
- Procedural Memory（程序记忆）：如何做某事
- Episodic Memory（情景记忆）：过去行为序列（计划中）

### 经验教训

**教训 1：记忆更新时机**
- 热路径更新：Agent 运行时即时更新
- 问题：Agent 可能未识别重要信息
- 解决：添加后台进程（每日 cron）反思所有会话

**教训 2：记忆粒度**
- 单个大文件 vs 多个小文件
- 选择：多小文件（易搜索、易更新、易管理）

**教训 3：记忆访问方式**
- 语义搜索 vs 文件系统操作（glob/grep）
- 当前：文件系统操作（简单可靠）
- 未来：添加语义搜索

**教训 4：记忆层次**
- 当前：所有记忆特定于 Agent
- 缺失：用户级、组织级记忆
- 计划：通过不同目录暴露不同层次

### 记忆系统带来的能力

**持续改进**：
- 记录成功方法 → 下次自动应用
- 记录失败教训 → 下次自动避免

**更简单的迭代**：
- 无需手动更新配置
- 用自然语言反馈 → Agent 自动更新记忆

**便携性**：
- 文件格式易于移植
- 可迁移到其他 harness（Deep Agents CLI, Claude Code, OpenCode）
- 使用标准约定保证兼容性

### 未来方向

**情景记忆（Episodic Memory）**：
- 将过去的会话作为文件暴露给 Agent
- Agent 可以查看自己的历史行为序列

**后台记忆进程**：
- Cron job，每日运行
- 反思所有会话，更新记忆
- 捕获 Agent 当时未识别的重要信息

**显式 /remember 命令**：
- 用户提示 Agent 反思并更新记忆
- 发现偶尔使用效果很好

**语义搜索**：
- 当前用 glob/grep 搜索记忆
- 添加语义搜索以处理复杂查询

**多级记忆**：
- Agent 级（当前）
- 用户级（跨 Agent 共享）
- 组织级（团队共享）

## 核心框架

```
记忆系统架构
├── 设计原则
│   ├── 文件即记忆
│   ├── 标准约定
│   └── Agent 直接读写
├── 记忆类型
│   ├── Semantic（语义）→ 事实、偏好
│   ├── Procedural（程序）→ 如何做
│   └── Episodic（情景）→ 行为序列（计划中）
├── 访问方式
│   ├── 当前：glob/grep
│   └── 未来：语义搜索
└── 记忆层次
    ├── Agent 级（当前）
    ├── 用户级（计划中）
    └── 组织级（计划中）

记忆更新路径
├── 热路径更新（即时）
│   └── Agent 运行时识别并更新
└── 后台更新（延迟）
    └── Cron 反思所有会话
```

## 关联实体

- [[entities/hwchase17|Harrison Chase]] - 作者
- [[entities/skill-system|Skill 系统]] - 记忆可以提炼为 Skill
- [[entities/harness-engineering|Harness Engineering]] - 记忆治理是三根支柱之一
- [[entities/soul-md|SOUL.md]] - 类似的文件化记忆模式

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]] - 补充 Context 层记忆系统的实现细节

## 实践启发

对 Atlas / WorkBuddy：
- 当前的 SOUL.md + IDENTITY.md + USER.md + 工作记忆文件已经类似这个系统
- 可改进：
  1. 添加后台反思进程
  2. 显式 /remember 命令
  3. 多级记忆（Agent 级 + 用户级）
  4. 从记忆中提炼 Skill

对庆正：
- 专用 Agent（如财报分析助手）比通用 Agent 更需要记忆
- 记忆让 Agent 越用越懂你
- 用自然语言反馈即可更新记忆，无需手动改配置
