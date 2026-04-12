# 不懂 Harness Engineering，你所谓的 AI 转型只是在浪费预算

**类型**: article
**日期**: 2026-04-12
**来源**: https://x.com/lewiskuo2/article/2043195969958007288
**作者**: [[entities/lewis-kuo|Lewis爆肝战神]]
**标签**: #harness-engineering #business-case #openai #anthropic

## 摘要

同样用 Claude 或 GPT，有人让 AI 写了几行代码就卡住了，有人却让 AI 连续工作 6 个小时，交付了一个完整的项目。OpenAI 3 名工程师，五个月，一行代码都没手写，指挥 Codex Agent 写了 100 万行代码，做出了一个真实的产品。差距在哪？答案是 **Harness Engineering**。

## 关键要点

### Agent = Model + Harness

- **Model 提供智能，Harness 让这个智能能用起来**
- Harness 包括：系统提示词、工具、文件系统、沙箱、编排逻辑、各种检查机制
- 核心三问：AI 在哪干活？用什么干活？怎么知道干得对不对？

### OpenAI 的方案：整理好的工作台

- **文件系统重构**：把 AGENTS.md 精简到 100 行，只做"目录"，真正内容放在 docs/ 目录
- **工具链**：bash、代码执行、沙箱环境，AI 可以自己测试、看日志、改代码
- **可观测性**：日志、指标、UI 界面都暴露给 AI，AI 能看到代码跑起来是什么样
- **Linter 重写**：错误信息让 AI 能看懂"哪里错了，怎么改"——受众从人变成 AI

### Anthropic 的方案：生成器 + 评估器

- **核心问题**：AI 评估自己的工作完全不靠谱
- **解决方案**：生成和评估拆成两个 AI
  - 生成器负责做前端页面
  - 评估器拿到工具，实际操作页面：点按钮、填表单、截图、检查功能
  - 对照四个标准打分：设计质量、原创性、工艺、功能性
- **迭代机制**：来回 5 到 15 轮
- **效果对比**：单 AI 跑 20 分钟花 $9，完整 Harness 跑 6 小时花 $200，交付真正能玩的游戏

### VergeX 的 AI Trading Harness：四核心模块

1. **工具调用（Tool Use）**：Harness 负责抓市场数据，模型只负责"想"，不直接碰市场
2. **上下文管理（Context Management）**：持续维护备忘录，记录持仓状态、账户余额、决策历史
3. **架构约束（Architectural Constraints）**：规则（最大杠杆、最大仓位）在代码层面强制执行
4. **反馈循环（Feedback Loop）**：每次交易结果写回系统，形成持续优化闭环

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 核心方法论
- [[entities/meta-harness|Meta-Harness]] - Harness 层优化
- OpenAI - Codex Agent 案例
- Anthropic - 生成器 + 评估器架构
- VergeX - AI Trading Harness 案例

## 核心洞察

> "这三个问题解决了，AI 就能持续工作。解决不了，AI 就只能陪你聊天。"

> "AI 不需要一开始就知道所有事情，它需要在正确的时机拿到正确的信息。"

> "Linter 的受众从人变成了 AI。"

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]]
