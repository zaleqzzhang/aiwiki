# AI Agent 系统架构全景图

**类型**: synthesis
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 3

## 概述

本文档综合当前知识库的三篇核心文章，构建 AI Agent 系统的完整架构视图。从三层架构到持续学习，从多 Agent 协作到 Harness Engineering，形成一套完整的方法论体系。

---

## 一、Agent 架构：三层模型

### 核心框架

```
Model（模型层）
├── 定义：模型权重本身
├── 更新方式：SFT、RL（如 GRPO）
└── 挑战：[[entities/catastrophic-forgetting|灾难性遗忘]]

Harness（Harness 层）
├── 定义：驱动 Agent 的代码 + 固定指令/工具
├── 更新方式：[[entities/meta-harness|Meta-Harness]]
└── 特点：通常在 Agent 整体层面更新

Context（上下文层）
├── 定义：Harness 外的可配置内容（指令、技能、工具）
├── 别名："记忆"
├── 更新方式：离线批处理 或 热路径更新
└── 特点：最灵活，支持多粒度
```

**关键洞察**：记忆系统是 Context 层的核心功能，承载 Agent 的持久知识、身份定义、用户偏好。

**参考**：[[sources/continual-learning-ai-agents|Continual Learning for AI Agents]]

---

## 二、持续学习：三层路径

### 模型层持续学习

| 维度 | 说明 |
|------|------|
| 技术手段 | SFT、RL（GRPO） |
| 粒度 | 通常 Agent 级 |
| 挑战 | 灾难性遗忘 |
| 成本 | 高 |

**2026 训练链路**（根据 [[sources/llm-training-pipeline|大模型训练：原理、路径与新实践]]）：

```
预训练（底座）
    ↓
后训练（SFT → RLHF/DPO/RFT → 拒绝采样微调 → 对齐）
    ↓
蒸馏（大模型轨迹 → 小模型）
    ↓
专用化（TranslateGemma 等）
    ↓
发布（Checkpoint 选择）
    ↓
生产流量回流（Cursor Composer 2 的 real-time RL）
```

**关键洞察**：
- 2026 年大模型差距来自预训练之后的训练链路
- DeepSeek-R1 四阶段：冷启动 SFT → RL（GRPO）→ 拒绝采样微调 → 对齐
- Meta-Harness：同一个底模，只改 Harness，可能拉出 6x 性能差距

### Harness 层持续学习

| 维度 | 说明 |
|------|------|
| 技术手段 | Meta-Harness |
| 流程 | 运行任务 → 评估 → 存储 trace → 编码 Agent 分析 → 修改 Harness |
| 粒度 | 通常 Agent 级 |
| 成本 | 中 |

### 上下文层持续学习

| 维度 | 说明 |
|------|------|
| 技术手段 | 离线批处理（"做梦"）或热路径更新 |
| 粒度 | Agent / 用户 / 组织 级 |
| 成本 | 低 |
| 特点 | 最灵活，最常用 |

**更新粒度对比**：

| 粒度 | 说明 | 案例 |
|------|------|------|
| Agent 级 | Agent 自己的持久记忆 | OpenClaw 的 [[entities/soul-md|SOUL.md]] |
| 组织级 | 团队/公司级别的定制 | Sierra Explorer |
| 用户级 | 个人级别的记忆和偏好 | Hex Context Studio |

**参考**：[[sources/continual-learning-ai-agents|Continual Learning for AI Agents]]

---

## 三、多 Agent 协作：两种模式

### 同步阻塞式（Hermes 模式）

```
父 agent（项目经理）
    ├── 拆解任务
    ├── 分发给子 agent
    └── 阻塞等待结果返回
        ├── 子 agent A（独立执行，中间过程隔离）
        ├── 子 agent B（独立执行，中间过程隔离）
        └── 子 agent C（独立执行，中间过程隔离）
```

