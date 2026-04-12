# Coding Agents 重塑工程、产品和设计

**类型**: article
**日期**: 2026-03-10
**来源**: https://x.com/hwchase17/status/2031051115169808685
**作者**: [[entities/hwchase17|Harrison Chase]]
**标签**: #CodingAgents #EPD #工作流变革

## 摘要

Coding Agents 正在重塑软件公司的 EPD（Engineering, Product, Design）流程。核心变化：实现成本大幅降低，瓶颈从实现转移到评审，PRD 先写再实现的流程已死，但需求文档作为评审伴侣依然重要。角色边界模糊，通才价值提升，每个人都需要产品感。

## 关键要点

### PRD 流程的死亡与重生

**旧流程（PRD → Mock → Code）**：
- 因为实现需要大量时间和专业技能
- 所以需要专业化分工和 PRD 作为沟通基础

**新流程（Idea → Prototype → Review）**：
- Coding Agents 让实现成本接近零
- 任何人都可以快速构建原型
- 瓶颈转移到评审（确保"good"而非"just works"）

**PRD 的新角色**：
- 不再是起点，而是评审的伴侣
- 解释原型中哪些是有意为之，哪些是意外
- 未来 PRD 可能是结构化、版本化的 prompts

### 新的角色分化

**Builder vs Reviewer**：

| 类型 | 能力要求 | 产出 |
|------|----------|------|
| Builder | 产品思维 + Coding Agents + 基础设计直觉 | 小功能从想法到上线，大功能原型 |
| Reviewer | 卓越系统思维 + 快速评审 + 极强沟通 | 确保架构、产品、设计质量 |

**通才价值提升**：
- 一个人能做产品+设计+工程 → 消除沟通开销
- 以前需要协调其他人，现在可以直接与 Agent 沟通
- 影响：一个人比三人团队更快

### 关键能力转变

**从实现到系统思维**：
- 实现成本降低 → 系统思维成为差异化因素
- 工程：服务架构、API、数据库的心智模型
- 产品：用户真正需要什么（不是他们说想要什么）
- 设计：为什么某个设计感觉对

**每个人都需要的核心能力**：
1. 产品感（知道该让 Agent 构建什么）
2. Coding Agents 使用能力
3. 快速评审能力
4. 系统思维

### 专业化门槛提高

**传统专业化已不够**：
- 必须是领域顶尖
- 必须评审速度快
- 必须沟通极强
- 这样的角色在公司里可能不多

**背景不再重要**：
- 优秀的人可能来自产品、设计或工程
- 重要的是：产品直觉 + 技术理解 + 文化感知
- "真正双语的人"——懂技术可能，也懂哪些文化潮流是真实的

## 核心框架

```
EPD 变革
├── 流程变化
│   ├── 旧：PRD → Mock → Code
│   └── 新：Idea → Prototype → Review
├── 瓶颈转移
│   ├── 旧：实现（编码耗时）
│   └── 新：评审（原型泛滥）
├── 角色分化
│   ├── Builder（通才 + Agent）
│   └── Reviewer（专家 + 快速评审）
└── 核心能力
    ├── 产品感（必备）
    ├── 系统思维（差异化）
    └── Coding Agents（工具）
```

## 关联实体

- [[entities/hwchase17|Harrison Chase]] - 作者
- [[entities/skill-system|Skill 系统]] - Builder 需要 Skill 来高效使用 Agent
- [[entities/harness-engineering|Harness Engineering]] - Coding Agents 的底层系统

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]] - 补充 Agent 对工作流的影响

## 实践启发

对庆正的自媒体创作：
- 可以成为 Builder：用 Agent 快速验证内容想法
- 评审能力更重要：确保内容质量而非数量
- 产品感是核心：知道该创作什么内容

对团队管理：
- 识别 Builder 和 Reviewer
- 建立评审流程应对原型泛滥
- 降低专业化门槛，提升产品感要求
