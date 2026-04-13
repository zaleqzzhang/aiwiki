# 概念：四动词框架（Constrain / Inform / Verify / Correct）

## 一句话

Harness Engineering 的四种基本动作——约束、告知、验证、纠正——覆盖了"让 Agent 可靠工作"的完整闭环。

## 为什么需要这个概念

"Harness Engineering"听起来很大——记忆治理、架构约束、评估闭环、Context Engineering、安全沙箱……概念太多，不知道从哪里开始。四动词框架把所有这些实践归入四个动作，给了一个清晰的心智模型：

> 好的 Harness 要做到四件事：**约束**（防止 Agent 做不该做的事）→ **告知**（确保 Agent 有正确信息）→ **验证**（检查 Agent 做对了没）→ **纠正**（修复做错的地方）。

不管你在做什么具体工作——写 AGENTS.md、设置 CI Hook、配置安全沙箱——都可以归入这四个动作之一。

## 与相邻概念的边界

- **vs 三大支柱**：三大支柱（Context Engineering / Architectural Constraints / Entropy Management）是 OpenAI 框架下的实践分类。四动词是更底层的动作分类——三大支柱的每一根都同时涉及四个动词。比如 Architectural Constraints 主要是 Constrain，但也包含 Verify（Linter 检查）和 Correct（自修复循环）。
- **vs Prompt / Context / Harness 三层**：三层是架构分层，四动词是操作分类。三层回答"在哪里做"，四动词回答"做什么"。
- **vs Martin Fowler 的定义**：Fowler 说 Harness Engineering 是 "the tooling and practices we can use to keep AI agents in check"——侧重约束。四动词框架纠正了这个偏见：约束只是四分之一，Inform 和 Correct 同样重要。好的 Harness 不只是安全限制，还能让 Agent **更能干**。

## 设计直觉

四动词不是并列的，它们构成一个循环：

```
Constrain（划边界）
    ↓
Inform（给信息）
    ↓
Agent 执行任务
    ↓
Verify（查结果）
    ↓
Correct（修问题）→ 回到 Constrain（更新边界）
```

**关键洞察：约束解空间反而让 Agent 更高效。** 直觉告诉我们约束是限制，但实践数据表明：无约束时 Agent 浪费 token 探索死胡同，有约束时更快收敛到正确方案。OpenAI 的依赖分层（Types→Config→Repo→Service→Runtime→UI）就是 Constrain 的典范——不是建议，而是 CI 强制执行。

每个动词的典型手段：

| 动词 | 手段 | 示例 |
|------|------|------|
| Constrain | 架构边界、依赖规则、安全沙箱 | OpenAI 六层依赖分层、OpenClaw 三层沙箱 |
| Inform | Context Engineering、文档 | AGENTS.md、CLAUDE.md、动态日志注入 |
| Verify | 测试、Linting、LLM 审计 | 确定性 Linter、Agent Code Review |
| Correct | 反馈循环、自修复 | diff-based 反馈、Hashline、重试策略 |

## 理解的关键转折

**大多数初学者只关注 Inform（写更好的 Prompt/文档），但真正的杠杆在 Constrain 和 Verify。**

写一个好的 AGENTS.md（Inform）是有价值的，但如果没有 CI 强制执行架构约定（Constrain）+ 自动化测试验证结果（Verify），Agent 照样可能产出不符合标准的代码——只是"不符合的方式更优雅了"。

NxCode 列举的常见错误 #4 "无反馈循环"正是指这个问题：只有 Constrain + Inform，没有 Verify + Correct 的 Harness 是"笼子"不是"导轨"——它防止 Agent 犯大错，但不帮 Agent 变好。

## 展开时可用的例子

- **OpenAI Codex 的完整闭环**：Constrain（依赖分层 CI 强制）→ Inform（代码库地图 + 架构文档）→ Verify（Linter 重写为 Agent 可读 + 测试套件）→ Correct（失败反馈回注上下文，Agent 自修复）。3 名工程师用这套 Harness 让 Agent 写了 100 万行代码。
- **Stripe Minions**：Constrain（只允许修改指定范围）→ Inform（Slack 任务描述自动转为上下文）→ Verify（CI 管线自动跑）→ Correct（失败自动回传给 Agent 重试）。每周 1000+ merged PRs。
- **LangChain 中间件**：LocalContext（Inform）→ LoopDetection（Verify）→ ReasoningSandwich（Constrain）→ PreCompletionChecklist（Verify + Correct）。从 Top 30 跳到 Top 5，仅改 Harness。

## 相关页面

- [[entities/harness-engineering|Harness Engineering]] - 四动词框架的来源
- [[craft/concepts/harness|概念：Harness]] - 上层概念
- [[craft/concepts/context-engineering|概念：Context Engineering]] - Inform 动词的深度展开
- [[sources/harness-engineering-complete-guide|Harness Engineering: Complete Guide]] - 核心来源
