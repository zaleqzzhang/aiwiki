# CLAUDE.md — Glossary

**类型**: article (glossary entry)
**日期**: 2026（无精确日期）
**来源**: https://latentpatterns.com/glossary/claude-md
**作者**: Latent Patterns
**标签**: #claude-md #instruction-file #claude-code #memory-hierarchy

## 摘要

CLAUDE.md 的完整定义和六层记忆层级设计。作为 Claude Code 专有的项目级配置文件，CLAUDE.md 在每次会话前自动加载，提供代码库架构、构建命令、测试流程、编码约定等持久上下文。核心设计是**六层记忆层级**，从组织策略到自动记忆，更具体的作用域覆盖更广泛的。

## 关键要点

### 1. 六层记忆层级

| 层级 | 文件路径 | 作用域 | 特点 |
|------|---------|--------|------|
| 1. Managed Policy | `/etc/claude-code/CLAUDE.md` | 组织级（最高优先级）| IT/DevOps 管理，不可覆盖 |
| 2. Project Memory | `./CLAUDE.md`, `.claude/rules/*.md` | 项目级 | 团队共享，版本控制 |
| 3. User Memory | `~/.claude/CLAUDE.md` | 用户级 | 个人全局默认 |
| 4. Local Project Memory | `./CLAUDE.local.md` | 项目+个人 | 自动 gitignore，私人偏好 |
| 5. Child Directories | `./src/CLAUDE.md` | 子目录级 | 按需加载（仅 Agent 访问该目录时） |
| 6. Auto Memory | `~/.claude/projects/…/memory/` | 自动学习 | Claude 自动保存，每次加载前 200 行 |

### 2. Scoped Rules

`.claude/rules/*.md` 支持 YAML frontmatter 中的 `paths:` 字段，限定规则激活范围。例如 `*.test.ts` 文件专用测试约定，`src/lib/server/**` 专用数据库访问模式。

### 3. 与 AGENTS.md 的关系

- CLAUDE.md 是**厂商特定的**（只有 Claude Code 读取）
- AGENTS.md 是**跨工具开放标准**（OpenAI Codex、GitHub Copilot、Cursor 等读取）
- 桥接方案：`AGENTS.md`（版本控制的规范文件）+ `CLAUDE.md → AGENTS.md`（symlink，可 gitignore）

### 4. 写作原则

简洁 > 完整。"follow best practices" 浪费 token；"use ES modules with named exports, never default exports" 直接影响代码生成。最佳 CLAUDE.md 读起来像给资深工程师的简短上手清单。

### 5. 安全面

同 AGENTS.md——恶意仓库的 CLAUDE.md 可能包含 prompt injection。审查 CLAUDE.md 应与审查 Makefile、package.json scripts 同等严格。

## 关联实体

- [[entities/claude-md|CLAUDE.md]] - 实体页面
- [[entities/soul-md|SOUL.md]] - 相关概念（Agent 身份层 vs 项目指令层）

## 引用此来源的页面

- [[entities/claude-md|CLAUDE.md]]
