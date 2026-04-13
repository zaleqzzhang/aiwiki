# 视角：Reasoning-First vs Environment-First

## 核心问题

构建 Agent Harness 时，应该**信任模型的推理能力、最小化外部脚手架**（Reasoning-First），还是**投资环境的可读性和机械强制**（Environment-First）？

## 视角 A：Reasoning-First（Claude Code / Mistral Vibe 路线）

**论据**：
- **模型能力在指数增长**：今天需要确定性 Linter 强制的规则，明年模型可能自己就能遵守。过度投资 Environment 基础设施可能被模型进步"废掉"
- **简洁主循环（Gather→Act→Verify→Repeat）**降低了系统复杂度——更少的代码意味着更少的 bug
- **MCP Code Mode 减 98.7% 工具上下文**证明了深度模型集成的效率优势——通用方案做不到这一点
- **上下文压缩技术成熟**：子 Agent 委托（独立上下文窗口）+ aggressive 压缩让长任务变得可行
- **Claude Code 在 SWE-Bench 上的领先地位**部分归功于这个架构选择

**代价**：
- **对模型能力的强依赖**：如果模型犯错，Reasoning-First 的防护网更薄
- **上下文压缩有信息损失**：aggressive 压缩可能丢失关键细节
- **512K 行代码说明"最小脚手架"是相对的**——复杂性没有消失，只是转移到了 Agent 内部

## 视角 B：Environment-First（Codex CLI / Gemini CLI 路线）

**论据**：
- **确定性保障不依赖模型**：Linter 检查一次配置永远生效，不会因为模型"忘了"而失效。Schema 校验（Zod）在边界层阻止幻觉——不管模型多"自信"，错误格式就是过不了
- **仓库可读性投资一劳永逸**：把仓库重构为 Agent-legible（progressive disclosure、AGENTS.md 做高层地图），对所有 Agent 工具都有益——不绑定特定模型
- **Hashline 的 10x 改进**是 Environment-First 哲学的最强证据——瓶颈在接口不在推理
- **后台垃圾收集 Agent 持续扫描架构漂移**——不需要人类记得检查，系统自动维护一致性

**代价**：
- **前期投入大**：重构仓库结构、编写 Agent-legible 文档、配置 Linter 规则——这些都是启动成本
- **灵活性受限**：确定性规则可能过于僵硬，在快速迭代阶段反而拖慢速度
- **Thought Signatures（Gemini）维护推理连续性**是一个脆弱的机制——400 错误强制签名可能在边界条件下中断

## 深层张力

**这是"信任智能体"vs"信任基础设施"的工程哲学之争。**

Reasoning-First 的隐含假设是：模型足够聪明，给它正确的信息就够了。
Environment-First 的隐含假设是：不管模型多聪明，确定性保障永远需要。

有趣的是，**40% 上下文利用率阈值**对两个阵营都有影响：
- Reasoning-First 阵营因此需要 aggressive 压缩——这是在修补模型的限制
- Environment-First 阵营因此投资让环境更精简——这是在适应模型的限制

**两个阵营都在围绕同一个物理约束（context window）做工程优化，只是优化方向不同。** 这暗示了一个可能的收敛点：未来的最优 Harness 可能同时包含两者——Environment-First 的确定性基础设施 + Reasoning-First 的上下文策展。OpenCode 的"桥接"位置可能预示了这个趋势。

## 使用建议

- 写关于 **Harness 架构选型**的文章时，这是最核心的决策框架
- 写关于 **Agent 工具对比**的文章时，先用这个视角对工具分类，再展开细节
- 写关于 **工程最佳实践**的文章时，用两大流派的最佳实践互补构建完整方案

## 相关页面

- [[craft/design-notes/two-families-reasoning-vs-environment|设计笔记：两大流派]] - 更详细的设计考量
- [[craft/concepts/harness|概念：Harness]] - 上层概念
- [[craft/concepts/context-engineering|概念：Context Engineering]] - 两大流派的核心分歧点
- [[sources/harness-architecture-comparison|Why Agent Harness Architecture is Important]] - 两大流派的核心来源
