# AGENTS.md

**类型**: concept
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 定义

跨 AI 编码工具的开放标准指令文件（Linux Foundation AAIF 治理）。放在仓库中，为 Agent 提供项目特定上下文——构建命令、测试流程、编码约定、架构说明。核心价值：**一次编写，多工具读取**。

## 核心特征

- **发现层级**：全局 `~/.codex/AGENTS.md` → 项目根 `AGENTS.md` → 子目录 `packages/api/AGENTS.md`，逐层拼接，近端覆盖远端
- **跨工具兼容**：OpenAI Codex、GitHub Copilot、Cursor、Windsurf、Amp 原生支持
- **无必填字段**：纯 Markdown，无特殊语法要求
- **Monorepo 友好**：根文件描述共享约定，子目录文件补充包特定指令

## 与其他实体的关系

- [[entities/claude-md|CLAUDE.md]] → 厂商特定替代品，通过 symlink 桥接
- [[entities/soul-md|SOUL.md]] → 不同层面的概念（SOUL.md 定义 Agent 身份，AGENTS.md 定义项目指令）
- [[entities/harness-engineering|Harness Engineering]] → AGENTS.md 是 Context Engineering 的关键组件

## 相关来源

- [[sources/agents-md-glossary|AGENTS.md — Glossary]] - 完整定义和设计细节

## 开放问题

- AGENTS.md 和 CLAUDE.md 长期是否会统一？
- 当多层 AGENTS.md 拼接后超过上下文窗口限制怎么办？
