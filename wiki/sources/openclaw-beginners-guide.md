# How Does OpenClaw Work? A Beginner's Guide

**类型**: article
**日期**: 2026-03-19
**来源**: https://dev.to/ljhao/how-does-openclaw-work-a-beginners-guide-21cj
**作者**: PrivOcto
**标签**: #openclaw #agent-architecture #skills #memory #local-first

## 摘要

OpenClaw 入门级架构指南。文章从实际使用视角拆解了 OpenClaw 的四层核心组件：Gateway（WebSocket 通信层）、Sessions/Memory（会话与记忆管理）、Provider/Model（模型集成）、Plugins/Tools（能力扩展）。强调 OpenClaw 的本地优先（local-first）哲学——所有数据、API keys、运行轨迹留在本地，不依赖云端。同时介绍了 ClawHub Skills 生态系统和多平台消息集成。

## 关键要点

- **四层架构**：Gateway（WebSocket 端口 18789）→ Sessions/Memory（会话管理 + 本地 Markdown 记忆）→ Provider/Model（多模型支持）→ Plugins/Tools（TypeScript 运行时加载）
- **本地优先**：数据、API keys、Agent 行为全部留在本地，无云端依赖。MIT 协议开源
- **记忆系统**：MEMORY.md（长期事实）+ memory/YYYY-MM-DD.md（每日笔记）+ vector embedding 检索。与 Hermes 的 Prompt Memory 类似但更简单——纯文件，无分层
- **Skills 生态**：ClawHub 10,000+ skills（文中另处提到 2,857）。Skill = 目录（SKILL.md + 可选脚本）。三级加载优先级：Workspace > ~/.openclaw/skills > Bundled
- **多平台消息**：WhatsApp（Baileys）、Telegram（grammY）、Discord（discord.js）、Slack、Teams、iMessage、Signal——统一通过 Gateway WebSocket 接入
- **会话作用域**：dmScope 三种模式——main（全局单会话）、per-channel-peer（按通道+发送者）、per-account-channel-peer（加账户隔离）
- **模型无关**：支持 Anthropic / OpenAI / Google Gemini / 自定义 OpenAI 兼容端点，多 key 自动轮转
- **增长数据**：72 小时 60,000 stars → 3 个月 247,000 stars。Peter Steinberger（PSPDFKit 创始人）创建

## 与现有知识的关系

- **对比 Hermes 记忆**：OpenClaw 用 MEMORY.md + 日期文件的"文件即记忆"模式，与 [[sources/agent-builder-memory-system|Agent Builder 的记忆系统]] 极其相似。但无 Hermes 的四层分级（Prompt Memory → Session Search → Skills → Honcho），也无 3,575 字符强制策展
- **Skill 系统差异**：OpenClaw Skills 更接近"打包插件"（目录 + SKILL.md），而 Hermes Skills 是从 trace 中提炼的经验。前者偏"安装即用"，后者偏"积累即学"
- **架构互补**：之前的 [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] 聚焦多 Agent 协作层面。这篇补充了 OpenClaw 的基础设施细节——Gateway、Plugin 系统、记忆方案

## 关联实体

- [[entities/openclaw|OpenClaw]] - 核心主体，四层架构详细拆解
- [[entities/skill-system|Skill 系统]] - ClawHub 生态、三级加载机制
- [[entities/soul-md|SOUL.md]] - OpenClaw 记忆系统采用类似的 MEMORY.md 模式

## 引用此来源的页面

- [[entities/openclaw|OpenClaw]]
