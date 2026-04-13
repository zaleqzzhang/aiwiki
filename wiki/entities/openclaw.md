# OpenClaw

**类型**: framework
**创建日期**: 2026-04-12
**最后更新**: 2026-04-13
**来源数量**: 6

## 定义

OpenClaw 是一个开源的自主 AI Agent 平台，运行在本地基础设施上（Windows/macOS/Linux）。由 Peter Steinberger（PSPDFKit 创始人，奥地利开发者）于 2025 年 11 月创建。2026 年 2 月 Steinberger 加入 OpenAI，OpenClaw 移交独立基金会管理。采用本地优先（local-first）哲学——所有数据、API keys、运行行为留在本地，无云端依赖。MIT 协议开源。

定位为 **"Android for AI agents"**——规模大、第三方生态广、碎片化（345K+ GitHub stars，截至 2026 年 4 月初）。50+ messaging 集成，跨渠道持久化是病毒式增长的核心驱动力。

在多 Agent 协作层面，采用"交响乐团"式的异步编排模式，核心是 subagent 系统，支持事件驱动的异步协作、运行时引导（Steer）、持久化和恢复。

## 核心架构（四层）

### 1. Gateway（通信层）

- WebSocket 服务器，端口 18789（默认）
- 角色声明：operator（操控系统）或 node（暴露设备能力）
- JSON Schema 验证 + 类型化 WebSocket API
- 多平台通道适配器：WhatsApp（Baileys）、Telegram（grammY）、Discord（discord.js）、Slack、Teams、iMessage、Signal

### 2. Sessions/Memory（会话与记忆）

**会话作用域**（dmScope）：
- main —— 全局单会话
- per-channel-peer —— 按通道+发送者分离
- per-account-channel-peer —— 加账户隔离

**记忆系统**（双层管理，源码见 `src/memory/manager.ts`）：
- **MEMORY.md（长期记忆）**：自动注入系统提示词，**截断到 200 行**，不衰减
- **memory/YYYY-MM-DD.md（每日笔记）**：追加写入，随时间衰减
- **写入策略**：显式写入（用户指令 "请记住…"）+ 隐式闪存（Memory Flush：会话结束/新 Session/压缩时自动提炼归档）
- **召回机制**：被动注入（System Prompt 阶段 BM25+向量双路召回）+ 主动搜索（memory_search）+ 深层钻取（memory_get 读取原始文件特定行号）
- **索引存储**：每日 Memory 文件切片→向量化→SQLite 存储分块和索引
- **时间衰减公式**：衰减系数 = e^(-λ × 天数)，λ = ln(2) / 半衰期（默认 30 天）。30 天减半，60 天剩 1/4，90 天剩 1/8
- MEMORY.md 不衰减（"常青"内容），每日笔记随时间降权

与 Hermes 四层记忆对比：OpenClaw 采用"文件即记忆"的扁平模式，无分层策展。但召回机制（BM25+向量）与 Hermes 的 FTS5 路径不同。

### 3. Provider/Model（模型集成）

- 模型无关：Anthropic / OpenAI / Google Gemini / 自定义 OpenAI 兼容端点
- 格式：provider/model
- 多 API key 自动轮转（命中 rate limit 自动切换）

### 4. Plugins/Tools（能力扩展）

- TypeScript 插件通过 jiti 运行时加载
- 插件生命周期：manifest 发现 → 启用验证 → 运行时加载 → 表面消费
- 插件注册：文本推理提供者、通道连接器、Agent 工具
- 工具集中注册表，核心工具和插件工具统一暴露类型化 schema

## Skills 生态

- ClawHub：10,000+ skills（编码、写作、数据分析、DevOps、AI/ML、生产力）
- Skill 结构：目录（SKILL.md + 可选脚本/引用文件）
- 三级加载优先级：Workspace skills > ~/.openclaw/skills > Bundled skills
- Workspace skills 同名覆盖——按项目定制行为

