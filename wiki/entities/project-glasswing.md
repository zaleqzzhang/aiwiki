# Project Glasswing

**类型**: project
**创建日期**: 2026-04-13
**最后更新**: 2026-04-13
**来源数量**: 1

## 定义

Project Glasswing 是 Anthropic 于 2026 年 4 月 7 日发布的防御性网络安全项目。将 Claude Mythos Preview 限制给 12 家合作组织，仅用于发现和修复自有/维护的开源软件中的漏洞。这是 AI 安全领域首个"模型太危险不公开发布，但通过受控渠道赋能防守方"的实际案例。

## 合作组织（12 家，8 家已公开）

| 组织 | 领域 |
|------|------|
| Microsoft | Windows, Azure, Edge |
| Apple | macOS, iOS, Safari |
| Amazon | AWS 云基础设施 |
| Broadcom | VMware, Symantec |
| Cisco | 网络设备和安全产品 |
| CrowdStrike | 端点检测与响应 |
| Linux Foundation | Linux, Kubernetes, 开源生态 |
| Palo Alto Networks | 下一代防火墙和安全运维 |
| + 4 家未公开 | 关键开源基础设施 |

## 运作机制

- **限制性部署**：仅用于防御性安全（发现和修复漏洞）
- **协调披露**：所有发现的漏洞走 coordinated disclosure 流程
- **Anthropic 监督**：保留使用监控权和访问撤销权
- **沙箱环境**：模型在受限网络访问的沙箱中运行
- **合同约束**：合作方签约限于防御性用途

## 核心逻辑

1. Mythos 级能力将不可避免地扩散（"以月计，非以年计"）
2. 给防守方争取"先手优势"——在攻击方获得类似能力前修复尽可能多的漏洞
3. 上游修复流向下游：Linux/Firefox/Windows/macOS 的补丁惠及所有用户

## 行业反应

- **Simon Willison**："安全风险确实可信，让受信团队先行是合理的权衡"
- **微软 CISO Igor Tsyganskiy**：在 CTI-REALM benchmark 上验证 Mythos 显著优于前代
- **Linux 基金会 CEO Jim Zemlin**："安全专业知识一直是大团队的奢侈品，Glasswing 改变了这个等式"
- **批评者**：限制给大公司形成信息不对称、类似私人安全卡特尔

## 意义

| 层面 | 影响 |
|------|------|
| AI 安全治理 | 首个"太危险不公开发布"的真实案例，RSP 框架的实际应用 |
| 网络安全范式 | 从周期性审计 → 持续自动化评估 |
| Bug Bounty 经济 | AI 先摘低垂果实，人类转向业务逻辑和社工 |
| 攻防博弈 | 窗口期以月计，防守方独占优势有限 |
| Agent 伦理 | 当 Agent 能力达到危险水平，治理框架如何设计？ |

## 与其他实体的关系

- [[entities/claude-mythos|Claude Mythos]] → 核心模型
- [[entities/anthropic|Anthropic]] → 发起组织（待创建）

## 相关来源

- [[sources/project-glasswing-mythos|Project Glasswing]] - 完整报道

## 开放问题

- 12 家合作组织的选择标准是什么？如何平衡公平性？
- 当多个 AI 实验室都有 Mythos 级能力时，Glasswing 模式是否可复制？
- 开源社区维护者（非大公司）如何获得类似能力？
