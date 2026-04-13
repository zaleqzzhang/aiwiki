# 写作指南：如何用 Craft 素材库写文章

## 写作路径（三步）

### 第一步：选切入角度 → 从 gaps/ 开始

认知缺口是最好的文章起点——读者觉得自己懂，其实搞混了。

当前可用的 3 个认知缺口：

| 缺口 | 一句话 | 适合的文章标题方向 |
|------|--------|-------------------|
| **RAG ≠ Agent Memory** | 查百科 ≠ 翻工作日志 | "为什么给 Agent 加了 RAG 还是不会学习" |
| **Prompt ≠ Harness** | 写封邮件 ≠ 搭工作环境 | "Prompt 工程的天花板在哪里" |
| **Skill ≠ Tool** | 手 ≠ 肌肉记忆 | "Agent 的工具和技能是两回事" |

**操作**：读一遍目标缺口卡片，找到"写文章时可以这样切入"那段——那是开头的种子。

### 第二步：搭骨架 → 用 concepts/ 确定概念链

一篇好的技术文章有清晰的概念递进链。从概念卡片中选 2-4 个概念，排列成"先懂这个 → 才能懂那个"的顺序。

**示例文章骨架：**

**《你以为在做 Agent？可能只是在写 Prompt》**
1. 开头：用 `gaps/prompt-is-not-harness.md` 的切入点引起共鸣
2. 概念 1：Harness（`concepts/harness.md`）—— 先讲清"是什么"
3. 概念 2：Trace（`concepts/trace.md`）—— Harness 的核心能力之一
4. 概念 3：Skill（`concepts/skill.md`）—— 从 Trace 到可复用经验
5. 深度：用 `design-notes/hermes-memory-limit.md` 展示约束设计的智慧
6. 思辨：用 `perspectives/constraint-vs-unbounded-design.md` 引发思考
7. 结尾：回扣认知缺口，给出明确判断

**《Agent Memory 不是 RAG 2.0》**
1. 开头：用 `gaps/rag-is-not-memory.md` 的切入点
2. 概念 1：Agent Memory（`concepts/agent-memory.md`）—— 三维度区分
3. 概念 2：Hook（`concepts/hook-mechanism.md`）—— Memory 的实时更新靠什么
4. 深度：用 `design-notes/memos-triple-dedup.md` 展示去重的工程智慧
5. 思辨：用 `perspectives/memory-autonomy-vs-system-enforcement.md` 讨论治理
6. 结尾：Harrison Chase 的"Memory is curation"

**《AI Agent 怎么从错误中学习》**
1. 开头：反常识——大多数 Agent 每天都在重复犯错
2. 概念 1：Trace（`concepts/trace.md`）—— 学习的原材料
3. 概念 2：Skill（`concepts/skill.md`）—— 经验的结晶
4. 概念 3：Meta-Harness（`concepts/meta-harness.md`）—— 自动化闭环
5. 深度：用 `design-notes/skill-auto-generation-quality.md` 展示质量控制
6. 背景：用 `concepts/three-layer-learning.md` 放到更大图景
7. 结尾：从 trace 到 skill 到 meta-harness，Agent 学习的完整链路

### 第三步：填内容 → 用 design-notes/ 和 perspectives/ 增加深度

**写概念段落时**：
- 从概念卡片的"一句话"开始，这是读者的第一印象
- 用"与相邻概念的边界"防止读者混淆
- 用"展开时可用的例子"增加说服力

**写深度段落时**：
- 从设计笔记的"直觉反应"引起共鸣——这是读者心里的声音
- 用"设计考量"逐层展开——回答"为什么这么做"
- 用"对比视角"展示不同选择的代价

**写思辨段落时**：
- 从视角卡片的"核心问题"引入
- 展示两种视角各自的论据和代价
- 用"深层张力"揭示问题的本质
- 给出自己的判断（参考"使用建议"）

## 写作原则速查

| 原则 | 怎么做 | 反面 |
|------|--------|------|
| 概念先行 | 先讲清"是什么"和"为什么这么设计" | 一上来就贴代码 |
| 层层递进 | 从直觉反应 → 设计考量 → 深层启发 | 概念堆砌，没有递进 |
| 细节服务理解 | 每个技术细节回答"帮读者理解什么" | 堆 benchmark 数字 |
| 有观点有边界 | 有判断，同时说明适用范围 | 中立综述，什么都不敢说 |
| 填补认知缺口 | 击中"以为懂但搞混了"的地方 | 讲读者已经知道的 |

## 素材库速查表

```
wiki/craft/
├── concepts/           # 「这个概念是什么」
│   ├── harness.md          → Agent 运行时系统
│   ├── trace.md            → 执行轨迹，改进的数据源
│   ├── skill.md            → 结构化可复用经验
│   ├── agent-memory.md     → 跨会话持久记忆
│   ├── hook-mechanism.md   → 生命周期拦截器
│   ├── meta-harness.md     → 用 Agent 改进 Agent
│   └── three-layer-learning.md → 三层学习架构
│
├── design-notes/       # 「为什么这么做」
│   ├── hermes-memory-limit.md    → 3,575 字符约束
│   ├── hermes-subagent-sync-blocking.md → 同步阻塞设计
│   ├── memos-triple-dedup.md     → 三层去重机制
│   └── skill-auto-generation-quality.md → 双重质量关卡
│
├── gaps/               # 「你可能搞混了」
│   ├── rag-is-not-memory.md      → RAG ≠ 记忆
│   ├── prompt-is-not-harness.md  → Prompt ≠ 运行时
│   └── skill-is-not-tool.md      → 技能 ≠ 工具
│
├── perspectives/       # 「换个角度想」
│   ├── constraint-vs-unbounded-design.md    → 约束 vs 无界
│   └── memory-autonomy-vs-system-enforcement.md → 自治 vs 强制
│
└── drafts/             # 「草稿在这写」
```

## 写作命令

告诉我以下任何一种，我就开始起草：

1. **"写 [文章标题]"** → 我从素材库组装骨架 + 初稿
2. **"基于 [gap/concept 名] 写一篇"** → 我选切入角度 + 组装骨架
3. **"这个主题还缺什么素材"** → 我检查素材库，告诉你需要补充什么
4. **"把 [草稿文件] 润色一遍"** → 我基于素材库检查概念准确性和深度
