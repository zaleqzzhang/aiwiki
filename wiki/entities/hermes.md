# Hermes

**类型**: framework
**创建日期**: 2026-04-12
**最后更新**: 2026-04-13
**来源数量**: 3

## 定义

Hermes 是由 Nous Research 开发的开源 AI Agent，核心特点是**自我改进闭环**。不同于 OpenClaw 的多 Agent 协作，Hermes 是单 Agent 架构，通过使用积累知识和技能。Hermes 同时支持多 Agent 协作模式（delegate_task），但其核心创新是学习循环。

## 核心架构

### 学习循环四要素

Hermes 的独特之处在于将"发生了什么"升级为"什么有效"：

1. **Periodic Nudge（定期提示）**
   - 会话中间自动触发
   - Agent 回顾并策展记忆
   - 主动决定保留什么

2. **Autonomous Skill Creation（自主技能创建）**
   - 触发条件：5+ 工具调用、错误恢复、用户纠正、非显而易见工作流
   - 输出：`~/.hermes/skills/` 下的 SKILL.md
   - 格式：agentskills.io 开放标准

3. **Skill Self-Improvement（技能自我优化）**
   - 六种操作：create, patch, edit, delete, write_file, remove_file
   - **patch > edit**：避免破坏已工作内容，token 效率更高

4. **FTS5 Session Search（会话检索）**
   - SQLite 存储全量会话
   - FTS5 全文索引
   - LLM 摘要后注入上下文

### 四层记忆架构

```
┌─────────────────────────────────────────────┐
│  Honcho Layer（可选）                        │
│  用户建模：偏好、风格、领域知识              │
│  12 个身份层，被动构建                       │
├─────────────────────────────────────────────┤
│  Skills（渐进式加载）                        │
│  程序性记忆：如何做                          │
│  仅加载名称+摘要，按需展开                   │
├─────────────────────────────────────────────┤
│  Session Search（按需检索）                  │
│  情景记忆：发生了什么                        │
│  SQLite + FTS5 + LLM 摘要                   │
├─────────────────────────────────────────────┤
│  Prompt Memory（始终在线）                   │
│  MEMORY.md + USER.md                        │
│  3,575 字符限制，强制策展                    │
└─────────────────────────────────────────────┘
```

**分层原则**：
- 重要性 → Prompt Memory（每个会话都需要）
- 相关性 → Session Search（特定主题触发时检索）
- 程序性 → Skills（任务执行时加载）
- 用户特性 → Honcho（长期个人助理场景）

## Profile 隔离机制

Hermes 除了 delegate_task 临时子 Agent，还支持**持久化 Profile 隔离**——这是构建多 Agent 团队的核心机制。

### Profile 隔离的七个维度

每个 Profile 是一个完全独立的 agent 环境，隔离以下状态：

| 维度 | 说明 |
|------|------|
| configuration | 独立的 config.yaml |
| sessions | 独立的会话历史 |
| memory | 独立的 MEMORY.md / 记忆层 |
| skills | 独立的技能库 |
| personality | 独立的 SOUL.md（身份定义） |
| cron state | 独立的定时任务 |
| gateway state | 独立的消息路由 |

**关键区分**：Profile ≠ Persona。Persona 只是换名字的克隆，Profile 是完全隔离的 agent 环境。

### SOUL.md vs AGENTS.md 分离原则

| 文件 | 用途 | 粒度 | 稳定性 |
|------|------|------|--------|
| SOUL.md | "who the agent is" — 身份、语气、优先级、行为边界 | 每个 Profile 独立 | 高（很少变） |
| AGENTS.md | "shared project context" — 仓库结构、编码规范、工具规则 | 项目级共享 | 中（随项目变） |

不混合——身份稳定归 SOUL.md，项目上下文可变归 AGENTS.md。

### 推荐四角色团队模型

| 角色 | Profile | 优化方向 |
|------|---------|---------|
| Orchestrator | Hermes（默认） | 规划、分解、排序、综合、QA |
| Research Specialist | Alan | 证据、验证、怀疑精神、不确定性标注 |
| Writer | Mira | 清晰、结构、受众意识、打磨 |
| Builder/Debugger | Turing | 实现、调试、测试、可重现性 |

**扩展原则**："先 Orchestrator + 1 Specialist → 验证 handoff → 再扩展"。最聪明的团队不是最大的，而是角色边界最清晰的。

### Profile + Gateway = 跨平台控制面

Profile 配合 Gateway 后，每个角色可以绑定不同的 messaging 身份（Telegram、Discord、Slack 等），实现：
- 远程监控和任务分派
- 跨平台角色路由
- 从"本地组织功能"升级为"活的多 Agent 控制系统"

## 多 Agent 协作模式（delegate_task）

Hermes 同时支持通过 delegate_task 进行临时多 Agent 协作：

### 架构设计

