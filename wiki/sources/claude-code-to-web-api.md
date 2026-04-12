# 从 Claude Code 到 Web API：解构 AI Agent 的 Skills 体系

**类型**: article
**日期**: 2026-01-18
**来源**: https://x.com/servasyy_ai/status/2012770711551570074
**作者**: [[entities/servasyy-ai|servasyy_ai]]
**标签**: #skills #claude-code #sdk #architecture

## 摘要

在 Claude Code 中构建的 skills 只能在本地环境运行，如何让它们脱离 Claude Code 实现 Web/API 访问、自动化运行、开放使用？答案在于理解 Claude Agent SDK 的三层架构（用户层、编排层、执行层）和三个角色（Skills、Agents、Subagents）的分工，然后重建编排层。

## 关键要点

### 三层架构

```
用户层 → 接收需求，返回结果
编排层 → 主 Agent + Skills，做决策、协调执行
执行层 → Subagents + Tools，干活
```

### 三个角色的本质

**Skills = 菜谱（工作流程定义者）**
- Skills 不干活，只告诉 Agent "该做什么、按什么顺序做"
- 是 Markdown 文档，不是代码
- 定义整个工作的剧本

**Agent = 项目经理（决策和调度者）**
- 拿着 Skills 这份剧本，负责执行和调度
- 做决策，但不亲自干重活
- 决定什么时候派发 Subagent 去干具体的活

**Subagents = 临时工（具体任务执行者）**
- 专门负责某一个具体任务
- 每个只关注自己的任务，完成后消失
- 上下文隔离：写作 Subagent 不需要知道配图的细节

### Claude Code 在背后做了什么

1. **自动加载和触发**：扫描 .claude/skills/ 目录，自动识别并触发
2. **提供工具箱**：Read、Edit、Execute、Grep 等内置工具
3. **管理上下文**：所有 skills 共享同一个工作台

### 解耦的三种方案

**方案一：直接使用 Claude Agent SDK**
- 把 skills 改造成 SDK 能识别的格式
- 用 SDK 提供的 Agent 作为编排层
- 工具封装成 Tools 或 MCP Server
- 适合：skills 成熟、追求稳定

**方案二：自建轻量级编排层**
- 用 Anthropic API 创建主 Agent
- 通过 prompt 让它理解 skills 体系
- 主 Agent 根据需求调用工具
- 适合：快速验证、灵活迭代

**方案三：基于 MCP 协议**
- 每个 skill 封装成独立的 MCP Server
- 主 Agent 通过 MCP 协议调用
- 可以和其他支持 MCP 的系统互通
- 适合：构建开放生态、追求标准化

### 解耦需要解决的四个问题

1. **Skill 加载和触发** - 读取 skill 文件，判断什么时候用哪个 skill
2. **工具函数** - 实现图片生成、公众号发布等具体功能
3. **上下文管理** - 让不同 skills 之间能传递数据和结果
4. **Agent 管理** - 派发和协调多个 AI 实例

## 关联实体

- [[entities/skill-system|Skill 系统]] - 核心概念
- [[entities/servasyy-ai|servasyy_ai]] - 作者
- Claude Code - 案例来源
- MCP 协议 - 解耦方案

## 核心洞察

> "Skills 是知识，Agent 是大脑，Tools 是手脚，编排层是关键。"

> "关键在于编排层。执行层相对简单，用户层也好办。难的是编排层：如何让主 Agent 理解需求、选择 skills、协调执行？"

> "理解了这些，解耦的路径就清晰了：重建编排层，让主 Agent 能够在 Web/API 环境中调度你的 skills。"

## 引用此来源的页面

- [[entities/skill-system|Skill 系统]]
- [[topics/agent-architecture|Agent 架构]]
