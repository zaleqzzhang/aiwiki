# Why Agent Harness Architecture is Important

**类型**: article
**日期**: 2026-03-08
**来源**: https://contextua.dev/why-agent-harness-architecture-is-important/
**作者**: Tom Foster
**标签**: #harness-architecture #claude-code #codex-cli #gemini-cli #mistral-vibe #opencode #two-families

## 摘要

深度对比五款主流 Agent Harness（Claude Code、Codex CLI、Gemini CLI、Mistral Vibe 2.0、OpenCode）的架构设计，归纳出两大设计流派：**Reasoning-First**（信任模型、最小脚手架）vs **Environment-First**（投资环境可读性和机械强制）。核心论点：Harness 架构决定了 Agent 能力的天花板和可靠性的地板。OpenCode 的 Hashline 实验提供了最强经验证据——仅改 Harness 编辑工具，Grok Code Fast 从 6.7% 跳到 68.3%（10x 改进）。

## 关键要点

### 1. 两大设计流派

| 流派 | 代表 | 核心信念 | 特征 |
|------|------|---------|------|
| Reasoning-First | Claude Code, Mistral Vibe 2.0 | 信任模型，最小脚手架 | 主循环简洁、aggressive 上下文压缩、CLAUDE.md 持久知识 |
| Environment-First | Codex CLI, Gemini CLI | 投资环境可读性和机械强制 | Agent-legible 文档、确定性 Linter、垃圾收集 Agent、Thought Signatures |

**Reasoning-First 共同点**：
- 简单主循环（Gather→Act→Verify→Repeat）
- 关注上下文衰减（context decay）问题
- 双 Agent 架构（Initializer + Coding Agent）或中间件管线
- 子 Agent 委托（独立上下文窗口）
- 确定性 Hook 系统做安全防护

**Environment-First 共同点**：
- 仓库本身重组为 Agent 可读（progressive disclosure、AGENTS.md 做高层地图）
- Agent 生成的 Linter 机械强制架构边界
- 后台垃圾收集 Agent 持续扫描架构漂移
- Schema 校验（Zod）在边界层阻止幻觉
- Thought Signatures（Gemini）维护推理连续性

### 2. 五款 Harness 详细对比

| Harness | 流派 | 上下文管理 | 编辑方式 | 安全模型 | 多 Agent |
|---------|------|-----------|---------|---------|---------|
| **Claude Code** | Reasoning-First | CLAUDE.md 层级 + 自动压缩 + MCP Code Mode（减 98.7% 工具上下文） | str_replace | Hook（Pre/PostToolUse, SessionStart）+ 审批级别 | Agent Teams（异步对等消息） |
| **Mistral Vibe 2.0** | Reasoning-First | 中间件管线 + 主动压缩 + 持久 todo | diff-based 反馈（失败时展示为什么不匹配） | Trust-folder 校验 | 单子 Agent（可扩展） |
| **Codex CLI** | Environment-First | AGENTS.md + progressive disclosure + Thread 持久化 | apply_patch | 三权限模式（Auto/Approval/Read-Only）+ macOS Seatbelt/Linux Landlock | Ralph Loop 模式 |
| **Gemini CLI** | Environment-First | Thought Signatures + 对话检查点 | - | 400 错误强制签名 | GitHub Actions 集成 |
| **OpenCode** | 桥接 | ReAct + pre-check 压缩 + staged context | **Hashline**（行哈希引用） | opencode.json + devcontainer 隔离 | OMO 多 Agent 编排 |

### 3. Hashline：最强经验证据

OpenCode 发现"The Harness Problem"——模型意图到文件修改的机械接口才是主要瓶颈。解法是 Hashline：给每行代码打唯一内容哈希，模型引用哈希而非复制原文。

**数据**：Grok Code Fast 从 6.7%（传统 patch）→ 68.3%（Hashline）—— **10x 改进，只改 Harness 不改模型**。

### 4. 40% 上下文利用率阈值

长上下文研究发现：上下文窗口利用率超过约 40% 后，Agent 可靠性显著下降。这解释了为什么 Reasoning-First 流派强调 aggressive 压缩。

### 5. 开源 vs 闭源

四款开源（Gemini CLI、Mistral Vibe、OpenCode、Codex CLI），一款闭源（Claude Code）。

| 优势 | 开源 | 闭源 |
|------|------|------|
| 审计性 | ✅ 完全可审查 | ❌ |
| 可移植性 | ✅ 模型无关 | ❌ 绑定模型 |
| 社区创新 | ✅ Hashline 来自社区 | ❌ |
| 模型深度集成 | ❌ | ✅ Code Mode 减 98.7% 上下文、Thought Signatures |

### 6. Harness 选择清单

1. **30 分钟以上任务怎么处理？** → 上下文压缩 + 进度持久化 + 子 Agent 委托
2. **编辑失败怎么恢复？** → diff 反馈 vs Hashline vs 重试
3. **代码去哪了？** → 本地 vs 云端，安全决策
4. **能扩展到多 Agent 吗？** → Agent Teams / Ralph Loop / OMO

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 本文从实践角度验证理论
- [[entities/tom-foster|Tom Foster]] - 作者
- [[entities/openclaw|OpenClaw]] - 文中未涉及但可对比
- [[entities/hermes|Hermes]] - 文中未涉及但可对比

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]]
- [[entities/harness-engineering|Harness Engineering]]