- **同步阻塞**：父 agent 等待所有子 agent 完成后返回
- **强隔离**：子 agent 中间推理不污染父 agent 上下文，只返回 summary
- **并行执行**：ThreadPoolExecutor，最多 3 个任务并行
- **深度限制**：MAX_DEPTH = 2（父 → 子，不可再嵌套）

### 子 Agent 构造

每次委派创建全新的 AIAgent 实例：
- 独立的对话历史
- 独立的 Terminal session
- 专门构建的 System Prompt

**工具剥夺**：
- `delegate_task`：禁用（防止递归委派）
- `clarify`：移除（无法与用户交互）
- `memory`：剥夺（保护共享记忆）
- `execute_code`：禁用（避免复杂脚本）

### 生命周期控制

- 迭代上限：默认 50 轮
- **无超时机制**：子 agent 可无限运行
- 凭证管理：三级优先级解析，自动释放 lease

## Gateway 架构

- **统一会话路由**：跨平台（CLI、Telegram、Discord、Slack、WhatsApp、Signal、Email）
- **Session ID 绑定**：一个会话跨平台连续
- **Cron Ticking**：定时任务触发
- **闭环设计**：消息 + 技能创建 + 定时输出都通过 Gateway

## Agent Loop

```
消息到达 → 生成 Task ID
    ↓
加载系统提示（缓存/重建）
    ↓
Preflight 检查（上下文限制）
    ↓
[超限] → Compression（辅助模型摘要）
    ↓
API 调用 → 工具执行 → 循环
    ↓
最终响应 → 持久化 SQLite → Gateway 输出
```

**Compression 细节**：
- 辅助模型扫描全量对话
- 提取有价值内容写入 MEMORY.md（3,575 字符内）
- Lineage（参考链）保留在 SQLite，可追溯原始内容

**Prompt Caching**：
- 稳定前缀 → API Provider 缓存
- 缓存失效：切换模型、改记忆文件、改上下文文件
- Provider 容错：配置多个 Provider，自动降级

## Terminal Backends（6 种）

| Backend | 适用场景 |
|---------|----------|
| Local | 个人使用，无隔离需求 |
| Docker | 需要沙箱隔离 |
| SSH | 工作在远程服务器 |
| Singularity | HPC 集群环境 |
| Modal | Serverless，间歇使用 |
| Daytona | Serverless，间歇使用 |

**容器安全**：
- 只读根文件系统
- Linux capabilities 降权
- 命名空间隔离
- 零遥测（设计默认）

## 内置工具（40+）

- **Execution**：终端命令、代码运行
- **Web**：搜索、浏览器自动化
- **Media**：视觉、图像生成、TTS
- **Coordination**：子 Agent 委托、多模型推理
- **Memory/Planning**：记忆层交互

**扩展能力**：
- MCP 协议支持
- 四个插件钩子：`pre_llm_call`, `post_llm_call`, `on_session_start`, `on_session_end`

## 与 OpenClaw 对比

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 架构 | 单 Agent + 可选多 Agent | 多 Agent 协作 |
| 学习 | 自我改进闭环 | 外部编排 |
| 记忆 | 四层分离架构 | Hub 集中路由 |
| Gateway | 完整闭环（消息+技能+定时） | 仅消息分发 |
| Token 效率 | 高（渐进式加载） | 低（上下文膨胀） |
| 灵活性 | 中 | 高 |
| 超时控制 | ❌ | ✅ |
| Steer 能力 | ❌ | ✅ |

## 适用场景

**Hermes 更适合**：
- 长期个人助理
- 知识积累型任务
- 需要跨平台连续会话
- 定时自动化任务

**OpenClaw 更适合**：
- 模块化任务分发
- 需要运行时引导（Steer）
- 需要持久化和恢复
- 复杂多 Agent 协作

## 与其他实体的关系

- [[entities/openclaw|OpenClaw]] → 对比框架
- [[entities/harness-engineering|Harness Engineering]] → Harness 的具体实现
- [[entities/skill-system|Skill 系统]] → 自主技能创建机制
- [[entities/hook-mechanism|Hook 机制]] → 插件钩子
- [[topics/multi-agent-collaboration|多 Agent 协作]] → 可选能力

## 相关来源

- [[sources/hermes-multi-agent-team|How to Build a Multi-Agent Team in Hermes]] - Profile 隔离 + 四角色团队 + SOUL.md/AGENTS.md 分离
- [[sources/hermes-openclaw-multi-agent-comparison|同步阻塞 vs 异步编排]] - 多 Agent 协作模式对比
- [[sources/hermes-agent-inside|Inside Hermes Agent]] - 四层记忆架构、学习循环详解

## 开放问题

- 单 Agent 架构的上限是什么？任务复杂度 vs 知识积累
- 3,575 字符限制是否足够？策展策略的权衡
- 如何在保持 token 效率的同时引入超时控制和 Steer 能力？
- Honcho 用户建模层的实际效果如何？