与 Hermes Skills 的关键区别：OpenClaw Skills 偏"安装即用"（打包分发），Hermes Skills 偏"积累即学"（从 trace 提炼）。

### ClawHub 供应链安全危机

- **ClawHavoc 攻击**：Koi Security 审计 2,857 个 Skills，发现 341 个恶意（335 来自同一攻击组织）
- **CVE-2026-25253**（CVSS 8.8）：WebSocket 自动连接暴露认证 Token，一键入侵
- **SecurityScorecard**：数万个公开暴露的 OpenClaw 实例
- **微软**：建议不在个人/企业工作站运行
- **Cisco**：称 OpenClaw 类个人 AI Agent 是"安全噩梦"
- **应对**：与 VirusTotal 合作扫描 Skills，发布安全指南。信任模型仍在演进中

## System Prompt 动态组装

源码位于 `src/agents/system-prompt.ts`，核心函数 `buildAgentSystemPrompt()`。

**23 个模块按固定顺序拼装**（核心模块摘要）：

| # | 模块 | 加载条件 | 作用 |
|---|------|---------|------|
| 1 | 身份标识 | 永远存在 | "You are OpenClaw, a personal AI assistant." |
| 2 | 工具清单 | full/minimal | 列出可用工具，工具名区分大小写 |
| 3 | 工具调用风格 | full | 简单任务直接干，复杂任务先告知用户 |
| 4 | 安全准则 | full | 不追求自我保护、不越权、不泄露隐私 |
| 6 | Skills 条件加载 | full + 有技能时 | 先扫描列表，判断后才加载 SKILL.md |
| 7 | 记忆召回 | full + 有记忆工具 | 回答"之前做过什么"前必须先搜索记忆 |
| 15 | Workspace 文件注入 | full/minimal | 直接注入 AGENTS.md/SOUL.md/USER.md 等 |
| 22 | 心跳机制 | full | 定期唤醒，`HEARTBEAT_OK` 或执行巡检 |
| 23 | 运行时信息 | 永远存在 | agentId、host、os、model、shell、channel |

**三种 PromptMode**：
- `full`：主 Agent 与用户直接对话，所有模块加载
- `minimal`：子 Agent 执行独立任务，只保留核心模块
- `none`：极简，只有身份标识

**Markdown 驱动的文件注入**（8 个核心文件）：
AGENTS.md（骨架/总纲）→ SOUL.md（灵魂/人格）→ IDENTITY.md（身份证）→ USER.md（主人档案）→ TOOLS.md（工具清单）→ HEARTBEAT.md（心跳任务）→ BOOTSTRAP.md（出生证明，首次启动后自删）→ BOOT.md（启动文件，配合 Hook）

**极简主义措辞哲学**："Quality > quantity" 一句话传达"注重核心信息、拒绝废话"的复杂指令。节省 Context Window 空间。

## 上下文管理（Compaction & Pruning）

### 上下文压缩（Compaction）

源码位于 `src/agents/compaction.ts`。

**触发方式**：
- 手动：`/compact` 命令，可指定保留关键信息
- 自动：token 用量 > 上下文窗口 - 预留空间（如 20 万窗口，预留 2 万，>18 万时触发）

**分块参数**：
- `BASE_CHUNK_RATIO = 0.4`（每块占上下文 40%）
- `MIN_CHUNK_RATIO = 0.15`（每块最小 15%）
- `SAFETY_MARGIN = 1.2`（20% 安全缓冲）
- `SUMMARIZATION_OVERHEAD_TOKENS = 4096`（Summary 指令预留）

**三级摘要策略**：
1. `summarizeInStages()`：顶层策略，判断消息量后分发
2. `summarizeChunks()`：分块处理，支持最多 3 次重试
3. `summarizeWithFallback()`：兜底，排除过大消息后重试，最终返回 "No prior history."

