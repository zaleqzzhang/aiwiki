# 设计笔记：Reasoning-First vs Environment-First 两大流派

## 设计选择

在设计 Agent Harness 架构时，有两条截然不同的路线：
- **Reasoning-First**：信任模型的推理能力，最小化外部脚手架，让模型自主决策
- **Environment-First**：投资环境的可读性和机械强制，用确定性手段约束不确定性

## 直觉反应

第一反应是"这取决于模型有多强"——模型足够强就 Reasoning-First，模型不够强就 Environment-First。

但实际情况复杂得多。**Claude Code 采用 Reasoning-First（信任模型），但它背后是 512,000+ 行 Harness 代码**——它并非真的"最小脚手架"，而是把复杂性藏在了 Agent 内部（上下文压缩、MCP Code Mode 减 98.7% 工具上下文）。Codex CLI 采用 Environment-First，但它的核心理念是让仓库本身变得 "Agent-legible"——不是给 Agent 加更多代码，而是重构环境让 Agent 更容易理解。

**两条路线的投资方向不同，但工程量可能差不多。**

## 设计考量

### 角度一：上下文管理策略

| 维度 | Reasoning-First | Environment-First |
|------|----------------|-------------------|
| 核心工具 | CLAUDE.md 层级 + 自动压缩 | AGENTS.md + progressive disclosure |
| 压缩策略 | Aggressive（主动压缩，信任模型能重建上下文） | Conservative（持久化更多，减少压缩损失） |
| 知识载体 | 模型的持久文件（CLAUDE.md, mcp.json） | 仓库结构本身（Agent-legible 文档） |
| 衰减应对 | MCP Code Mode（减 98.7% 工具上下文） | Thought Signatures（维护推理连续性） |

### 角度二：错误处理哲学

Reasoning-First 的错误处理是**事后修复**——让模型先尝试，失败了再通过反馈循环纠正。Mistral Vibe 的 diff-based 反馈就是这个逻辑：编辑失败时，展示"为什么不匹配"让模型理解问题。

Environment-First 的错误处理是**事前预防**——在模型行动之前，用确定性 Linter、Schema 校验（Zod）、Agent 生成的架构约束来阻止错误发生。Codex CLI 的后台垃圾收集 Agent 持续扫描架构漂移就是这个逻辑。

### 角度三：多 Agent 扩展

- Reasoning-First → Agent Teams（Claude Code 的异步对等消息）、单子 Agent（Mistral Vibe）
- Environment-First → Ralph Loop 模式（Codex CLI）、GitHub Actions 集成（Gemini CLI）、OMO 多 Agent 编排（OpenCode）

### 角度四：Hashline 的启示

OpenCode 站在两大流派之间（"桥接"位置），但 Hashline 这个创新来自 Environment-First 的思路——**改善模型和环境之间的接口**，而非改善模型的推理能力。Grok Code Fast 从 6.7% → 68.3%（10x）证明了：**有时候瓶颈不在模型脑子里，而在模型和环境之间的管道上。**

## 对比视角

两大流派不是二选一，而是一个光谱。实际产品往往在光谱中间取点：

- Claude Code **主体 Reasoning-First**，但 Hook 系统做确定性安全防护（Environment-First 元素）
- Codex CLI **主体 Environment-First**，但 AGENTS.md 依赖模型理解自然语言文档（Reasoning-First 元素）
- OpenCode 明确站在**桥接位置**——ReAct 循环（Reasoning） + pre-check 压缩 + Hashline（Environment）

**深层趋势**：随着模型能力持续增强，光谱是否会向 Reasoning-First 一端移动？不一定。因为 40% 上下文利用率阈值是一个物理约束——不管模型多强，context rot 都会发生。Environment-First 的环境可读性投资不会因为模型变强而过时。

## 启发

如果你在选择架构方向：
1. **团队规模小、模型预算充足** → 偏 Reasoning-First（Claude Code 路线），把工程精力放在 CLAUDE.md 和上下文策展
2. **团队规模大、需要机械一致性** → 偏 Environment-First（Codex CLI 路线），投资仓库可读性和 CI 约束
3. **不确定** → 从 Environment-First 的基础设施开始（AGENTS.md + Linter），再叠加 Reasoning-First 的工具（上下文压缩 + 子 Agent 委托）

## 相关页面

- [[entities/harness-engineering|Harness Engineering]] - 两大流派是 Harness 架构的核心分野
- [[craft/concepts/harness|概念：Harness]] - 上层概念
- [[craft/concepts/context-engineering|概念：Context Engineering]] - 两大流派在上下文管理上的差异
- [[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]] - 两大流派的核心来源
