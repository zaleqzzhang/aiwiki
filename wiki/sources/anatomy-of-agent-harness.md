# The Anatomy of an Agent Harness

**类型**: article
**日期**: 2026-04-06
**来源**: https://x.com/akshay_pachaar/status/2041146899319971922
**作者**: [[entities/akshay-pachaar|Akshay Pachaar]]
**标签**: #AgentHarness #HarnessEngineering #生产实践

## 摘要

深度拆解 Agent Harness 的 12 个组件，综合 Anthropic、OpenAI、LangChain 等框架的实践经验。提出 "If you're not the model, you're the harness" 的定义，将 Harness 明确为包在模型外层的完整软件基础设施。核心论断：**Harness Is the Product** — 同样的模型，不同的 Harness 设计，性能差异可达 20+ 排名。

## 关键要点

### Harness 的定义
> "If you're not the model, you're the harness." — Vivek Trivedy

**三层工程**：
- **Prompt engineering** → 制造指令
- **Context engineering** → 管理所见
- **Harness engineering** → 包含以上 + 工具编排、状态持久化、错误恢复、验证循环、安全强制

### Harness 的 12 个组件

1. **Orchestration Loop** - 心跳，实现 TAO（Thought-Action-Observation）循环
2. **Tools** - Agent 的手，schema 定义 + 沙箱执行
3. **Memory** - 多时间尺度（短期对话 + 长期持久化）
4. **Context Management** - 对抗 context rot（Chroma: 关键内容在 mid-window 时性能降 30%+）
5. **Prompt Construction** - 分层组装（system + tools + memory + history + user）
6. **Output Parsing** - 原生 tool calling + 结构化输出
7. **State Management** - Checkpointing + Time-travel debugging
8. **Error Handling** - 四种错误类型（transient/LLM-recoverable/user-fixable/unexpected）
9. **Guardrails and Safety** - 输入/输出/工具三层防护
10. **Verification Loops** - 验证工作让质量提升 2-3x（Boris Cherny）
11. **Subagent Orchestration** - Fork/Teammate/Worktree 三种模式

### 关键设计决策

**七个选择**：
1. 单 Agent vs 多 Agent → 先最大化单 Agent
2. ReAct vs Plan-and-Execute → LLMCompiler 报告 3.6x 加速
3. Context 管理 → 五种生产策略
4. 验证循环设计 → Guides（前馈）vs Sensors（反馈）
5. 权限和安全架构 → Permissive vs Restrictive
6. 工具范围策略 → Vercel 删掉 80% 工具后效果更好
7. Harness 厚度 → Anthropic 赌薄 Harness + 模型进化

### 脚手架隐喻

**关键洞察**：脚手架在建筑完成后会被拆除。随着模型进步，Harness 复杂度应该降低。

**共进化原则**：模型现在会带着特定 Harness 进行后训练。Claude Code 的模型学会了使用它训练时的特定 Harness。

## 核心框架

```
Agent Harness = 模型外的所有软件基础设施

Model（CPU）+ Harness（OS）
├── Context Window → RAM
├── External DB → Disk
├── Tools → Device Drivers
└── Orchestration Loop → Process Scheduler

关键设计：
- 最小化高信号 token 集合
- 验证循环让质量提升 2-3x
- 脚手架隐喻：模型进步 → Harness 简化
```

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 核心概念，文章定义了三层工程
- [[entities/hook-mechanism|Hook 机制]] - 文章提到 Guardrails 和 Safety 的实现
- [[entities/akshay-pachaar|Akshay Pachaar]] - 作者，Agent Harness 实践者
- [[entities/hermes|Hermes]] - 文章提到 ReAct vs Plan-and-Execute 的选择
- [[entities/openclaw|OpenClaw]] - 文章提到 Subagent Orchestration 模式

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]] - 补充 Harness 的 12 组件视角
- [[topics/multi-agent-collaboration|多 Agent 协作]] - 提供单 Agent vs 多 Agent 的决策框架

## 扩展阅读

1. Beren Millidge: "Scaffolded LLMs as Natural Language Computers"
2. Anthropic: Context Engineering Guide
3. LangChain: The Anatomy of an Agent Harness
