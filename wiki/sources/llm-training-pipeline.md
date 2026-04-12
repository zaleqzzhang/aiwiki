# 大模型训练：原理、路径与新实践

**类型**: article
**日期**: 2026-04-03
**来源**: https://x.com/HiTw93/status/2040047268221608281
**作者**: [[entities/HiTw93|@HiTw93]]
**标签**: #大模型训练 #后训练 #RLHF #Agent训练

## 摘要

2026 年大模型效果拉开差距的关键不再是预训练本身，而是后训练、评测、奖励、Agent 训练、蒸馏这一整套训练链路。文章详细拆解了从预训练到 Agent 训练的完整流水线，强调训练后半段才是用户感知提升的真正来源。提出了 "竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹" 这一核心论断。

## 关键要点

### 预训练是底座
- 预训练决定知识范围、泛化潜力和模式归纳能力
- 规模定律是预算分配工具，非抽象曲线
- Llama3 8B 用 15T tokens（Chinchilla 最优点的 75 倍），换取更高能力密度
- Tokenizer 设计直接影响下游性能和多语言能力

### 数据配方决定模型能力
- 数据工程是完整的数据生产工程，不只是清洗
- 去重和污染控制对结果影响很大
- 合成数据已成正式训练流程一部分
- 每代更强的模型都参与重构下一代模型的数据

### 后训练决定用户感知
- 后训练是多阶段流水线（DeepSeek-R1 四阶段：冷启动 SFT → RL → 拒绝采样微调 → 对齐）
- GRPO 比 PPO 更适合大模型 RL（不需要独立价值网络）
- 后训练不只改变能力，也改变风格

### Eval/Grader/Reward 重新定义训练目标
- Eval 决定测什么，Grader 决定怎么打分，Reward 决定优化方向
- Verified rewards 用程序验证正确性，不再依赖人工偏好
- Reward hacking 和 alignment faking 是新风险

### Agent 训练优化 Harness 本身
- Agent 训练不只是训练模型，也训练 Harness
- Meta-Harness 优化的是 Harness code 本身，可能拉出 6x 性能差距
- 环境质量是 Agent 训练的核心（稳定性、真实性、覆盖度、抗利用性）

## 关键洞察

> "竞争优势不再是 prompt，而是你的 Harness 捕获的轨迹。每次 Agent 的成功和失败，都是训练下一代的数据。" — Phil Schmid

**训练链路**：
```
预训练 → 后训练（SFT/RLHF/DPO/RFT）→ 蒸馏 → 专用化 → 发布
    ↓                                           ↓
数据配方                                    Checkpoint 选择
    ↓                                           ↓
系统约束                                 生产流量回流
```

**Meta-Harness 发现**：同一个底模，只改 Harness，可能拉出 6x 性能差距。

## 关联实体

- [[entities/harness-engineering|Harness Engineering]] - 文章明确将 Harness Engineering 作为独立优化对象
- [[entities/meta-harness|Meta-Harness]] - 被 Meta-Harness 论文深度引用
- [[entities/HiTw93|@HiTw93]] - 作者，大模型训练链路研究者
- [[entities/hwchase17|Harrison Chase]] - 文章引用 Phil Schmid 关于轨迹的观点

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]] - 补充训练链路视角
- [[topics/continual-learning|持续学习]] - 提供模型层学习的完整路径

## 扩展阅读

1. DeepSeek-R1 技术报告（四阶段训练流水线）
2. Meta-Harness 论文（Harness code 优化）
3. Kimi K2.5 Tech Blog（PARL 并行拆解）
4. Cursor Composer 2 Technical Report（real-time RL）
