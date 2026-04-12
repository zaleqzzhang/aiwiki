# LLM Wiki 知识库导出

**导出时间**: 2026-04-12 12:14
**统计**: 3 来源 | 9 实体 | 3 主题 | 1 综合

---

# 一、来源摘要 (3)

## 1. Continual Learning for AI Agents

**作者**: Harrison Chase
**日期**: 2026-04-05
**来源**: https://x.com/hwchase17/status/2040467997022884194

### 摘要

大多数关于持续学习的讨论聚焦于更新模型权重，但对于 AI Agent，学习可以发生在三个不同层次：模型层、Harness 层、上下文层。理解这一区别会改变我们对"可改进系统"的构建思路。

Agent 系统的三层架构：
- **Model（模型层）**：模型权重本身
- **Harness（Harness 层）**：驱动 Agent 的代码 + 固定的指令和工具
- **Context（上下文层）**：Harness 之外的配置性内容（指令、技能、工具），也称为"记忆"

### 关键要点

**模型层持续学习**
- 技术手段：SFT、RL（如 GRPO）
- 核心挑战：**灾难性遗忘** — 模型在新数据上更新后，会退化之前的能力

**Harness 层持续学习**
- 优化方法：Meta-Harness — 让 Agent 在一批任务上运行 → 评估 → 存储日志 → 编码 Agent 分析 trace 并建议修改 Harness 代码

**上下文层持续学习**
- 更新粒度：Agent 层面 / 租户层面（用户/组织/团队）
- 更新方式：离线批处理（"做梦"）或热路径更新
- 另一维度：显式（用户提示）vs 隐式（Agent 自主）

**Trace 是核心**
所有流程都依赖 trace — Agent 完整执行路径的记录。

---

## 2. 同步阻塞 vs 异步编排：Hermes Delegate 与 OpenClaw 多 Agent 机制对比

**作者**: servasyy_ai
**日期**: 2026-04-11
**来源**: https://x.com/servasyy_ai/status/2042951017462169812

### 摘要

本文深入对比两种多 Agent 协作设计哲学：Hermes 的 `delegate_task` 机制（同步阻塞、"总承包商-分包商"模式）与 OpenClaw 的 subagent 系统（异步编排、"交响乐团"模式）。两者在架构、通信模式、隔离性、灵活性上存在本质差异，适用于不同场景。

### 关键要点

**Hermes delegate：同步阻塞 + 强隔离**
- 同步阻塞：父 agent 等待所有子 agent 完成后返回
- 强隔离：子 agent 的中间推理不污染父 agent 上下文，只返回 summary
- 并行执行：支持最多 3 个任务并行
- 无超时机制，无 Steer 能力
- Token 效率极高，天然沙箱

**OpenClaw Subagent：异步编排 + 灵活协作**
- 异步事件驱动：spawn 后立即返回，结果通过 announce 推送
- Steer 机制：向运行中的子 agent 发送引导消息
- 生命周期管理：runTimeoutSeconds、runs.json 持久化、resumeSessionId 恢复
- 支持外部 Agent（ACP 协议）
- Token 开销较大，状态管理复杂

### 架构对比

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 同步/异步 | 同步阻塞 | 异步事件驱动 |
| 隔离性 | 强 | 弱 |
| Token 效率 | 极高 | 较低 |
| 超时控制 | ❌ | ✅ |
| Steer 能力 | ❌ | ✅ |
| 持久化恢复 | ❌ | ✅ |

---

## 3. 给你的 Agent 搭操作系统（实战篇）：Hermes-Agent vs 自建 MemOS

**作者**: servasyy_ai
**日期**: 2026-04-10
**来源**: https://x.com/servasyy_ai/status/2042473092036116847

### 摘要

本文基于 Harness Engineering 三根支柱（评估闭环、架构约束、记忆治理），深度对比两种实现路径：Hermes-Agent（有界热记忆 + 主动进化）和自建 MemOS（无界记忆 + 系统强制）。两种路径都在践行 Harness Engineering，但设计哲学截然不同。

### Harness Engineering 三根支柱

1. **记忆治理** → 解决 Agent 失忆症
2. **架构约束** → 明确 Agent 职责边界
3. **评估闭环** → 保证 Agent 质量

### 记忆治理对比

**Hermes 三层记忆架构**：
- L1 热记忆：MEMORY.md（2200 字符上限）+ USER.md
- L2 外部 Provider：Honcho/Mem0 等
- L3 冷检索：Session History（SQLite FTS5）

