# OpenClaw

**类型**: framework
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 2

## 定义

OpenClaw 是一种多 Agent 协作框架，采用"交响乐团"式的异步编排模式。核心是 subagent 系统，支持事件驱动的异步协作、运行时引导（Steer）、持久化和恢复。追求最大的灵活性和控制力。

## 核心特征

### 角色体系

- **main**：主 agent
- **orchestrator**：编排器，可继续 spawn 子 agent
- **leaf**：叶节点，不可再 spawn

### 并发配置

- 全局最多 8 路并发
- 每 agent 最多管理 5 个活跃子 agent
- 嵌套深度可配置

### 异步事件驱动

- **非阻塞 spawn**：父 agent spawn 子 agent 后立即返回
- **announce 推送**：子 agent 完成后，结果以"user message"形式推送回父 agent
- **避免轮询**：文档明确"不要调用 sessions_list、sessions_history，等待完成事件自动到达"

### Steer 机制

向运行中的子 agent 发送引导消息：
- 限流：每 2 秒最多一次
- 消息最大：4000 字符
- 用途：调整方向、查询进度、提供额外上下文

**对比 Hermes**：Hermes 无法 Steer，用户查询进度会误杀所有子 agent。

### 生命周期管理

- **runTimeoutSeconds**：时间维度超时（默认 300 秒）
- **runs.json**：持久化运行记录，支持 orphan recovery
- **resumeSessionId**：恢复已有会话

### ACP 协议支持

支持调用外部 Agent：
- Claude Code
- Codex
- 其他 ACP 兼容的 Agent

## 优势

- **灵活性高**：可动态调整方向
- **超时控制**：时间 + 迭代双维度
- **用户友好**：查询进度不误杀
- **支持外部 Agent**：ACP 协议
- **持久化**：中断后可恢复

## 劣势

- **Token 开销大**：announce 推送注入上下文，steer 产生额外开销
- **状态管理复杂**：需要处理异步事件、消息推送
- **攻击面大**：子 agent 保留更多能力，隔离性弱于 Hermes

## 与 Hermes 对比

| 维度 | OpenClaw | Hermes |
|------|----------|--------|
| 同步/异步 | 异步事件驱动 | 同步阻塞 |
| 隔离性 | 弱 | 强 |
| Token 效率 | 低 | 高 |
| 灵活性 | 高 | 低 |
| 超时控制 | ✅ | ❌ |
| Steer 能力 | ✅ | ❌ |
| 外部 Agent | ✅ ACP | ❌ |
| 持久化恢复 | ✅ | ❌ |

## SOUL.md 集成

OpenClaw 使用 [[entities/soul-md|SOUL.md]] 作为 Agent 层面的持久记忆：
- 记录 Agent 身份、行为准则、积累经验
- 通过"做梦"（离线分析 trace）更新 SOUL.md
- 参考：[[sources/continual-learning-ai-agents|Continual Learning for AI Agents]]

## 适用场景

- 需要在子 agent 运行中调整方向
- 需要超时控制
- 调用外部编码 agent（Claude Code / Codex）
- 多层嵌套 agent 树
- 超过 3 个 agent 并行
- 长流程任务（需要持久化和恢复）

## 与其他实体的关系

- [[entities/hermes|Hermes]] → 对比框架
- [[entities/steer-mechanism|Steer 机制]] → 核心创新
- [[entities/acp-protocol|ACP 协议]] → 外部 Agent 支持
- [[entities/soul-md|SOUL.md]] → 持久记忆模式
- [[entities/multi-agent-collaboration|多 Agent 协作]] → 应用领域

## 相关来源

- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - 与 Hermes 的详细对比
- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - SOUL.md 和持续学习机制

## 开放问题

- 如何降低 token 开销同时保持灵活性？
- 如何在复杂场景下管理异步状态？
- 与 LangChain Deep Agents 的关系是什么？
