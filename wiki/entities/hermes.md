# Hermes

**类型**: framework
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 定义

Hermes 是一种多 Agent 协作框架，采用"总承包商-分包商"模式。核心机制是 `delegate_task` 工具，支持同步阻塞式任务委派。追求极致的隔离性和 token 效率。

## 核心特征

### 架构设计

- **同步阻塞**：父 agent 等待所有子 agent 完成后返回
- **强隔离**：子 agent 中间推理不污染父 agent 上下文，只返回 summary
- **并行执行**：ThreadPoolExecutor，最多 3 个任务并行
- **深度限制**：MAX_DEPTH = 2（父 → 子，不可再嵌套）

### 子 Agent 构造

每次委派创建全新的 AIAgent 实例：
- 独立的对话历史
- 独立的 Terminal session（独立工作目录和状态）
- 专门构建的 System Prompt

**工具剥夺**：
- `delegate_task`：禁用（防止递归委派）
- `clarify`：移除（无法与用户交互）
- `memory`：剥夺（保护共享记忆）
- `execute_code`：禁用（避免复杂脚本）
- `send_message`：移除（防止跨平台消息）

### 生命周期控制

- 迭代上限：默认 50 轮（可通过 config.yaml 配置）
- **无超时机制**：future.result() 无 timeout，子 agent 可无限运行
- 凭证管理：三级优先级解析，自动释放 lease

### 通信模式

纯粹单向返回（Request-Response）：
- 无语义通信
- 只有 UI 层面的进度显示（CLI spinner / Gateway）

## 优势

- **Token 效率极高**：上下文零膨胀
- **天然沙箱**：强隔离适合安全敏感场景
- **实现简洁**：单工具调用完成委派
- **并行能力**：3 路并行适合批量独立任务

## 劣势

- **无超时**：可能无限运行（只受迭代次数限制）
- **无 Steer**：无法向运行中的子 agent 发送引导
- **并发上限硬编码**：MAX_CONCURRENT_CHILDREN = 3
- **误杀问题**：用户查询进度会触发 Gateway interrupt，杀死所有子 agent
- **无持久化**：中断后无法恢复

## 与 OpenClaw 对比

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 同步/异步 | 同步阻塞 | 异步事件驱动 |
| 隔离性 | 强 | 弱 |
| Token 效率 | 高 | 低 |
| 灵活性 | 低 | 高 |
| 超时控制 | ❌ | ✅ |
| Steer 能力 | ❌ | ✅ |

## 改进方向

**高优先级**：
1. 加入超时机制：`timeout_seconds` 参数
2. 改进 Gateway interrupt 逻辑：区分查询进度 vs 真正中断

**中优先级**：
1. 引入 Steer 能力
2. 放开并发上限（可配置）
3. 支持持久化和恢复

**长期**：
1. 支持外部 Agent（ACP 协议）

## 适用场景

- 并行处理 3 个互不相关的任务
- 需要极致 token 效率
- 需要强隔离（安全敏感）
- 快速集成、简单决策-执行流程

## 与其他实体的关系

- [[entities/openclaw|OpenClaw]] → 对比框架
- [[entities/delegate-task|delegate_task]] → 核心机制
- [[entities/multi-agent-collaboration|多 Agent 协作]] → 应用领域

## 相关来源

- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - 详细对比分析

## 开放问题

- 如何在保持隔离性的同时引入超时控制？
- 是否可以借鉴 OpenClaw 的 Steer 机制而不牺牲 token 效率？
- 如何支持外部 Agent 调用？