**MemOS 无界记忆**：
- PostgreSQL 17 + pgvector
- 35,000+ 条记忆，65,524 条关系边
- Hook 机制实现 100% 捕获
- 智能去重三层机制
- 混合搜索：BM25 + 向量 + RRF

### 架构约束对比

**Hermes**：主 Agent + 子 Agent 并行
**MemOS**：多常驻 Agent + 工蜂系统

**MemOS Skill 自动生成四步流程**：
1. 规则过滤
2. 混合检索
3. LLM 决策（confidence ≥ 0.7 升级）
4. 质量评分（≥ 6 分写入）

### 评估闭环对比

**Hermes**：Cron 调度 + 消息推送
**MemOS**：心跳检查 + 智库审核 + 反思系统

### 成本对比

| 维度 | Hermes-Agent | MemOS |
|------|-------------|-------|
| 安装时间 | 10 分钟 | 1-2 周 |
| API 成本 | $10-50/月 | $200-300/月 |
| 适合时长 | < 3 个月 | > 6 个月 |

---

# 二、核心实体 (9)

## 1. Harness Engineering

**定义**: 构建可靠 Agent 系统的方法论框架。核心理念：竞争优势不再是 prompt，而是 Harness 捕获的轨迹。

**三根支柱**：
1. 记忆治理 → 解决 Agent 失忆症
2. 架构约束 → 明确 Agent 职责边界
3. 评估闭环 → 保证 Agent 质量

**核心洞察**：
- 轨迹是核心资产
- 系统强制 vs Agent 自觉（100% vs 60% 捕获率）
- 长期 ROI：短期用框架，长期自建

---

## 2. Hook 机制

**定义**: Agent 系统中的事件拦截器。在 Agent 生命周期的关键节点自动触发预定义逻辑，实现系统级强制执行。

**关键 Hook**：
- `before_agent_start`：启动时自动注入相关记忆和 Skill
- `agent_end`：结束时自动捕获对话

**核心价值**：不给 Agent 选择的机会，系统强制执行 → 100% 捕获率。

---

## 3. Skill 系统

**定义**: 将 Agent 的任务轨迹提炼为可复用经验的机制。

**两种模式**：
- Hermes：Agent 主动创建 + 后台 review
- MemOS：系统自动生成 + 质量评分

**Auto-recall**：
- `before_agent_start` Hook 自动检索并注入相关 Skill
- 前一只工蜂的失败，变成后一只工蜂的本能

---

## 4. SOUL.md

**定义**: Agent 层面的持久记忆模式。记录其"灵魂" — 核心身份、行为准则、积累的经验。

**核心特征**：
- 持久性：跨越会话存在
- 自主更新：Agent 可更新自己的 SOUL.md
- 身份定义：记录 Agent 是谁、擅长什么、如何行为

**案例**：OpenClaw 使用 SOUL.md 作为上下文层学习的核心。

---

## 5. Steer 机制

**定义**: 运行时引导能力：父 agent 可以向正在运行中的子 agent 发送消息，调整其方向、提供额外上下文、或查询进度。

**特点**：
- 非中断式：不杀死子 agent
- 限流保护：每 2 秒最多一次
- 消息上限：最大 4000 字符

**对比**：Hermes 无 Steer，用户查询进度会误杀所有子 agent。

---

## 6. Hermes

**定义**: 同步阻塞式多 Agent 框架，采用"总承包商-分包商"模式。

**核心特征**：
- 同步阻塞，强隔离
- 最多 3 个任务并行
- 无超时机制，无 Steer 能力
- Token 效率极高

**适用**：个人 AI 助手，快速启动，强隔离场景。

---

## 7. OpenClaw

**定义**: 异步编排式多 Agent 框架，采用"交响乐团"模式。

**核心特征**：
- 异步事件驱动
- Steer 机制，持久化恢复
- 支持外部 Agent（ACP 协议）
- Token 开销较大

**适用**：多 Agent 长期协作，需要灵活调整方向。

---

## 8. 灾难性遗忘

**定义**: 模型层持续学习的核心挑战：当模型在新数据或新任务上更新权重时，倾向于丧失之前已掌握的知识和能力。

**应对**：
- 选择性更新（如 LoRA）
- 架构层面规避：将学习转移到 Harness 层或上下文层

---

## 9. Meta-Harness

**定义**: Harness 层持续学习的方法：让 Agent 在一批任务上运行 → 评估 → 存储日志 → 编码 Agent 分析 trace → 建议修改 Harness 代码。

