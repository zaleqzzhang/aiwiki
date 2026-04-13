# AGENTS.md — Glossary

**类型**: article (glossary entry)
**日期**: 2026（无精确日期）
**来源**: https://latentpatterns.com/glossary/agents-md
**作者**: Latent Patterns
**标签**: #agents-md #instruction-file #cross-tool #open-standard

## 摘要

AGENTS.md 的完整定义和设计细节。作为跨工具的开放标准指令文件（Linux Foundation AAIF 治理），AGENTS.md 为 AI 编码 Agent 提供项目特定上下文——构建命令、测试流程、编码约定、架构说明。核心设计是**发现层级**（全局→项目→子目录逐层拼接），近端覆盖远端。与 CLAUDE.md 的关系通过 symlink 桥接。

## 关键要点

1. **跨工具兼容性**：被 OpenAI Codex、GitHub Copilot、Cursor、Windsurf、Amp 等原生读取；Claude Code 不原生读取，需 symlink 桥接
2. **发现层级**：`~/.codex/AGENTS.md`（全局）→ `repo-root/AGENTS.md`（项目）→ `packages/api/AGENTS.md`（子目录），逐层拼接，近端覆盖
3. **标准化历程**：从厂商碎片化（.cursorrules、.windsurfrules 等）→ Geoffrey Huntley 提出 AGENT.md（单数）→ OpenAI 标准化为 AGENTS.md（复数）→ 2025-12 转入 Linux Foundation AAIF
4. **上下文窗口权衡**：每字节都消耗 prompt token，简洁具体优于冗长文档
5. **安全面**：指令文件被 Agent 当作可信指导，恶意仓库的 AGENTS.md 可作为 prompt injection 载体

## 关联实体

- [[entities/agents-md|AGENTS.md]] - 实体页面

## 引用此来源的页面

- [[entities/agents-md|AGENTS.md]]
