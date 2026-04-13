# Prompt Engineering vs Context Engineering vs Harness Engineering: What's the Difference in 2026?

**类型**: article
**日期**: 2026-03-26
**来源**: https://dev.to/ljhao/prompt-engineering-vs-context-engineering-vs-harness-engineering-whats-the-difference-in-2026-37pb
**作者**: PrivOcto
**标签**: #prompt-engineering #context-engineering #harness-engineering #comparison

## 摘要

一篇面向开发者的三层工程方法论对比文章，核心论点：Prompt Engineering、Context Engineering、Harness Engineering 构成 **嵌套层级关系**（Prompt ⊂ Context ⊂ Harness），而非平行选择。文章引用 Princeton 研究（harness 配置提升 solve rate 64%）和 MIT 统计（95% 企业 AI 试点失败）来论证"生产失败的根源是架构缺口而非 Prompt 质量"。包含详细对比表格和 Birgitta Boeckeler 三支柱框架。

## 关键要点

### 三层定义与核心问题

| 层 | 核心问题 | 范围 |
|----|---------|------|
| Prompt Engineering | "How should I phrase this?" | 单次交互 |
| Context Engineering | "What does the model need to know?" | 多轮信息流 |
| Harness Engineering | "How do agents operate reliably over thousands of inferences?" | 跨天/周的多步骤系统 |

### 层级关系（非平行）

- **Prompt ⊂ Context**：Prompt 处理指令措辞，是 Context Engineering 中策展信息生态的一个子集
- **Context ⊂ Harness**：Context 决定模型看到什么信息，Harness 还管"系统防止什么、度量什么、修复什么"
- 三者不是选择题，而是**叠加策略**：先 Prompt 快速见效 → 加 Context 处理复杂工作流 → 上 Harness 保障生产

### 关键数据

- **Princeton 研究**：仅改变 harness 配置（不改模型），solve rate 提升 64%。同一 Claude Opus 4.5，在不同 harness 下：2% vs 12%，纯粹归因于环境设计
- **MIT 统计**：约 95% 大企业 AI 试点未能产出可度量回报
- **Agent 失败率**：约 20%
- **Prompt 脆弱性**：重排示例可导致准确率偏移超过 40%

### Context Engineering 六大组件

1. System Instructions（行为准则与边界）
2. Memory Management（短期会话 + 长期持久知识）
3. Retrieved Information（RAG，从数据库/API 拉取）
4. Tool Orchestration（工具访问与输出回流）
5. Output Structuring（响应格式强制）
6. Query Augmentation（将粗糙输入转为可处理请求）

### Context Rot

LLM 的上下文衰减现象：随 token 数量增加，模型准确召回信息的能力下降。因此 Context Engineering 要策展"最小可行的高信号 token 集"，而非无脑填充。

### Birgitta Boeckeler 三支柱（Harness Engineering）

1. **Context Engineering**：持续增强的知识库 + 动态可观测数据
2. **Architectural Constraints**：确定性 Linter 和结构化测试强制边界
3. **Garbage Collection**：周期性 Agent 扫描文档漂移和约束违规

注：这与 wiki 中已有的"三根支柱"（记忆治理 + 架构约束 + 评估闭环）有重叠但侧重不同——Boeckeler 版本强调 Garbage Collection 而非评估闭环。

### 实践案例

- **OpenAI**：不写手动代码，用 Harness 管理 100 万行以上产品代码。Agent 失败视为改进 Harness 的信号
- **Stripe**：每周 1,300 个 AI 编写的 PR，通过 Harness 强制的任务划分、沙箱运行时和审核门控

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 三层层级关系的完整论述
- [[entities/openclaw|OpenClaw]] - 文末 related articles 链接

## 引用此来源的页面

- [[entities/harness-engineering|Harness Engineering]]
- [[topics/agent-architecture|Agent 架构]]
- [[craft/concepts/harness|概念：Harness]]
- [[craft/gaps/prompt-is-not-harness|认知缺口：Prompt ≠ Harness]]