**核心洞察**：
- Trace 是数据源
- Agent 改进 Agent
- 自动化程度高

---

# 三、核心主题 (3)

## 1. Agent 架构

AI Agent 架构正在从简单的"模型 + 提示词"演变为多层可配置系统。

**三层架构**：
- Model（模型层）→ 模型权重
- Harness（Harness 层）→ 代码 + 固定指令/工具
- Context（上下文层）→ 记忆系统

**演进趋势**：
1. 从单一模型到多层系统
2. 从固定到可演进
3. 从通用到个性化
4. Trace 驱动

---

## 2. 持续学习

持续学习指系统在生命周期内不断改进的能力。

**三层路径**：
- 模型层：SFT、RL，成本高，灾难性遗忘
- Harness 层：Meta-Harness，中等成本
- 上下文层：最灵活，最常用，成本最低

**更新粒度**：Agent 级 / 组织级 / 用户级

**更新时机**：离线批处理（"做梦"）或热路径更新

---

## 3. 多 Agent 协作

**同步阻塞式（Hermes）**：
- "总承包商-分包商"模式
- 强隔离，高效，无 Steer

**异步编排式（OpenClaw）**：
- "交响乐团"模式
- 灵活，可控，开销大

**选择原则**：在需要隔离时强隔离，在需要协作时强协作。

---

# 四、综合分析：AI Agent 系统架构全景图

## 核心框架

```
Agent 架构
├── Model 层 → 持续学习 → 灾难性遗忘
├── Harness 层 → 持续学习 → Meta-Harness
└── Context 层 → 持续学习
    ├── 记忆系统
    │   ├── SOUL.md（Agent 级）
    │   ├── 用户级记忆
    │   └── 组织级记忆
    └── Skill 系统
        ├── 轨迹捕获（Hook 机制）
        ├── 自动生成
        └── 自动注入（Auto-recall）

多 Agent 协作
├── 同步阻塞式（Hermes）
│   └── 强隔离、高效、无 Steer
└── 异步编排式（OpenClaw）
    ├── Steer 机制
    ├── 持久化恢复
    └── 外部 Agent（ACP）

Harness Engineering
├── 记忆治理
│   ├── 有界热记忆（Hermes）
│   └── 无界记忆 + Hook（MemOS）
├── 架构约束
│   ├── 主 Agent + 子 Agent（Hermes）
│   └── 多常驻 Agent + 工蜂（MemOS）
└── 评估闭环
    ├── Cron + 消息推送（Hermes）
    └── 心跳 + 审核 + 反思（MemOS）
```

## 选择框架

### 按场景选择

| 场景 | 推荐方案 |
|------|----------|
| 个人 AI 助手 | Hermes-Agent |
| 多 Agent 长期协作 | 自建 OpenClaw Harness（MemOS） |
| 标准化流水线生产 | OMO 模式 |
| 一次性问题解决 | Agent Teams |

### 按时间选择

| 使用时长 | 推荐方案 |
|----------|----------|
| < 3 个月 | Hermes |
| > 6 个月 | 自建 Harness |

### 按需求选择

| 需求 | 选择 |
|------|------|
| 快速启动 | Hermes |
| 极致 token 效率 | Hermes |
| 灵活调整方向 | OpenClaw |
| 无上限记忆 | MemOS |
| 100% 轨迹捕获 | MemOS（Hook） |

---

## 核心引用

> "竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹。每次 Agent 的成功和失败，都是训练下一代的数据。" — Phil Schmid

> "Harness 的核心是轨迹捕获。如果你看不到轨迹，就无法验证 Harness 是否在正常工作。" — servasyy_ai

> "轨迹是所有持续学习流程的核心数据源。" — Harrison Chase

---

## 对 Atlas / WorkBuddy 的启发

**当前架构**：
- Model: 底层 LLM
- Harness: WorkBuddy 核心 + AGENTS.md
- Context: SOUL.md + IDENTITY.md + USER.md + 工作记忆文件

**改进方向**：

**短期（1-2 周）**：
1. 引入 `agent_end` Hook：每次任务结束自动记录
2. 建立简单的去重机制
3. 实现 TODO 一致性检查

**中期（1-2 月）**：
1. 引入 `before_agent_start` Hook：自动注入相关记忆和 Skill
2. 建立 Skill 自动提炼机制
3. 构建可视化界面

**长期（3-6 月）**：
1. 实现多 Agent 协作
2. 建立评估闭环
3. 引入 Steer 能力

---

*导出完成 - 2026-04-12*
