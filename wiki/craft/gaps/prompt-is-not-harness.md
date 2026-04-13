# 认知缺口：Prompt ≠ Harness

## 读者"以为懂"的认知

"写好 Prompt 就等于搭好了 Agent。Agent 表现不好？那肯定是 Prompt 没写好，改改措辞、加点 few-shot examples 就行了。"

## 实际的关键区别

### Prompt 是给模型看的一段文本，Harness 是模型跑在上面的整个系统

Prompt 是 Harness 的一个组件——就像 CPU 指令是操作系统的一部分，但操作系统远不止 CPU 指令。

一个完整的 Harness 包含（以 Akshay Pachaar 的 12 组件拆解为例）：

| 层次 | 组件 | Prompt 能覆盖吗？ |
|------|------|------------------|
| 核心 | Orchestration Loop | ❌（代码逻辑） |
| 核心 | Tools Integration | ❌（工具注册与调度） |
| 核心 | Memory Management | ❌（存储、检索、策展） |
| 核心 | Context Management | 部分（Prompt 构建是其中一环） |
| 核心 | Model Integration | ❌（API 调用、降级、重试） |
| 核心 | Human-in-the-Loop | ❌（交互协议） |
| 健壮性 | Error Handling | ❌（异常处理、重试逻辑） |
| 健壮性 | Security & Guardrails | ❌（输入验证、权限控制） |
| 健壮性 | State Management | ❌（会话状态、检查点） |
| 质量保障 | Observability | ❌（trace 收集、监控） |
| 质量保障 | Evaluation | ❌（自动评估、A/B 测试） |
| 质量保障 | Deployment & Scaling | ❌（部署、扩缩容） |

Prompt 直接相关的只有 Context Management 中的一小部分。其余 11 个组件都在 Prompt 的范围之外。

### 反直觉的数据

Claude Code 有 512,000+ 行代码。其中 Prompt 占多少？即使把所有 System Prompt、Tool Description、Few-shot Examples 加起来，也不过几千行。**99% 以上的代码都是 Harness——模型调用只是其中一小部分。**

同一个 Claude 3.5 Sonnet 模型：
- 裸 API + 精心设计的 Prompt → SWE-Bench 通过率 X
- Anthropic 的 Harness + 同样的 Prompt → SWE-Bench 通过率 6X

Princeton 研究更精确：同一 Claude Opus 4.5，仅改变 harness 配置（不改模型、不改 Prompt），solve rate 提升 64%。一个 harness 得分 2%，另一个得分 12%——6x 性能差距，100% 归因于环境设计。

**嵌套关系**：Prompt ⊂ Context ⊂ Harness。Prompt 优化指令措辞，Context 策展模型需要的信息（六大组件），Harness 还管防止、度量、控制和修复。MIT 统计：约 95% 大企业 AI 试点失败——根源不是 Prompt 质量，而是 Harness 层的架构缺口。

### 为什么这个误解有害

如果你相信"Prompt = Agent"，你会把所有时间花在调措辞上——多加一句"请仔细思考"、换一种角色设定、增加几个示例。这些确实有用，但收益递减极快。

真正决定 Agent 系统上限的是 Harness 层的工程：

- 模型出错时如何恢复？（Error Handling）
- 工具调用失败时如何重试？（Retry Logic）
- 长任务如何保持上下文连贯？（State Management）
- 如何知道 Agent 做得好不好？（Evaluation）
- 过去的经验如何传递到下一次？（Memory + Skills）

这些问题的答案不在 Prompt 里，在代码里。

## 正确的心智模型

**Prompt 是给 Agent 下的一道指令，Harness 是 Agent 运行的整个环境。**

Vivek Trivedy 的定义最直接："If you're not the model, you're the harness." 模型之外的一切——就是 Harness。

类比：Prompt 像给员工的一封邮件（"请完成这个任务"），Harness 像给员工的整个工作环境（电脑、工具、流程规范、培训手册、绩效考核体系、同事支持）。邮件写得再好，如果工作环境一塌糊涂，员工也做不好事情。

## 写文章时可以这样切入

> "很多团队在 Prompt Engineering 上花了几个月，Agent 的表现从 50 分提到了 60 分，然后就怎么调都不动了。这不是 Prompt 的问题——是撞到了 Prompt 能力的天花板。真正让 Agent 从 60 分到 90 分的，是 Harness Engineering：错误恢复、工具编排、记忆管理、评估闭环——所有那些 Prompt 覆盖不到的'系统层'。"

## 相关页面

- [[craft/concepts/harness|概念：Harness]] - Harness 的完整概念讲解
- [[sources/anatomy-of-agent-harness|The Anatomy of an Agent Harness]] - 12 组件拆解
- [[craft/concepts/meta-harness|概念：Meta-Harness]] - Harness 层的持续改进方法
