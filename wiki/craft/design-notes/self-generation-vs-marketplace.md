# 设计笔记：Skill 自生成 vs 外部市场

## 设计选择

Agent 获取 Skill 的方式：**自己从经验中生成**（Hermes）vs **从外部市场安装**（OpenClaw ClawHub）。

## 直觉反应

"外部市场更好啊——13,000+ Skills 现成可用，为什么要让 Agent 自己慢慢积累？就像用 npm 安装包比自己写库快一万倍。"

## 设计考量

### 角度一：启动速度 vs 长期积累

| 维度 | 外部市场（OpenClaw） | 自生成（Hermes） |
|------|---------------------|------------------|
| **Day 1** | 立刻获得 13,000+ Skills | 从零开始，什么都不会 |
| **Day 30** | 同 Day 1（Skills 不会自动进化） | 已积累领域特定经验 |
| **Day 90** | 同 Day 1 | 使用自创 Skill 的任务完成速度 **+40%** |

Hermes 的 "grows with you" 承诺是真实的——但有两个重要限制：
1. **不跨领域迁移**：编程 Skills 对写作任务无帮助
2. **本质是结构化笔记 + 检索**，不是能力泛化

### 角度二：质量控制

外部市场面临**长尾质量问题**：
- Claude Code Skills 社区有 80,000+ Skills，但"大多数质量堪忧"
- OpenClaw ClawHub 12-20% 恶意率（ClawHavoc 攻击：341/2,857 恶意 Skills，分发 Atomic macOS Stealer）
- 质量问题不在数量，在于缺乏结构化的设计方法和测试协议

自生成的质量由系统设计保障：
- Hermes：5+ 工具调用、错误恢复、用户纠正时触发——**但会重复造 Skill**（社区高频反馈）
- MemOS：四步流程（规则过滤 → 混合检索 → LLM 决策 → 质量评分 ≥ 6 分写入）——100% 捕获率

### 角度三：安全面

这是两条路径最大的分水岭。

外部市场 = **打开一扇门**：
- Skill 是可执行的代码包（SKILL.md + 脚本）
- 每个安装都是一次信任授权
- 恶意开发者可植入病毒/后门/WebShell
- OpenClaw 的 9 个 CVE 中多个与 Skill/Plugin 供应链相关

自生成 = **不开门**：
- Hermes 零 Agent CVE（截至 2026-04），关键原因之一就是 Skill 自生成
- 不需要从外部下载未审计的代码
- 消除了供应链攻击向量
- 但也放弃了生态连接广度

### 角度四：第三条路（Superpowers）

obra 的 Superpowers 代表了第三条路径：**预定义的纪律规范**。

不从 trace 提炼，也不从市场安装——而是由人类工程师预先编写的最佳实践流程（TDD、代码审查、计划先行），按需加载到 Agent 上下文中。

特点：零依赖、任意 AI 会话可用、没有供应链风险、但也不会自动进化。

## 对比视角

三条路径的本质差异是**信任模型**：

| 路径 | 信任对象 | 风险 | 进化能力 |
|------|---------|------|---------|
| 自生成（Hermes） | 信任 Agent 的判断力 | 重复造轮子、不跨领域 | ✅ 自动进化 |
| 外部市场（OpenClaw） | 信任社区和审核机制 | 供应链攻击、质量长尾 | ❌ 静态分发 |
| 预定义规范（Superpowers） | 信任人类工程师 | 不自动进化、需手动维护 | ❌ 手动更新 |

## 启发

1. **"安全是杀手级特性"**：Hermes 的零 CVE 记录很大程度上归功于自生成路径——这不是附带好处，而是核心竞争力
2. **npm 类比是误导的**：软件包是确定性的（代码审计可以完全覆盖），Skill 是概率性的（LLM 执行路径不可预测）——软件包的安全模型不适用于 Skill 生态
3. **混合可能是最终形态**：自生成做领域特定 Skill，预定义做工程规范，外部市场做经过严格审计的高质量 Skill——但当前没有系统实现了这种混合
4. **118 vs 13,000 的数字不重要**：重要的是"哪些 Skill 在你的场景中真正有用"——大多数用户可能只用到 5-10 个高频 Skill

## 相关页面

- [[craft/concepts/skill|概念：Skill]] - Skill 概念的完整卡片
- [[craft/design-notes/skill-auto-generation-quality|Skill 质量双关卡]] - 自生成的质量控制机制
- [[craft/concepts/agent-security|概念：Agent 安全]] - 安全视角
- [[entities/skill-system|Skill 系统]] - 实体页面
- [[sources/hermes-agent-complete-guide|Hermes Agent Complete Guide]] - 118 Skills 数据来源
- [[sources/openclaw-vs-hermes-race|OpenClaw vs Hermes: The Race]] - 生态 vs 学习对比
