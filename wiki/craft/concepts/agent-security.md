# 概念：Agent 安全

## 一句话

Agent 安全不等于模型安全——当 Agent 拥有工具调用权、网络访问权和文件系统权时，安全威胁从"输出不可靠"升级为"系统被攻破"。

## 为什么需要这个概念

传统 LLM 安全主要关注模型层面：幻觉、偏见、越狱。但当 LLM 被封装为 Agent——拥有执行命令、读写文件、访问网络的能力——安全问题的性质发生了根本变化：

- 幻觉 → 可能执行不存在的操作
- Prompt Injection → 可能被引导执行恶意操作
- 工具调用 → 可能超出授权范围操作

OpenClaw 的安全事件是这个问题的具体化：2026 年 3 月 4 天内 9 个 CVE（含 CVSS 9.9），135,000+ 实例暴露公网，ClawHub 12-20% 恶意 Skill 率。根因是架构性的：设计为消费级本地工具，但成长为网络化 Agent——信任假设在规模化后变得危险。

## 与相邻概念的边界

- **vs Prompt Safety（模型安全）**：Prompt Safety 关注模型输出的安全性（不说有害内容、不泄露敏感信息）。Agent 安全关注模型行动的安全性（不执行恶意操作、不超越权限边界）。一个是"说什么"，一个是"做什么"。
- **vs 传统应用安全**：传统应用的代码路径是确定性的——你可以审计所有分支。Agent 的行为是概率性的——同一个输入可能触发不同的工具调用链。这意味着传统安全测试（单元测试覆盖率）不足以保障 Agent 安全，需要补充对抗性测试和运行时监控。
- **vs Harness Engineering 的 Constrain 动词**：Constrain 是更广的概念——包括架构约束、依赖规则等。Agent 安全专注于 Constrain 中的安全子集——权限控制、沙箱隔离、供应链安全。

## 三层纵深防御

OpenClaw 的三层安全沙箱提供了参考架构：

| 层 | 职责 | 实现 |
|---|------|------|
| **文件系统沙箱** | 限制 Agent 能访问哪些文件 | Workspace 边界 + 路径白名单 |
| **命令执行沙箱** | 限制 Agent 能运行哪些命令 | 白名单 + Ask 模式 + safeBins 豁免 |
| **网络访问沙箱** | 限制 Agent 能访问哪些网络 | 域名白名单 + 防泄露 |

Codex CLI 采用了更底层的方案：macOS Seatbelt + Linux Landlock 内核级沙箱，三权限模式（Auto/Approval/Read-Only）。

Hermes 则从不同角度规避安全问题：容器加固 + 子 Agent 命名空间隔离 + 凭证轮换 + **Skill 自生成绕过供应链攻击向量** + Tirith 安全模块（硬性阻止 `curl | sh` 类危险命令）。

**Project Glasswing / Claude Mythos 代表了另一个维度的安全挑战**：当 Agent 的能力本身就是威胁。Mythos 能在所有主流 OS 和浏览器中自主发现数千个零日漏洞，Firefox 147 exploit 开发达到 90x 提升（181 次成功 vs 前代仅 2 次）。这不是"Agent 被攻破"的问题，而是"Agent 太强"的问题——首个因网络攻击能力过于危险而不公开发布的模型。Anthropic 的应对是 **受控发布**：仅限 12 家防御性安全合作组织，仅用于发现和修复自有软件漏洞。

## 设计直觉

Agent 安全的核心张力是：**能力 vs 控制**。

给 Agent 更多能力（更多工具、更大文件系统访问、更广网络权限）→ Agent 更能干。
给 Agent 更多控制（更严格的沙箱、更频繁的人工审批）→ Agent 更安全但更慢。

四种解决方式：
1. **信任但验证**：允许执行但记录一切（Hermes 插件钩子模式）
2. **分级授权**：低风险操作自动执行，高风险操作暂停等人确认（OpenClaw Human-in-the-Loop）
3. **最小权限**：默认拒绝一切，按需开放（Codex CLI Read-Only 默认模式）
4. **受控发布**：能力本身就是风险时，限制谁能使用（Project Glasswing 模式——12 家合作组织 + 仅限防御性用途 + Anthropic 保留撤销权）

## 理解的关键转折

**开放生态是安全的最大威胁面。**

Hermes 零 Agent 特定 CVE（截至 2026-04），关键原因之一是 Skill 自生成——不需要从外部市场下载未审计的代码。OpenClaw 的 12-20% 恶意 Skill 率（分发 Atomic macOS Stealer）说明了开放 Skill 市场的真实风险。

这带来一个深层权衡：**开放生态的连接广度 vs 封闭生态的安全深度**。OpenClaw 选择了 24+ 平台 + 13,000+ 社区 Skill，获得了最大连接性但暴露了最大攻击面。Hermes 选择了 6 平台 + 自生成 Skill，牺牲了生态广度但消除了供应链攻击向量。

**没有"两全其美"的方案**——只有在你的场景中，安全和能力哪个更重要。

## 展开时可用的例子

- **OpenClaw 安全事件时间线**：CVE-2026-25253（CVSS 8.8）WebSocket 劫持 → 4 天 9 个 CVE → 135,000+ 暴露实例 → ClawHub 恶意 Skill 分发 Atomic macOS Stealer。根因：消费级信任假设 + 网络化部署 = 灾难性攻击面。
- **Codex CLI 三权限模式**：Auto（全自动执行 + Seatbelt 沙箱）→ Approval（每步确认）→ Read-Only（只能看不能改）。默认 Read-Only 体现最小权限原则。
- **AGENTS.md 的 Prompt Injection 风险**：Agent 将 AGENTS.md 当作可信指导执行，恶意仓库可以在 AGENTS.md 里注入操作指令。开放标准的固有张力：互操作性 ↔ 信任边界。
- **Project Glasswing 受控发布**：当 Agent 能力本身就是武器（自主发现零日漏洞、90x exploit 提升），安全问题从"防止被攻破"升级为"限制谁能使用"。12 家合作组织 + 仅限防御性用途 + 协调披露 + 沙箱环境 + 合同约束——五层治理机制确保能力不被滥用。这是 Agent 安全的"第四象限"：不是 Agent 不安全，而是 Agent 太安全（太强）。

## 相关页面

- [[entities/openclaw|OpenClaw]] - 安全事件的详细记录
- [[entities/hermes|Hermes]] - 安全设计的对比参考
- [[entities/claude-mythos|Claude Mythos]] - "太强不公开发布"的极端安全案例
- [[entities/project-glasswing|Project Glasswing]] - 受控发布的治理框架
- [[craft/concepts/harness|概念：Harness]] - 安全是 Harness 的核心职责之一
- [[craft/concepts/four-verb-framework|概念：四动词框架]] - Constrain 是安全的主要动词
- [[craft/perspectives/open-standard-vs-deep-integration|视角：开放标准 vs 深度集成]] - 开放生态安全张力
- [[sources/hermes-vs-openclaw-2026|Hermes vs OpenClaw 2026]] - 安全数据的核心来源
- [[sources/project-glasswing-mythos|Project Glasswing]] - Mythos 能力与受控发布详情
