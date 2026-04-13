# How to Build a Multi-Agent Team in Hermes

**类型**: article
**日期**: 2026-04-13
**来源**: https://x.com/neoaiforecast/status/2043455838459920718
**作者**: @neoaiforecast
**标签**: #hermes #multi-agent #profile #SOUL-md #team-design

## 摘要

本文介绍了在 Hermes 中从单 Agent 升级为多 Agent 团队的完整方法。核心机制是 **Profile 隔离**——不是 delegate_task 那种临时子 Agent，而是持久化的角色环境隔离。每个 Profile 拥有独立的 memory、sessions、skills、personality、cron state 和 gateway state，形成真正的"角色边界"。文章提出了四角色团队模型（Orchestrator + Research + Writer + Builder），并强调"多 Agent 的起点是隔离，不是更好的 prompting"。

关键创新在于将 **SOUL.md**（身份定义）和 **AGENTS.md**（项目上下文）分离：SOUL.md 定义 agent 是谁（语气、优先级、行为），AGENTS.md 定义共享的项目规则。这种分离让每个 Profile 成为有独立人格的持久 Agent，而非换了名字的克隆。

## 关键要点

- **Profile ≠ Persona**：Profile 是隔离的 agent 环境（memory、session、skills、personality、cron、gateway 全部独立），不是仅改名字的 cosmetic persona
- **隔离是起点**：从单 Agent 到多 Agent 团队的关键跃迁不是更好的 prompting，而是状态隔离
- **四角色团队模型**：
  1. Orchestrator（编排）：规划、分解、排序、综合
  2. Research Specialist（研究）：证据、验证、怀疑精神、不确定性标注
  3. Writer（写作）：清晰、结构、受众意识、打磨
  4. Builder/Debugger（工程）：实现、调试、测试、可重现性
- **SOUL.md vs AGENTS.md 分离原则**：
  - SOUL.md = "who the agent is"（身份、语气、优先级）
  - AGENTS.md = "shared project context"（仓库结构、编码规范、工具规则）
  - 不混合——身份稳定，项目上下文可变
- **`--clone` 继承**：从工作中的 Profile 克隆新 Profile，继承 config.yaml + .env + SOUL.md，但 memory 和 session 独立
- **团队参考文件**（team-agents.md）：记录角色清单、交接规则、使用场景
- **messaging 整合**：Profile + Gateway = 跨平台（Telegram/Discord/Slack/WhatsApp/Signal/Email）的多 Agent 控制面
- **渐进式扩展**：先 Orchestrator + 1 个 Specialist → 验证 handoff → 再扩展

## 实践启发

1. **Profile 对 Atlas 的启示**：当前 Atlas 的 SOUL.md + IDENTITY.md + USER.md 结构已在实践 SOUL.md/AGENTS.md 分离原则——但单 Agent 模式下没有 Profile 隔离的需求
2. **团队设计原则**：最有用的 AI 团队映射到真实的职能角色（研究、写作、工程），不是任意的人格标签
3. **"小而精"原则**：最聪明的多 Agent 团队不是最大的团队，而是角色边界最清晰的团队

## 关联实体

- [[entities/hermes|Hermes]] - Profile 隔离是 Hermes 多 Agent 团队的核心机制
- [[entities/soul-md|SOUL.md]] - Agent 身份定义文件，与 AGENTS.md 分离使用
- [[topics/multi-agent-collaboration|多 Agent 协作]] - Profile 模式是第三种多 Agent 模式：不同于 delegate_task（临时子 Agent）和 OpenClaw 异步编排

## 引用此来源的页面

- [[entities/hermes]]
- [[entities/soul-md]]
- [[topics/multi-agent-collaboration]]
