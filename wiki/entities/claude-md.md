# CLAUDE.md

**类型**: concept
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 定义

Claude Code 专有的项目级配置文件。每次会话前自动加载，提供代码库架构、构建命令、测试流程、编码约定等持久上下文。核心设计：**六层记忆层级**。

## 六层记忆层级

| 层级 | 文件路径 | 作用域 | 谁管理 |
|------|---------|--------|--------|
| Managed Policy | `/etc/claude-code/CLAUDE.md` | 组织级 | IT/DevOps |
| Project Memory | `./CLAUDE.md`, `.claude/rules/*.md` | 项目级 | 团队 |
| User Memory | `~/.claude/CLAUDE.md` | 用户级 | 个人 |
| Local Project Memory | `./CLAUDE.local.md` | 项目+个人 | 个人（gitignored） |
| Child Directories | `./src/CLAUDE.md` | 子目录级 | 团队（按需加载） |
| Auto Memory | `~/.claude/projects/…/memory/` | 自动学习 | Claude 自动（前 200 行/会话） |

## 核心特征

- **Scoped Rules**：`.claude/rules/*.md` 支持 `paths:` frontmatter，按文件模式匹配激活规则
- **按需加载**：子目录 CLAUDE.md 仅在 Agent 访问该目录时加载，保持上下文精简
- **自动记忆**：Claude 自动保存发现的模式和学习，限制 200 行/会话
- **写作原则**：简洁 > 完整，像给资深工程师的上手清单

## 与其他实体的关系

- [[entities/agents-md|AGENTS.md]] → 跨工具开放标准替代品，symlink 桥接
- [[entities/soul-md|SOUL.md]] → 不同层面（SOUL.md = Agent 身份层，CLAUDE.md = 项目指令层）
- [[entities/harness-engineering|Harness Engineering]] → 是 Reasoning-First 流派的 Context 实现

## 相关来源

- [[sources/claude-md-glossary|CLAUDE.md — Glossary]] - 完整定义和六层记忆层级

## 开放问题

- Auto Memory 的 200 行限制是否足够？如何控制自动学习的质量？
- Managed Policy 在企业环境中如何与 AGENTS.md 的组织级指令协调？
