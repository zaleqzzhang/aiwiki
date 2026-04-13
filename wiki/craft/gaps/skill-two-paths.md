# 认知缺口：Skill 不止"从经验中学习"一条路

## 读者"以为懂"的认知

"Skill 就是 Agent 从使用中学到的经验嘛——犯了错，提炼教训，下次不再犯。就像人类的肌肉记忆。"

## 实际区别

**Skill 至少有三条路径，"从经验中学习"只是其中一条。**

| 路径 | 代表 | 机制 | 优势 | 劣势 |
|------|------|------|------|------|
| **积累→提炼** | Hermes | 从 Trace 中自动提取可复用经验 | 自动进化、贴合你的场景 | 冷启动慢、不跨领域、会重复造 Skill |
| **安装→使用** | OpenClaw ClawHub | 从外部市场下载打包好的 Skill | 立刻可用、生态丰富（13,000+） | 供应链风险（12-20% 恶意率）、不进化 |
| **规范→注入** | Superpowers (obra) | 人类预定义的工程最佳实践 | 零风险、质量有保障、任意 Agent 可用 | 不自动进化、需人工维护 |

大多数人只知道前两条路径，很少有人意识到"预定义纪律规范"也是一种 Skill。

## 为什么误解常见

1. **"学习"叙事的吸引力**："Agent 能从经验中学习"比"Agent 能加载预定义规范"听起来更酷，媒体和营销都偏向第一种叙事
2. **Hermes 的品牌效应**：Hermes 把"自我改进"作为核心卖点，强化了"Skill = 从经验中提炼"的单一印象
3. **Superpowers 知名度低**：obra 的 Superpowers 项目远不如 Hermes/OpenClaw 知名，第三条路径缺乏曝光
4. **类比误导**："肌肉记忆"这个比喻太成功了——它精准地描述了第一条路径，但遮蔽了另外两条

## 正确心智模型

Skill 的本质是**结构化的可复用经验单元**。获取方式不影响它的本质。

```
Skill 获取的三条路径
├── 路径一：从 Trace 提炼（经验型）
│   ├── 触发：5+ 工具调用、错误恢复、用户纠正
│   ├── 输出：SKILL.md（触发条件 + 流程 + 输出规范）
│   └── 进化：Skill Self-Improvement（patch/edit/delete）
│
├── 路径二：从市场安装（生态型）
│   ├── 来源：ClawHub / 社区注册表 / GitHub
│   ├── 输出：SKILL.md + 可执行脚本
│   └── 进化：依赖上游更新
│
└── 路径三：预定义规范（纪律型）
    ├── 来源：人类工程师编写
    ├── 输出：纯 Markdown 最佳实践流程
    └── 进化：手动维护

三条路径的共同点：
- 都是结构化的知识单元
- 都可以注入 Agent 上下文
- 都在特定场景下自动触发或按需加载
```

**选择指南**：
- **你在一个新领域摸索** → 路径一（让 Agent 跟你一起学）
- **你需要现成的工具集成** → 路径二（但严格审计来源）
- **你有成熟的工程规范** → 路径三（把团队知识转化为 Skill）
- **最佳实践**：三条路径混合使用，各取所长

## 文章切入点

开头用反直觉引入：

> "提到 Agent 的 Skill 系统，大多数人想到的是'从经验中学习'——像一个不断积累的手艺人。但 Hermes 的 118 个 Skills、OpenClaw 的 13,000 个 Skills、obra 的 Superpowers——它们来自三条完全不同的路径。"

然后展开三条路径的设计选择和权衡，用安全（12-20% 恶意率）和质量（80,000+ 但大多质量堪忧）的数据说明"数量不等于质量"。最后引出一个更深的问题：**Agent 的"知识"应该从哪里来？**

## 相关页面

- [[craft/concepts/skill|概念：Skill]] - Skill 概念的完整卡片
- [[craft/design-notes/self-generation-vs-marketplace|自生成 vs 外部市场]] - 路径一 vs 路径二的深度对比
- [[craft/design-notes/skill-auto-generation-quality|Skill 质量双关卡]] - 路径一的质量控制
- [[craft/gaps/skill-is-not-tool|Skill ≠ Tool]] - 关联认知缺口
- [[entities/skill-system|Skill 系统]] - 实体页面
- [[sources/hermes-agent-complete-guide|Hermes Agent Complete Guide]] - 118 Skills 数据
- [[sources/openspec-superpowers-harness|OpenSpec、Superpowers 和 Harness]] - 第三条路径来源
