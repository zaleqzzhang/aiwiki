# 认知缺口：Agent 安全 ≠ 漏洞发现安全

## 读者"以为懂"的认知

"AI 安全就是防止 AI 被攻击嘛——防止越狱、防止注入、防止数据泄露。只要把 Agent 保护好，安全问题就解决了。"

## 实际区别

**Agent 安全（防御）和 Agent 作为安全武器（攻击/防御能力）是两个完全不同的问题域。**

| 维度 | Agent 安全（防御侧） | Agent 安全能力（攻防侧） |
|------|---------------------|-------------------------|
| **核心问题** | 如何保护 Agent 不被利用 | Agent 自身能发现/利用多少漏洞 |
| **威胁模型** | 外部攻击者 → Agent | Agent → 软件系统 |
| **典型案例** | OpenClaw 9 CVE、ClawHub 恶意 Skill | Claude Mythos 自主发现数千零日漏洞 |
| **治理重点** | 沙箱、权限、供应链 | 受控发布、合作组织限制、披露流程 |
| **失败后果** | Agent 被攻破、数据泄露 | 漏洞被滥用、网络武器化 |

Claude Mythos 是这个区别的极端案例：

- **不是 Agent 不安全**——Mythos 的对齐水平被评为"前所未有的可靠"
- **而是 Agent 太强**——Firefox 147 exploit 开发 90x 提升（181 次成功 vs 前代仅 2 次），自主发现所有主流 OS 和浏览器的零日漏洞
- **安全问题转向了**：从"如何防止 Agent 被攻破"到"如何防止 Agent 能力被滥用"

## 为什么误解常见

1. **"安全"一词的多义性**：人们用同一个词指代"保护 Agent"和"Agent 的安全研究能力"，在 Agent 语境下混为一谈
2. **历史惯性**：网络安全长期是"人攻击系统"的模型，"AI 自主攻击系统"是新范式，认知还没转过来
3. **Prompt Safety 的遮蔽效应**：大量 AI 安全讨论集中在对齐、越狱、幻觉——这些都是"保护 Agent"的视角，遮蔽了"Agent 作为武器"的视角
4. **正面叙事偏差**："AI 帮助发现漏洞"听起来是好事，人们不会自然联想到"同样的能力可以被用来攻击"

## 正确心智模型

Agent 安全实际上有**三个象限**：

```
Agent 安全全景
├── 第一象限：防御 Agent（保护 Agent 不被利用）
│   ├── 沙箱隔离（文件/命令/网络三层）
│   ├── 权限模型（Auto/Approval/Read-Only）
│   ├── 供应链安全（Skill/Plugin 审计）
│   └── 代表案例：OpenClaw 三层沙箱、Codex CLI Seatbelt
│
├── 第二象限：Agent 防御性安全（用 Agent 保护系统）
│   ├── 自动化漏洞发现（代码审计 Agent）
│   ├── 持续安全评估（替代周期性渗透测试）
│   ├── 协调披露（发现 → 修复 → 公开）
│   └── 代表案例：Project Glasswing、12 家合作组织
│
└── 第三象限：Agent 攻击性风险（Agent 能力被滥用）
    ├── 自主 exploit 开发（Mythos 90x 提升）
    ├── 能力扩散（"以月计，非以年计"）
    ├── 双重用途困境（发现漏洞 = 利用漏洞的前提）
    └── 治理框架：受控发布 + 能力评估 + 使用监控
```

**大多数人只想到第一象限**。第二象限是"好消息"（AI 帮我们修漏洞），第三象限是"坏消息"（AI 也能帮攻击者利用漏洞）。Glasswing 的精妙之处在于：**试图最大化第二象限的价值，同时最小化第三象限的风险**——通过限制谁能使用、用于什么目的。

## 文章切入点

开头用反直觉引入：

> "2026 年 4 月，一个 AI 模型发现了 FreeBSD 中存在 17 年的 RCE 漏洞。但 Anthropic 做了一个从未有人做过的决定：不公开发布这个模型。"

然后揭示：Agent 安全不只是"保护 Agent"——当 Agent 足够强大时，Agent 本身就是最大的安全问题。用三个象限模型帮读者建立完整认知，用 Glasswing 案例说明"受控发布"这种新型安全治理模式。

## 相关页面

- [[craft/concepts/agent-security|概念：Agent 安全]] - Agent 安全的完整概念卡片
- [[craft/gaps/agent-security-is-not-prompt-security|Agent 安全 ≠ Prompt 安全]] - 关联认知缺口
- [[entities/claude-mythos|Claude Mythos]] - "太强不公开发布"的模型
- [[entities/project-glasswing|Project Glasswing]] - 受控发布治理框架
- [[sources/project-glasswing-mythos|Project Glasswing]] - 核心来源
