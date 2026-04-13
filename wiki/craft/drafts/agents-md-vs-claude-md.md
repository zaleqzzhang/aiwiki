# AGENTS.md vs CLAUDE.md：通用标准和专属配置，你都需要

> 一个管广度，一个管深度。不是二选一，而是分层共存。

---

## 先搞清楚它们是什么

**AGENTS.md** 是跨 AI 工具的开放标准——放在仓库里，告诉所有 Agent 这个项目怎么干活。Claude Code、Codex、Cursor、Copilot 都认它。由 Linux Foundation AAIF 治理，行业标准路线。

**CLAUDE.md** 是 Claude Code 的专属配置——只有 Claude Code 读它，但能做到 AGENTS.md 做不到的事。

核心区别一句话：**AGENTS.md 追求"所有工具都能用"，CLAUDE.md 追求"一个工具用到极致"。**

---

## 三个关键差异

**1. 层级精细度**

AGENTS.md 三层：全局 → 项目根 → 子目录，逐层拼接。

CLAUDE.md 六层：组织策略（IT 强制）→ 项目规则 → 用户偏好 → 本地覆盖 → 子目录 → 自动记忆。多出的三层不是冗余——组织级管控、个人 gitignored 配置、自动学习，每一层都解决 AGENTS.md 覆盖不到的场景。

**2. 加载策略**

AGENTS.md 启动时全量拼接。CLAUDE.md 按需加载——子目录配置只在访问时才注入，Scoped Rules 按文件路径模式匹配激活。**不浪费 token，精准命中。**

**3. 自动学习**

AGENTS.md 是静态文件，人写人维护。CLAUDE.md 有 Auto Memory——Claude 自动把会话中发现的模式写入记忆，每会话 200 行。**配置文件自己会成长。**

---

## 我的判断

**底层用 AGENTS.md，上层用 CLAUDE.md。**

80% 的通用内容——构建命令、测试流程、架构约定——写在 AGENTS.md 里，所有工具共享。20% 的高级能力——Scoped Rules、Auto Memory、组织策略——交给 CLAUDE.md。

用 `ln -s AGENTS.md CLAUDE.md` 做桥接，一份内容两个入口。Claude 特有配置放 `.claude/rules/`。

这不是站队问题。**通用标准保证你不被锁死在一个工具上，专属配置保证你用好手上的工具。** 两者的关系像 HTML 标准和浏览器私有 CSS——标准管底线，实现管体验。

真正危险的是只选一边：只用 AGENTS.md，你放弃了 Claude Code 一半的能力；只用 CLAUDE.md，你把自己绑死在一个厂商上。

**两个都写，十分钟的事。**