**特点**：
- 强隔离：子 agent 中间推理不污染父 agent 上下文
- 高效并行：最多 3 个任务并行
- Token 节省：只返回 summary
- 缺乏灵活性：无 [[entities/steer-mechanism|Steer]] 能力

### 异步编排式（OpenClaw 模式）

```
父 agent（指挥家）
    ├── 定义全局拓扑
    └── 异步运行，持续监听
        ├── agent A（固定角色，事件驱动）
        ├── agent B（固定角色，事件驱动）
        └── agent C（固定角色，事件驱动）
            ↓
        announce 推送结果
            ↓
        父 agent 接收并处理
            ↓
        可选：发送 Steer 调整方向
```

**特点**：
- 异步事件驱动：spawn 后立即返回
- Steer 能力：可向运行中的子 agent 发送引导
- 持久化恢复：支持中断后恢复
- Token 开销大：announce 和 steer 产生额外开销

### 对比总结

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 同步/异步 | 同步阻塞 | 异步事件驱动 |
| 隔离性 | 强 | 弱 |
| Token 效率 | 高 | 低 |
| 灵活性 | 低 | 高 |
| 超时控制 | ❌ | ✅ |
| Steer 能力 | ❌ | ✅ |
| 持久化恢复 | ❌ | ✅ |
| 外部 Agent | ❌ | ✅ ACP |

**选择原则**：在需要隔离时强隔离，在需要协作时强协作。

**参考**：[[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]]

---

## 四、Harness Engineering：三根支柱

### 支柱一：记忆治理

**问题**：Agent 失忆症——每次对话都要重新解释背景。

**解决**：给 Agent 搭持久化记忆系统。

**两种路径**：

| 维度 | Hermes | MemOS |
|------|--------|-------|
| 记忆架构 | 三层：L1 热记忆 + L2 外部 Provider + L3 冷检索 | 无界：PostgreSQL + pgvector |
| 管理方式 | Agent 主动管理 | 系统 Hook 强制捕获 |
| 捕获率 | 依赖 Agent 自觉（~60%） | Hook 强制（~100%） |
| 容量 | 固定上限（3.5KB） | 无上限（35,000+ 条） |
| 检索方式 | FTS5 全文搜索 | BM25 + 向量 + RRF 融合 |

**Hook 机制**：
- `before_agent_start`：启动时自动注入相关记忆
- `agent_end`：结束时自动捕获对话

**智能去重三层机制**：
1. 内容哈希精确去重
2. Top-5 相似度检测（阈值 0.75）
3. LLM 最终裁决（DUPLICATE/UPDATE/NEW）

### 支柱二：架构约束

**问题**：单个 Agent 什么都干，职责混乱。

**解决**：用架构约束明确职责边界。

**两种路径**：

| 维度 | Hermes | MemOS |
|------|--------|-------|
| 架构模式 | 主 Agent + 子 Agent 并行 | 多常驻 Agent + 工蜂系统 |
| Agent 生命周期 | 子 Agent 用完即毁 | 常驻 Agent 长期运行 + 工蜂按需 spawn |
| 记忆继承 | 子 Agent 继承主 Agent 快照 | DNA 级共享（AGENTS.md） |
| 权限隔离 | 无 | 有（scope=private/shared/public） |
| Skill 系统 | Agent 主动创建 + 后台 review | 系统自动生成 + 质量评分 |

**MemOS 工蜂系统**：
- 4 个常驻 Agent：智库、黄家 1 号、技术顾问、创意伙伴
- 工蜂按需 spawn：写作工蜂、配图工蜂、爬虫工蜂
- 临终反哺：工蜂完成后经验沉淀

**Skill 自动生成四步流程**：
1. 规则过滤（chunks 数量 + 非平凡内容）
2. 混合检索（FTS + Vector + RRF）
3. LLM 决策（confidence ≥ 0.7 升级，< 0.3 新建）
4. 质量评分（≥ 6 分才写入）

**Auto-recall**：
- `before_agent_start` Hook 自动检索并注入相关 Skill
- 效果：前一只工蜂的失败，变成后一只工蜂的本能

