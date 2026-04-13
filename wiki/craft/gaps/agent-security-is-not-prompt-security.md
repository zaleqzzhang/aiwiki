# 认知缺口：Agent 安全 ≠ Prompt 安全

## 读者"以为懂"的认知

"Agent 安全就是防止 Prompt Injection 和越狱嘛。做好输入过滤、加上 guardrails、限制敏感话题——和 LLM 安全是一回事。"

## 实际区别

**Prompt 安全管的是"模型说了什么"，Agent 安全管的是"Agent 做了什么"。**

| 维度 | Prompt 安全 | Agent 安全 |
|------|-----------|-----------|
| **威胁面** | 模型输出 | 模型行动（文件操作、命令执行、网络访问） |
| **最坏情况** | 输出有害内容 | 系统被攻破、数据被盗、恶意代码被执行 |
| **攻击向量** | Prompt Injection、越狱 | 工具滥用、权限升级、供应链攻击、Skill 恶意代码 |
| **防御手段** | 输入过滤、输出检查 | 沙箱隔离、权限模型、Hook 拦截、Human-in-the-Loop |
| **影响范围** | 单次交互 | 整个系统（文件系统、网络、其他服务） |

OpenClaw 的安全事件是最好的说明：

- **WebSocket 劫持**（CVE-2026-25253, CVSS 8.8）——攻击者不需要任何 Prompt Injection，直接通过无认证的 /api/export-auth 端点提取所有 API 密钥
- **ClawHub 恶意 Skill 分发 Atomic macOS Stealer**——不是模型被骗了，是用户安装了含恶意代码的 Skill 包
- **135,000+ 实例暴露公网**——不是 LLM 安全问题，是部署安全问题

这些攻击没有一个需要和 LLM 交互。Agent 安全的威胁面远大于 Prompt 安全。

## 为什么误解常见

1. **LLM 安全研究的路径依赖**：学术界和工业界最早关注的是模型安全（对齐、越狱、幻觉），Agent 安全是更新的领域，认知还没跟上
2. **"Agent"这个词的误导**：人们把 Agent 想成"会聊天的 AI"，忘了 Agent 还有执行能力——它能运行命令、读写文件、访问网络
3. **安全团队的组织盲区**：很多公司的 AI 安全团队从 ML 安全转型而来，擅长模型层安全但不熟悉系统层安全（沙箱、权限、供应链）

## 正确心智模型

Agent 安全 = Prompt 安全 + **系统安全** + **能力治理**

```
Prompt 安全（模型层）
├── 输入过滤（防止 injection）
├── 输出检查（防止有害内容）
└── Guardrails（话题限制）

系统安全（Harness 层）← 大多数人忽略的部分
├── 沙箱隔离（文件系统 / 命令执行 / 网络访问）
├── 权限模型（Auto / Approval / Read-Only）
├── Hook 拦截（before_tool_call 参数校验）
├── 供应链安全（Skill/Plugin 来源审计）
├── Human-in-the-Loop（高风险操作暂停）
└── 部署安全（认证、网络隔离）

能力治理（Agent 层）← 最新出现的维度
├── 受控发布（限制谁能使用危险能力）
├── 协调披露（发现→修复→公开的流程管理）
├── 使用监控（Anthropic 保留撤销权）
└── 案例：Project Glasswing（Mythos 90x exploit 提升→不公开发布）
```

**真正该问的问题不是"Agent 会不会说危险的话"，而是"Agent 能接触到什么、能执行什么、能力有多强、谁在监管它的行动"。**

## 文章切入点

开头用 OpenClaw 事件引入：
> "2026 年 3 月，4 天，9 个 CVE。但没有一个是因为 Prompt Injection。"

然后揭示"Agent 安全 ≠ Prompt 安全"的认知缺口，用三层沙箱 + 权限模型 + 供应链安全展开"Agent 安全到底管什么"。最后用 Hermes vs OpenClaw 的安全差异（零 CVE vs 九 CVE）说明"架构选择决定安全底线"。

## 相关页面

- [[craft/concepts/agent-security|概念：Agent 安全]] - 完整概念卡片
- [[craft/gaps/agent-security-is-not-vuln-hunting|Agent 安全 ≠ 漏洞发现安全]] - 关联认知缺口（能力治理维度）
- [[craft/perspectives/controlled-vs-open-release|控制发布 vs 开放发布]] - 能力治理的核心争论
- [[entities/openclaw|OpenClaw]] - 安全事件详细记录
- [[entities/hermes|Hermes]] - 安全设计对比
- [[craft/concepts/four-verb-framework|概念：四动词框架]] - Constrain 是安全的主要动词
- [[sources/hermes-vs-openclaw-2026|Hermes vs OpenClaw 2026]] - 安全数据来源
- [[sources/openclaw-prompt-context-harness|深度解析 OpenClaw 设计哲学]] - 三层安全沙箱来源
