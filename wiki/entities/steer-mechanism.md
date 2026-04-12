# Steer 机制

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 定义

Steer 机制是一种运行时引导能力：父 agent 可以向正在运行中的子 agent 发送消息，调整其方向、提供额外上下文、或查询进度。这是 OpenClaw 相对于 Hermes 的核心优势之一。

## 核心特征

- **非中断式**：不杀死子 agent，而是发送引导消息
- **限流保护**：每 2 秒最多一次
- **消息上限**：最大 4000 字符
- **异步场景**：适用于异步编排的多 agent 系统

## 对比：有 Steer vs 无 Steer

| 场景 | Hermes（无 Steer） | OpenClaw（有 Steer） |
|------|-------------------|---------------------|
| 用户查询进度 | 触发 Gateway interrupt，误杀所有子 agent | 返回进度，子 agent 继续运行 |
| 调整方向 | 无法调整，必须中断重启 | 发送 Steer 消息，实时调整 |
| 提供新信息 | 无法提供，必须重新开始 | 随时注入新上下文 |

## 使用场景

**查询进度**
```
用户: "?"
OpenClaw: 向子 agent 发送 Steer 消息查询状态 → 返回进度 → 子 agent 继续运行
Hermes: 触发 interrupt → 杀死所有子 agent
```

**调整方向**
```
用户: "换成方案 B"
OpenClaw: 向子 agent 发送 Steer 消息 → 子 agent 调整策略
Hermes: 必须中断 → 重新开始任务
```

**注入上下文**
```
父 agent 发现新信息 → 向子 agent 发送 Steer → 子 agent 获得新上下文继续工作
```

## 设计考量

**限流的必要性**：
- 防止父 agent 过度干预子 agent
- 避免 token 爆炸
- 保持子 agent 的自主性

**消息长度限制**：
- 4000 字符上限
- 防止上下文膨胀
- 保持消息简洁聚焦

## 与其他概念的关系

- [[entities/openclaw|OpenClaw]] → Steer 机制的实现者
- [[entities/hermes|Hermes]] → 缺失 Steer 能力，需改进
- [[entities/multi-agent-collaboration|多 Agent 协作]] → Steer 是协作的关键能力

## 相关来源

- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - Steer 机制的详细说明

## 实践启发

对于多 Agent 系统，Steer 能力是区分"僵化执行"和"灵活协作"的关键。

Hermes 改进方向：引入 Steer 机制，同时保持 token 效率。

## 开放问题

- Steer 消息应该包含哪些内容？如何设计消息格式？
- 如何平衡 Steer 频率和子 agent 自主性？
- Steer 是否应该支持子 agent 向父 agent 发送（反向引导）？