### 支柱三：评估闭环

**问题**：Agent 做完了任务，但不知道做得对不对。

**解决**：建立评估闭环，让 Agent 能自检、能从失败中学习。

**两种路径**：

| 维度 | Hermes | MemOS |
|------|--------|-------|
| 评估方式 | Cron 调度 + 消息推送 | 心跳检查 + 智库审核 + 反思系统 |
| 质量控制 | 无明确机制 | 智库审核（证据、配方、质量） |
| 失败反思 | 无 | 延迟分析，批量处理 |
| 告警方式 | Telegram/Discord/Slack | Discord |

**MemOS 多层评估**：
1. 心跳检查：正常不说话，异常才告警
2. TODO 一致性检查：扫描 ⏳ 状态，对比记忆修正
3. 智库审核：工蜂完成后智库审核
4. 反思系统：延迟分析失败记录

**延迟分析 vs 实时反思**：
- 成本低 10 倍
- 不阻塞主流程
- 能发现跨 session 的系统性模式

**参考**：[[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]]

---

## 五、核心概念关系图

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

---

## 六、选择框架

### 按场景选择

| 场景 | 推荐方案 |
|------|----------|
| 个人 AI 助手 | Hermes-Agent |
| 多 Agent 长期协作 | 自建 OpenClaw Harness（MemOS） |
| 标准化流水线生产 | OMO 模式（预定义角色 + 路由分发） |
| 一次性问题解决 | Agent Teams（Claude Code 辩论室） |

### 按时间选择

| 使用时长 | 推荐方案 |
|----------|----------|
| < 3 个月 | Hermes（快速启动，低成本） |
| > 6 个月 | 自建 Harness（长期 ROI 高） |

### 按需求选择

| 需求 | 选择 |
|------|------|
| 快速启动 | Hermes |
| 极致 token 效率 | Hermes |
| 强隔离（安全敏感） | Hermes |
| CLI 优先 | Hermes |
| 社区 Skill | Hermes（Skills Hub） |
| 灵活调整方向 | OpenClaw |
| 超时控制 | OpenClaw |
| 调用外部 Agent | OpenClaw（ACP） |
| 多层嵌套 agent 树 | OpenClaw |
| 长流程任务 | OpenClaw（持久化恢复） |
| 无上限记忆 | MemOS |
| 100% 轨迹捕获 | MemOS（Hook） |
| 可视化管理 | MemOS |
| Skill 进化轨迹 | MemOS |

---

## 七、核心引用

> "竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹。每次 Agent 的成功和失败，都是训练下一代的数据。" — Phil Schmid

> "Harness 的核心是轨迹捕获。如果你看不到轨迹，就无法验证 Harness 是否在正常工作。" — servasyy_ai

> "轨迹是所有持续学习流程的核心数据源。" — Harrison Chase

---

## 八、对 Atlas / WorkBuddy 的启发

### 当前架构

```
Model: 底层 LLM（GLM-5.0）
Harness: WorkBuddy 核心 + AGENTS.md
Context: SOUL.md + IDENTITY.md + USER.md + 工作记忆文件
```

### 可改进方向

**短期（1-2 周）**：
1. 引入 `agent_end` Hook：每次任务结束自动记录到工作记忆文件
2. 建立简单的去重机制：内容哈希 + 相似度检测
3. 实现 TODO 一致性检查：防止状态漂移

**中期（1-2 月）**：
1. 引入 `before_agent_start` Hook：自动注入相关记忆和 Skill
2. 建立 Skill 自动提炼机制：从工作记忆中提炼可复用经验
3. 构建可视化界面：记忆浏览、Skill 管理

**长期（3-6 月）**：
1. 实现多 Agent 协作：根据任务特性选择同步阻塞或异步编排
2. 建立评估闭环：心跳检查 + 质量审核 + 反思系统
3. 引入 Steer 能力：支持运行时调整方向

---

## 相关来源

- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]]
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]]
- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]]
