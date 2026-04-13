# Hermes Agent Tutorial: Install & Set Up Your First Self-Improving AI (2026)

**类型**: article
**作者**: NxCode Team
**日期**: 2026-04-12
**来源**: https://www.nxcode.io/resources/news/hermes-agent-tutorial-install-setup-first-agent-2026
**标签**: #hermes-agent #tutorial #skill-system #memory #MCP #cron

## 摘要

NxCode 团队出品的 Hermes Agent v0.8.0 实操教程，从零搭建一个可运行的自我改进 AI Agent。覆盖安装、模型配置（OpenRouter/Anthropic/Ollama）、Telegram/Discord 消息网关、Skill 自动创建机制、Cron 调度器、MCP 服务器集成等全流程。文章定位为"30 分钟上手指南"，面向有基础开发经验的用户。

## 关键要点

- **一键安装**：`curl` 一行脚本完成安装，自动检测 OS、安装依赖、创建虚拟环境、注册全局命令 `hermes`
- **模型要求**：最低 64K context tokens，低于此阈值启动时直接拒绝（这是 Harness 层对模型能力的硬性约束）
- **三种模型接入**：OpenRouter（200+ 模型统一 API）、Anthropic 直连、Ollama 本地离线；支持 fallback provider chain
- **40+ 内置工具**：文件操作、终端命令、Web 搜索、代码分析
- **消息网关（Gateway）**：支持 Telegram / Discord / Slack / WhatsApp / Signal，可安装为系统服务（launchd / systemd）
- **Skill 自动创建**：完成复杂任务（5+ tool calls）后自动生成结构化 Markdown Skill，遵循 agentskills.io 标准，可跨 Agent 移植
- **Skill 自我改进**：执行 Skill 时发现更优路径会自动更新 Skill 文档
- **持久化记忆**：MEMORY.md + SQLite FTS5 全文搜索，跨会话记忆检索
- **Cron 调度器**：支持自然语言和 cron 表达式定义定时任务，可指定投递渠道（Telegram/Discord）
- **MCP 集成**：通过 config.yaml 配置 MCP 服务器，支持工具白名单/黑名单控制
- **六种终端后端**：local / docker / ssh / daytona / modal / singularity（安全沙箱执行）
- **安全模块 Tirith**：硬性阻止危险命令（如 `curl | sh`），防御提示注入攻击
- **从 OpenClaw 迁移**：安装向导自动检测 `~/.openclaw`，提供配置/技能/记忆迁移
- **Token 开销**：CLI 每轮约 6-8K input tokens，Telegram 每条约 15-20K

## 新信息（与现有知识库补充）

- Hermes v0.8.0 的具体系统要求：4GB RAM（推荐 8GB+）、2GB 磁盘、Apple Silicon 上 7B 模型 50-80 tok/s
- `/compress` 命令用于长会话中总结历史、降低 token 消耗（对话压缩的实际操作方式）
- `hermes doctor` 诊断工具：环境检查、依赖、API 连通性、配置
- 配置文件结构：`~/.hermes/` 下包含 config.yaml、.env、SOUL.md、MEMORY.md、skills/、sessions/、cron/、logs/

## 关联实体

- [[hermes]] - 本文是其官方推荐的入门教程，提供 v0.8.0 实操细节
- [[openclaw]] - 提到迁移路径（setup wizard 自动检测 ~/.openclaw）
- [[skill-system]] - 描述了 Skill 自动创建的触发条件（5+ tool calls）和自我改进机制
- [[nous-research]] - Hermes Agent 的开发组织
- [[MCP]] - 详细展示了 MCP 配置格式和工具过滤选项

## 引用此来源的页面

- [[entities/hermes]]
- [[entities/skill-system]]
- [[topics/agent-architecture]]
