# 如何优化 AGENTS.md：从 Context Engineering 的角度

> AGENTS.md 不是"给 AI 的 README"。它是 Harness 的 Inform 层——模型看到的上下文直接决定模型的行为质量。优化 AGENTS.md，本质上是在做 Context Engineering。

---

## 先理解你在优化什么

AGENTS.md 在 Agent 启动时被发现、拼接、注入 System Prompt。它消耗的是**上下文窗口的 token 预算**——你写的每一行都在和对话历史、工具定义、代码片段争抢空间。Codex 默认限制 32KB 拼接上限。

这意味着优化 AGENTS.md 的核心矛盾是：**信息量 vs token 效率**。你需要在有限的 token 预算内，最大化模型的行为正确率。

这不是一个文档写作问题，这是一个信息工程问题。

---

## 原则一：信噪比优先

AGENTS.md 里每一句话都有"信号成本"——它占的 token 数——和"信号收益"——它多大程度上改变了 Agent 的行为。

**高信噪比写法**：

```markdown
## 禁止
- 不要用 `any`，用 `unknown`
- 不要在组件内直接 fetch，走 `src/lib/api.ts`
- 不要修改 migrations/ 下已有文件，只追加
```

**低信噪比写法**：

```markdown
## 类型安全
我们非常重视类型安全。在 TypeScript 开发中，应该尽可能避免使用 any 类型，
因为它会破坏类型系统的完整性。推荐使用 unknown 类型作为替代……
```

前者 3 行 = 3 条硬规则，Agent 违反概率极低。后者 3 行 ≈ 1 条软建议，Agent 执行率取决于它"记住了"这段话的哪部分。

**判断标准：如果删掉某一行，Agent 的行为不会有可观测的变化，那就删掉它。**

---

## 原则二：约束比告知更有效

Harness Engineering 的四动词框架——**Constrain → Inform → Verify → Correct**——揭示了一个被大多数人忽略的事实：

**AGENTS.md 天然是 Inform（告知）工具，但你应该尽量把它写成 Constrain（约束）工具。**

告知 vs 约束的区别：

| 类型 | 示例 | Agent 遵从率 |
|------|------|-------------|
| 告知 | "推荐使用函数式组件" | 中（会偶尔生成 class 组件） |
| 约束 | "禁止使用 class 组件。发现一律重写为函数式。" | 高（明确边界触发遵从） |
| 约束+理由 | "禁止 class 组件——项目 lint 规则会 reject。" | 最高（Agent 理解后果） |

**"禁止"清单是 AGENTS.md 中投入产出比最高的内容。** LLM 在面对明确边界时的遵从率显著高于面对模糊建议。而且约束是可累积的——每踩一次坑就加一条禁令，AGENTS.md 会自动演化为你项目的"踩坑手册"。

---

## 原则三：利用层级做 token 分级

AGENTS.md 的三层发现机制不只是"组织结构"，它是**token 预算分级工具**：

```
~/.codex/AGENTS.md            ← 全局：个人编码偏好（极简，<500 token）
repo-root/AGENTS.md           ← 项目：共享约定（核心，2000-4000 token）
packages/api/AGENTS.md        ← 模块：精准规则（按需加载）
```

**核心策略：只有 Agent 操作某个子目录时才需要看到的规则，绝不放在根目录。**

这不仅仅是组织整洁的问题——Monorepo 中，根目录放了所有模块的规则，Agent 在操作前端代码时要"读过"一遍后端 API 的数据库访问约定，纯粹浪费 token。分层后，Agent 的上下文始终保持精准。

---

## 原则四：结构化 > 散文

LLM 从结构化文本中提取信息的准确率远高于从段落散文中提取。优先使用：

- **表格**：规则映射（目录 → 职责 → 禁止项）
- **代码块**：精确命令（`pnpm test`、`docker compose up`）
- **列表**：有序步骤、无序约束
- **标题层级**：作为语义分区，帮 LLM 建立信息索引

避免使用：
- 长段落解释（LLM 倾向于"记住"段落开头和结尾，遗忘中间）
- 隐含在上下文中的规则（"我们之前说过……"——Agent 不保证记得）
- 条件复杂的嵌套逻辑（拆成多条独立规则）

---

## 原则五：AGENTS.md 是活文档，过时即有害

**过时的 AGENTS.md 比没有 AGENTS.md 更危险——Agent 会忠实执行过期规则。**

一个实际场景：项目从 Jest 迁移到 Vitest，但 AGENTS.md 里还写着"运行 `npx jest`"。Agent 在提交前乖乖跑 Jest——测试全过（因为 Jest 配置还没删），但实际 CI 跑 Vitest 时失败。Agent 不会质疑 AGENTS.md 的正确性，它只会执行。

**维护策略**：

1. 每次技术栈变更后立刻 review AGENTS.md
2. 在文件头加 `<!-- Last reviewed: 2026-04-13 -->` 做提醒
3. 如果团队有 CI，加一条"AGENTS.md 超过 30 天未修改则发出 warning"

---

## 原则六：闭环思维——AGENTS.md 只是 Inform，你还需要 Verify 和 Correct

最关键的认知：**AGENTS.md 写得再好，它也只是四动词框架中的 Inform 环节。**

只有 Inform 没有 Verify 的 Harness 是"发了邮件不跟进"——你告诉 Agent 不要用 `any`，但如果没有 Linter 强制检查，Agent 照样会偶尔犯错。

完整闭环是：

```
AGENTS.md（Inform）     → Agent 知道规则
CI Linter（Verify）     → 自动检查是否遵守
失败反馈（Correct）     → 把错误信息回注上下文，Agent 自修复
架构约束（Constrain）   → 从根本上让违规不可能发生
```

OpenAI Codex 团队的实践：3 名工程师指挥 Agent 写了 100 万行代码——不是因为他们的 AGENTS.md 写得完美，而是他们建了完整的四动词闭环：依赖分层 CI 强制（Constrain）+ 代码库地图（Inform）+ 重写过的 Linter（Verify）+ 失败回注自修复（Correct）。

**AGENTS.md 是起点，不是终点。** 写好它，然后问自己：谁来验证？谁来纠正？
