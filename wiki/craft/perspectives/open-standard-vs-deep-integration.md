# 视角：开放标准 vs 深度集成

## 核心问题

Agent 生态应该走**跨工具开放标准**（AGENTS.md / OpenClaw 24+ 平台 / ClawHub 13,000+ Skill）还是**单一厂商深度集成**（CLAUDE.md / Claude Code Code Mode / Thought Signatures）？

## 视角 A：开放标准优先

**论据**：
- **互操作性是真实需求**：团队不可能只用一个 Agent 工具。开发者用 Claude Code 写代码、Cursor 做快速编辑、Copilot 做代码补全——AGENTS.md 让一份规则服务所有工具
- **社区创新不可替代**：Hashline（10x 性能提升）来自开源社区 OpenCode，不是任何大厂内部研发。ClawHub 的 13,000+ Skill 覆盖了单一厂商不可能覆盖的长尾场景
- **避免锁定**：模型换代极快。今天最强的 Claude，明年可能被 Gemini 超越。开放标准让你的 Harness 投资跨模型迁移
- **Linux Foundation AAIF 治理**提供了标准化保障

**代价**：
- **最大公约数困境**：AGENTS.md 只能支持所有工具共有的特性，无法利用任何工具的独特能力
- **安全漏洞面扩大**：OpenClaw 的 12-20% 恶意 Skill 率、AGENTS.md 的 prompt injection 风险——开放意味着信任边界更薄
- **缺乏深度优化**：通用标准无法做到 Claude Code MCP Code Mode 那样的 98.7% 工具上下文压缩

## 视角 B：深度集成优先

**论据**：
- **性能优势不可忽视**：MCP Code Mode 减 98.7% 工具上下文、Thought Signatures 维护推理连续性——这些都是跨工具标准做不到的
- **安全可控**：Hermes 零 Agent 特定 CVE，核心原因之一是 Skill 自生成绕过了供应链攻击向量
- **Claude Code 的 512K 行代码**证明了深度集成的工程投入产出比——它在 SWE-Bench 上的领先地位部分归功于与 Claude 模型的深度协同
- **用户体验更连贯**：一个工具做好，好过五个工具各做一半

**代价**：
- **厂商锁定风险**：如果 Anthropic 改变定价策略或 API，你的整个 Harness 投资可能打水漂
- **创新瓶颈**：内部团队的创新速度 < 整个开源社区
- **适用场景有限**：不是所有团队都能接受只用一个模型/工具

## 深层张力

**这不是技术问题，是信任模型问题。**

开放标准假设"Agent 工具是可替换的"——所以需要一个通用协议层。深度集成假设"Agent 工具是核心基础设施"——所以值得深度投入。

更根本的张力是：**Agent 系统的护城河在哪里？**
- 如果护城河在数据和轨迹（Phil Schmid 的观点）→ 开放标准可行，因为 trace 跟着你走
- 如果护城河在 Harness 的深度优化（NxCode 的观点）→ 深度集成更有利，因为通用 Harness 做不到极致优化

当前趋势是**分层共存**：底层用开放标准（AGENTS.md 做项目配置），上层用深度集成（CLAUDE.md 做工具特定优化）。两者通过 symlink 桥接。

## 使用建议

- 写关于**工具选型**的文章时，用这个视角对比"选一个做深 vs 多个都用"的权衡
- 写关于**Agent 安全**的文章时，用这个视角展现"开放生态的安全代价"
- 写关于**标准化趋势**的文章时，用这个视角讨论"为什么 AGENTS.md 很重要但不够"

## 相关页面

- [[entities/agents-md|AGENTS.md]] - 开放标准的代表
- [[entities/claude-md|CLAUDE.md]] - 深度集成的代表
- [[craft/concepts/agents-md-standard|概念：AGENTS.md 标准]] - 开放标准的概念卡片
- [[craft/concepts/agent-security|概念：Agent 安全]] - 安全层面的张力
- [[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]] - 开源 vs 闭源对比
