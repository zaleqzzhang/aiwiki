# OpenClaw vs. Hermes Agent: The Race to Build AI Assistants That Never Forget

**类型**: article
**作者**: Janakiram MSV
**日期**: 2026-04-02
**来源**: https://thenewstack.io/persistent-ai-agents-compared/
**标签**: #openclaw #hermes-agent #persistent-agent #security #ecosystem

## 摘要

The New Stack 资深作者 Janakiram MSV 的深度对比文章。核心论点：AI Agent 正在从"会话式工具"分裂为两个物种——会话内工具（Claude Code / Codex / Cursor）和常驻式 Agent（OpenClaw / Hermes）。OpenClaw 是"生态优先"（Android for AI agents），Hermes 是"学习优先"（the agent that grows with you）。两者代表了 persistent agent infrastructure 的两种早期原型。文章类比 Kubernetes 从无状态容器到有状态服务的演进，预判 AI 也在经历类似转型。

## 关键要点

### 宏观判断："常驻 Agent"是一个新软件品类

- 会话式工具（Claude Code / Codex / Cursor）= 无状态容器，每次重启擦除上下文
- 常驻式 Agent（OpenClaw / Hermes）= 有状态服务 + 持久卷 + 学习循环
- 一位开发者记录了 26 天内 59 次 context compaction 后自建持久化层的经历
- 会话工具也在加持久化（Claude Code auto-memory、Cursor workspace context），但架构野心不同

### OpenClaw：生态优先

- **规模**：345K+ GitHub stars（截至 2026 年 4 月初），Peter Steinberger 创建
- **定位**：Android for AI agents — 规模大、第三方生态广、碎片化
- **核心价值**：50+ messaging 集成、模型无关（任意 LLM Provider）、跨渠道持久化
- **ClawHub**：社区 Skill 市场，数千个 Skills
- **安全危机**：
  - ClawHavoc 攻击：Koi Security 审计 2,857 个 Skills，发现 341 个恶意（335 来自同一攻击组织）
  - CVE-2026-25253（CVSS 8.8）：WebSocket 自动连接暴露认证 Token
  - 微软建议不要在个人/企业工作站运行
  - Cisco 称 OpenClaw 类个人 AI Agent 是"安全噩梦"
  - 后与 VirusTotal 合作扫描 Skills
- **跨渠道持久化是病毒式增长的核心驱动力**

### Hermes：学习优先

- **规模**：~22K GitHub stars（2026 年 4 月初）
- **三组件学习循环**：持久化记忆（FTS5 + LLM 摘要）→ 自主 Skill 创建 → 自训练循环（Atropos RL）
- **v0.6.0 新特性**：Profile 多实例隔离、MCP Server 模式（`hermes mcp serve`）
- **安全策略**：架构约束 > 事后修补（容器加固、Tirith 预执行扫描、文件系统 checkpoint 回滚）
- **基础设施成本**：$5 VPS 或 Serverless 可运行

### 选择框架

| 需求 | 推荐 | 理由 |
|------|------|------|
| 最大 messaging 覆盖 | OpenClaw | 50+ vs 7 |
| 持久化跨会话记忆 | Hermes | FTS5 + LLM 摘要 |
| 大型预置 Skill 生态 | OpenClaw | 数千 ClawHub Skills |
| 自我改进 Agent | Hermes | 闭环学习循环 |
| RL 训练和轨迹导出 | Hermes | Atropos 集成 |
| 内置安全防护 | Hermes | 保守架构 |
| 社区规模和第三方支持 | OpenClaw | 345K+ stars |
| 最小基础设施运行 | Hermes | $5 VPS |

### 趋势判断

- Hermes 已支持从 ClawHub 安装 Skill + OpenClaw→Hermes 迁移工具 → 趋向**融合**而非赢者通吃
- agentskills.io 标准设计目标就是跨平台可移植
- **核心未答问题**：Agent 积累的知识归谁？用户？平台？模型提供商？

## 新信息（与现有知识库补充）

- OpenClaw 创始人 Peter Steinberger 是奥地利开发者，2026 年 2 月加入 OpenAI，OpenClaw 移交独立基金会
- ClawHavoc 供应链攻击的具体数据：341/2,857 恶意 Skills
- CVE-2026-25253 具体漏洞：WebSocket 自动连接暴露认证 Token（CVSS 8.8）
- Hermes v0.6.0 新增 MCP Server 模式：`hermes mcp serve` 暴露会话给 IDE
- "26 天 59 次 context compaction"的真实案例（引用 Claude Code GitHub issue #34556）
- OpenClaw 345K stars vs Hermes 22K stars 的量级差距

## 关联实体

- [[hermes]] - "learning-loop-first"定位，学习循环三组件视角
- [[openclaw]] - "ecosystem-first"定位，345K stars、ClawHub、安全危机
- [[skill-system]] - agentskills.io 跨平台可移植标准
- [[harness-engineering]] - 从"无状态工具"到"有状态服务"的架构演进
- [[atropos]] - Hermes 自训练循环的 RL 框架（新实体候选）

## 引用此来源的页面

- [[entities/hermes]]
- [[entities/openclaw]]
- [[topics/agent-architecture]]
