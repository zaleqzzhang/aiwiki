# 给你的 Agent 搭操作系统（实战篇）：Hermes-Agent vs 自建 MemOS

**类型**: article
**作者**: [[entities/servasyy-ai|servasyy_ai]]
**日期**: 2026-04-10
**来源**: https://x.com/servasyy_ai/status/2042473092036116847
**标签**: #harness-engineering #memory #skill #evaluation

## 摘要

本文基于 [[entities/harness-engineering|Harness Engineering]] 三根支柱（评估闭环、架构约束、记忆治理），深度对比两种实现路径：Hermes-Agent（有界热记忆 + 主动进化）和自建 MemOS（无界记忆 + 系统强制）。两种路径都在践行 Harness Engineering，但设计哲学截然不同。

## 关键要点

### Harness Engineering 三根支柱

1. **记忆治理** → 解决 Agent 失忆症
2. **架构约束** → 明确 Agent 职责边界
3. **评估闭环** → 保证 Agent 质量

### 两种设计哲学

**Hermes-Agent**：
- 有界热记忆 + 多层检索 + 主动进化
- Agent 自己决定什么值得记
- CLI 优先，个人助手定位
- 快速启动，低成本

**自建 MemOS**：
- 无界记忆 + 自动进化 + 系统强制
- Hook 机制自动捕获轨迹，不依赖 Agent 自觉
- Web 可视化，团队协作定位
- 搭建成本高，长期 ROI 高

### 记忆治理对比

**Hermes 三层记忆架构**：

```
L1 热记忆：MEMORY.md（2200 字符上限）+ USER.md（1375 字符）
    ↓ Agent 主动管理（memory add/replace/remove）
L2 外部 Provider：Honcho/Mem0 等（8+ 个插件）
    ↓ 语义搜索
L3 冷检索：Session History（SQLite FTS5）
    ↓ 全文搜索
```

策略：**热记忆精炼 + 冷检索兜底**

**MemOS 无界记忆**：

```
PostgreSQL 17 + pgvector
├── memories 表（35,000+ 条）
├── skills 表（几百个）
├── task_skills 表（关联）
└── memory_graph 表（65,524 条关系边）
```

**Hook 机制实现 100% 捕获**：
- `before_agent_start`：启动时自动注入相关记忆
- `agent_end`：结束时自动捕获对话

**智能去重三层机制**：
1. 内容哈希精确去重
2. Top-5 相似度检测（阈值 0.75）
3. LLM 最终裁决（DUPLICATE/UPDATE/NEW）

**混合搜索**：BM25 + 向量 + RRF 融合

**两级共享架构**：
- Local Scope：同实例内零拷贝共享（scope=private/shared/public）
- Hub-Client：跨实例主动推送，私有数据不离开本地

### 架构约束对比

**Hermes**：主 Agent + 子 Agent 并行
- 子 Agent 独立执行（isolated subagents）
- 记忆继承主 Agent 快照
- Skills 通过 skill_manage 创建，后台周期性 review
- Progressive disclosure（启动时只注入索引）

**MemOS**：多常驻 Agent + 工蜂系统
- 4 个常驻 Agent：智库、黄家 1 号、技术顾问、创意伙伴
- 工蜂按需 spawn：写作工蜂、配图工蜂、爬虫工蜂
- DNA 级共享：AGENTS.md 自动注入所有工蜂
- 临终反哺 → 升级为 MemOS Skill 系统

**MemOS Skill 自动生成四步流程**：
1. 规则过滤（chunks 数量 + 非平凡内容）
2. 混合检索（FTS + Vector + RRF）
3. LLM 决策（confidence ≥ 0.7 升级，< 0.3 新建）
4. 质量评分（≥ 6 分才写入）

**Auto-recall**：
- `before_agent_start` Hook 自动检索并注入相关 Skill
- Agent 想不看都不行

### 评估闭环对比

**Hermes**：Cron 调度 + 消息推送
- 每小时检查系统健康
- 每天生成活动摘要
- 异常推送到 Telegram/Discord/Slack

**MemOS**：多层评估机制
1. **心跳检查**：正常不说话，异常才告警
2. **TODO 一致性检查**：扫描 ⏳ 状态，对比记忆修正
3. **智库审核**：工蜂完成后智库审核（证据、配方、质量）
4. **反思系统**：延迟分析失败记录，批量分析根因

**延迟分析 vs 实时反思**：
- 成本低 10 倍
- 不阻塞主流程
- 能发现跨 session 的系统性模式

### 成本对比

| 维度 | Hermes-Agent | 自建 MemOS |
|------|-------------|-----------|
| 安装时间 | 10 分钟 | 1-2 周 |
| 配置时间 | 30 分钟 | 1-2 周 |
| 上手时间 | 1 小时 | 2-4 周 |
| 维护成本 | 低 | 中 |
| API 成本 | $10-50/月 | $200-300/月（4 Agent） |
| 适合时长 | < 3 个月 | > 6 个月 |

### 适用场景

**选 Hermes-Agent**：
- 个人 AI 助手
- 需要快速启动
- CLI 优先，不需要可视化
- 使用社区 Skill（Skills Hub）
- 所有子 Agent 共享记忆即可

**自建 MemOS**：
- 多 Agent 长期协作
- 需要无上限记忆 + 权限隔离
- 需要 100% 轨迹捕获
- 需要可视化管理
- 需要看 Skill 进化轨迹

**其他场景**：
- 标准化流水线生产 → OMO 模式（预定义角色 + 路由分发）
- 一次性问题解决 → Agent Teams（Claude Code 辩论室）

### 实施建议：分阶段实施

**第一阶段**：搭 MemOS，跑起来
- PostgreSQL + pgvector（或 SQLite + FTS5）
- before_agent_start Hook（注入记忆）
- agent_end Hook（捕获记忆）
- 简单去重机制

**第二阶段**：定义角色分工，建立配方库
- 明确常驻 Agent 和工蜂
- 编写 AGENTS.md、recipes/、bees/
- 通过 System Prompt 注入

**第三阶段**：加入评估机制
- 心跳检查
- Agent 互评
- TODO 一致性检查

**一个月后**：开始看到轨迹积累的效果

**关键原则**：先跑起来，再优化。

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 三根支柱框架
- [[entities/hermes|Hermes]] - 对比的框架之一
- [[entities/openclaw|OpenClaw]] - MemOS 的基础框架
- [[entities/memos|MemOS]] - 自建的 Harness 系统
- [[entities/hook-mechanism|Hook 机制]] - 100% 捕获的关键
- [[entities/auto-recall|Auto-recall]] - 自动注入 Skill 的机制
- [[entities/skill-system|Skill 系统]] - 经验提炼和复用
- [[entities/progressive-disclosure|Progressive Disclosure]] - 节省 token 的技巧

## 引用此来源的页面

*待其他页面创建后补充*

## 相关主题

- [[topics/harness-engineering|Harness Engineering]]
- [[topics/memory-systems|记忆系统]]
- [[topics/multi-agent-collaboration|多 Agent 协作]]
- [[topics/agent-architecture|Agent 架构]]

## 核心引用

> "竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹。每次 Agent 的成功和失败，都是训练下一代的数据。" — Phil Schmid
