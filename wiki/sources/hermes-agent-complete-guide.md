# What Is Hermes Agent? Complete Guide to the Self-Improving AI (2026)

**类型**: article
**作者**: NxCode Team
**日期**: 2026-04-12
**来源**: https://www.nxcode.io/resources/news/hermes-agent-complete-guide-self-improving-ai-2026
**标签**: #hermes-agent #complete-guide #pricing #learning-loop #migration

## 摘要

NxCode 团队出品的 Hermes Agent 全面指南，覆盖功能、架构、安装、定价、与 OpenClaw 对比、使用场景和局限性。文章数据截至 2026 年 4 月，包含多个社区评测引用和第三方数据。核心定位：Hermes 是 2026 年最有趣的开源 AI Agent 框架，解决了"让 Agent 使用越多越好"的核心问题。

## 关键要点

### 最新数据

- **64.2K GitHub stars**（截至 2026 年 4 月）——从 The New Stack 文章（2026-04-02）时的 22K 增长到 64K+
- **118 个 Skills**（96 bundled + 22 optional），26+ 类别
- **v0.8.0**（2026-04-08）：209 个合并 PR，Browser Use 集成，远程后端，worktree 并行
- **v0.1.0 → v0.8.0**：两个月从零到成熟，开发速度激进
- **MiniMax 合作**：M2.7 模型直接集成
- **Nous Portal**：Hermes 4 70B / 405B + 免费 Xiaomi MiMo v2 Pro（辅助任务）

### 定价模型（首次具体数据）

| 层级 | 月成本 |
|------|--------|
| 爱好者（本地 + Ollama） | $0 |
| 轻量用户（VPS + Hermes 4 70B） | $20-50 |
| 重度用户（VPS + 前沿模型） | $50-150 |
| 团队/重度（专用服务器 + 405B） | $100-400+ |

**关键发现**：73% 的 API 调用是固定开销（工具定义占近一半），平均每次 API 调用 ~$0.30（预算模型），每任务约 20 次调用。

### 学习循环的五步周期（更清晰的表述）

1. **Receive**：消息通过统一 Gateway 到达
2. **Retrieve**：FTS5 搜索持久记忆和 Skill（~10ms / 10,000+ 文档）
3. **Reason and Act**：LLM 处理，使用 Skills 和记忆指导方法
4. **Document**：5+ tool calls 后自主生成/更新 Skill（agentskills.io 标准）
5. **Persist**：结果、偏好、新知识存入持久记忆

### Benchmark

- 使用自创 Skill 的 Agent 完成研究任务比新实例快 **40%**（Nous Research 发布）

### 诚实的局限性（文章难得的客观评估）

1. **"Grows with you" 实质是结构化笔记+检索**，不是魔法。领域特定，不跨领域迁移
2. **反思和优化模块额外消耗 15-25% token**
3. **Honcho 自学习功能默认关闭**，多个用户反馈学习能力似乎不工作——文档缺口
4. **记忆透明度不如 OpenClaw**：OpenClaw 文件即记忆可直接检查，Hermes 自动管理不够透明
5. **v0.1→v0.8 两个月**：版本间 API 稳定性无保证
6. **6 个平台 vs OpenClaw 50+**
7. **非代码生成工具**：对话 Agent ≠ 编码 Agent，严肃软件工程仍需 Claude Code / Cursor

### 安全对比

- Hermes：**零 Agent CVE**
- OpenClaw：CVE-2026-25253（CVSS 8.8）+ 其他
- 内置迁移工具：`hermes claw migrate`

### 哲学对比（精辟概括）

| | Hermes | OpenClaw |
|---|--------|----------|
| 哲学 | Agent as a mind to develop | Agent as a system to orchestrate |

## 新信息（与现有知识库补充）

- Hermes stars 从 22K（04-02）暴涨到 64.2K（04-12），10 天 3x 增长
- 118 Skills 的具体构成：96 bundled + 22 optional，社区提交经安全扫描器检查
- 73% API 调用为固定开销——对成本优化有直接指导意义
- FTS5 检索延迟：~10ms / 10,000+ 文档——实际性能数据
- Skill 不跨领域迁移 + 15-25% token overhead——学习循环的真实代价
- Honcho 默认关闭——需显式启用，文档缺口导致用户困惑
- v0.8.0 新增：Browser Use 集成、Matrix Tier 1 支持、worktree 并行、209 PR

## 关联实体

- [[hermes]] - 最全面的功能和定价数据更新
- [[openclaw]] - 对比维度新增：哲学定位、安全 CVE、迁移工具
- [[skill-system]] - 118 Skills 具体数据、agentskills.io 标准细节
- [[nous-research]] - MiniMax 合作、Nous Portal 产品线
- [[atropos]] - RL 训练系统，batch trajectory generation（新实体候选）

## 引用此来源的页面

- [[entities/hermes]]
- [[entities/skill-system]]
- [[topics/agent-architecture]]