**保留项**：活跃任务、重要决策、TODO、承诺、不透明标识符（UUID/哈希必须原文保留）

**保护机制**：超时 5 分钟（`EMBEDDED_COMPACTION_TIMEOUT_MS = 300000`）、写锁防并发、可配置压缩模型

### 精细化修剪（Pruning）

源码位于 `src/agents/pi-embedded-runner/tool-result-truncation.ts`。

- **头尾保留，中间省略**：错误信息通常在首尾，JSON 核心定义在头部
- **裁剪上限**：不超过 50%
- **KV Cache 窗口优化**：Cache 过期后主动剔除旧片段，提升推理 Latency

## Harness Engineering 实践

虽然 OpenClaw 没有显式宣称"Harness 框架"，但其底层架构处处体现 Harness Engineering 精髓。

### 7 种 Hook 钩子（全生命周期）

| 钩子名称 | 触发时机 | 典型用途 |
|---------|---------|---------|
| `before_prompt_build` | 构建提示词之前 | 注入额外上下文 |
| `before_tool_call` | 执行工具之前 | 拦截或修改工具参数 |
| `after_tool_call` | 工具执行之后 | 处理工具结果 |
| `before_compaction` | 上下文压缩之前 | 观察或标注 |
| `after_compaction` | 上下文压缩之后 | 后处理 |
| `message_received` | 收到消息时 | 消息预处理 |
| `message_sending` | 发送消息前 | 消息后处理 |

实战示例：`before_tool_call` Hook 做参数校验——拦截错误的实例 ID 格式，返回"参数错误"迫使模型修正，一次配置统一校验。

### 三层安全沙箱（纵深防御）

1. **文件系统沙箱**：限制 Workspace 访问范围，越界读写直接阻断
2. **命令执行沙箱**：Security 模式（白名单）+ Ask 模式（暂停等人确认）+ safeBins 豁免
3. **网络访问沙箱**：白名单域名 + 防泄露机制

底层依托 OS 最小权限兜底，安全机制解耦为独立进程插件。

### Human-in-the-Loop

高风险操作暂停执行等人确认。"写类型"操作几乎都需人工审批。

### Harness vs Workflow

| 维度 | Workflow | Harness |
|------|----------|---------|
| 路径 | 硬编码线性（A→B→C） | 动态软约束 |
| 模型角色 | 节点/执行者 | 自主规划+迭代 |
| 主导权 | 人 | AI（在约束内） |
| 灵活性 | 低 | 高 |
| 异常处理 | 容易断裂 | 自动反思重试 |

核心区别：**主导权归属**。模型能力越强，Harness 越优于 Workflow。

## 多 Agent 协作特征

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

- [[sources/openclaw-vs-hermes-race|OpenClaw vs Hermes: The Race]] - The New Stack 深度对比，"生态优先 vs 学习优先"，安全危机详情
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - 与 Hermes 的详细对比
- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - SOUL.md 和持续学习机制
- [[sources/openclaw-beginners-guide|How Does OpenClaw Work?]] - 四层架构入门指南、Skills 生态、记忆系统
- [[sources/openclaw-prompt-context-harness|深度解析 OpenClaw 在 Prompt/Context/Harness 三维度中的设计哲学]] - 源码级分析：23 模块 Prompt 组装、压缩算法、Hook 系统、三层沙箱

## 开放问题

- 如何降低 token 开销同时保持灵活性？（Compaction + Pruning 已部分解答，但仍有优化空间）
- 如何在复杂场景下管理异步状态？
- Skills 安全问题如何在"能力无限"与"运行安全"之间找平衡？（已引入 ClawHub 鉴权，但生态安全仍在演进）
- 时间衰减公式（半衰期 30 天）是否适合所有场景？是否需要按 Memory 类型差异化？
- Hook 系统的粒度是否足够？是否需要更细粒度的约束（如 per-tool Hook）？
