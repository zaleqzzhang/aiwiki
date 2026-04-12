# Skill 系统

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 定义

Skill 系统是将 Agent 的任务轨迹提炼为可复用经验的机制。Skill 是从成功或失败的任务中提取的结构化知识，可以被后续任务自动检索和注入。

## 核心价值

- **经验复用**：一次学习，多次使用
- **失败转化为本能**：前一只 Agent 的失败变成后一只 Agent 的本能
- **持续进化**：Skill 随着使用不断迭代改进

## 两种实现模式

### Hermes：主动生成 + 后台 review

**生成方式**：
- Agent 自主调用 `skill_manage create`
- 后台周期性 review 和优化

**特点**：
- Agent 有主动权，可精心设计 Skill 结构
- Progressive disclosure 节省 token（启动时只注入索引）
- Skills Hub 社区生态

**已知问题**：
- Skill 重复制造（社区反馈高频问题）
- 去重依赖 Agent 自觉 + 后台 review
- 质量控制不如 MemOS 严格
- 无法追溯 Skill 来源

### MemOS：系统强制捕获 + 质量保障

**自动生成四步流程**：

**第一步：规则过滤**
- chunks 数量达到最低阈值
- 内容"非平凡"（纯闲聊不算）

**第二步：混合检索**
- FTS 全文搜索 + Vector 向量搜索 + RRF 融合
- 找出已有相关 Skill
- 决定：升级已有 Skill 还是创建新 Skill

**第三步：LLM 决策**
- confidence ≥ 0.7 → 升级已有 Skill（合并新经验）
- confidence < 0.3 → 生成全新 Skill
- 中间地带 → 跳过，等更多数据

**第四步：质量评分**
- 0-10 分打分
- ≥ 6 分才写入 skills 表
- 同时建立 task_skills 关联记录进化轨迹

**特点**：
- 100% 捕获率
- 完整进化轨迹（可追溯来源）
- 置信度阈值保证质量
- 异步执行，不阻塞主流程

## Auto-recall：自动注入

**核心问题**：生成了 Skill，如何让它在正确时机出现？

**解决**：通过 `before_agent_start` Hook 自动检索并注入。

**流程**：
1. Agent 收到新任务
2. Hook 提取任务关键词
3. 混合检索相关 Skill
4. 按相关度排序，取 Top-N
5. 注入到 System Prompt

**效果**：Agent 开始时已"看到"所有相关经验。

## Progressive Disclosure（渐进式披露）

Hermes 的节省 token 技巧：

**启动时**：只注入 Skill 的描述索引
**需要时**：再加载完整内容

避免一次性注入大量 Skill 导致上下文膨胀。

## 实战案例

**场景**：写作工蜂写推文时用了太多技术术语，读者看不懂。

**处理**：
1. Skill 系统捕获失败，生成 Skill："推文写作避免堆砌术语，每个技术概念必须配一句大白话解释"
2. 三天后，另一只写作工蜂接到新推文任务
3. Auto-recall 自动注入这条 Skill
4. 新工蜂从第一句话开始就用大白话写

**前一只工蜂的失败，变成了后一只工蜂的本能。**

## 数据对比

| 维度 | Hermes | MemOS |
|------|--------|-------|
| Skill 数量 | 依 Agent 生成 | 几百个（结构化） |
| 生成方式 | Agent 主动 | 系统自动 |
| 捕获率 | 依赖 Agent | ~100% |
| 质量控制 | 后台 review | LLM 评分 + 阈值 |
| 进化轨迹 | ❌ 无 | ✅ task_skills 表 |
| 社区生态 | ✅ Skills Hub | ❌ 暂无 |

## 与其他概念的关系

- [[entities/hook-mechanism|Hook 机制]] → 实现 100% 捕获
- [[entities/auto-recall|Auto-recall]] → 自动注入 Skill
- [[entities/harness-engineering|Harness Engineering]] → Skill 是轨迹捕获的价值实现
- [[entities/hermes|Hermes]] → 主动式 Skill 系统
- [[entities/memos|MemOS]] → 自动化 Skill 系统

## 相关来源

- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]] - Skill 系统的详细对比

## 实践启发

对于 Atlas / WorkBuddy：
- 当前：无系统化 Skill 提炼
- 可实现：
  1. 从工作记忆文件中提炼 Skill
  2. 基于任务类型自动分类
  3. 在新任务开始时自动注入相关 Skill
  4. 记录 Skill 的使用效果，迭代改进

## 开放问题

- Skill 的粒度如何确定？太粗还是太细？
- 如何防止 Skill 过时？
- 如何评估 Skill 的实际效果？
