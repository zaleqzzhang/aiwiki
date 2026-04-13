# Inside Hermes Agent: How a Self-Improving AI Agent Actually Works

**类型**: article
**作者**: [[entities/mr-anand|Mr. Ånand]]
**日期**: 2026-04-03
**来源**: https://mranand.substack.com/p/inside-hermes-agent-how-a-self-improving
**标签**: #hermes #self-improving #memory-architecture #learning-loop

## 摘要

Hermes Agent 架构深度解析，展示单 Agent 如何通过闭环学习实现自我改进。核心创新是将"发生了什么"升级为"什么有效"，通过四层记忆架构、自主技能创建、FTS5 会话检索构建完整的学习循环。与 OpenClaw 多 Agent 协作不同，Hermes 选择单 Agent + 知识积累路径。

## 关键要点

### 核心差异

| 维度 | OpenClaw | Hermes |
|------|----------|--------|
| 架构 | 多 Agent 协作 | 单 Agent |
| 学习 | 外部编排 | 自我改进闭环 |
| 记忆 | Hub 集中路由 | 四层分离架构 |
| 适用 | 任务分发 | 知识积累 |

### 学习循环四要素

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

**分层原则**
- 重要性 → Prompt Memory（每个会话都需要）
- 相关性 → Session Search（特定主题触发时检索）
- 程序性 → Skills（任务执行时加载）
- 用户特性 → Honcho（长期个人助理场景）

### Gateway 架构

- **统一会话路由**：跨平台（CLI、Telegram、Discord、Slack、WhatsApp、Signal、Email）
- **Platform Adapters**：各平台适配器
- **Session ID 绑定**：一个会话跨平台连续
- **Cron Ticking**：定时任务触发

### Agent Loop 内部

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

**Compression 细节**
- 辅助模型扫描全量对话
- 提取有价值内容写入 MEMORY.md（3,575 字符内）
- Lineage（参考链）保留在 SQLite，可追溯原始内容

**Prompt Caching**
- 稳定前缀 → API Provider 缓存
- 缓存失效：切换模型、改记忆文件、改上下文文件
- Provider 容错：配置多个 Provider，自动降级

### Terminal Backends（6 种）

| Backend | 适用场景 |
|---------|----------|
| Local | 个人使用，无隔离需求 |
| Docker | 需要沙箱隔离 |
| SSH | 工作在远程服务器 |
| Singularity | HPC 集群环境 |
| Modal | Serverless，间歇使用 |
| Daytona | Serverless，间歇使用 |

**容器安全**
- 只读根文件系统
- Linux capabilities 降权
- 命名空间隔离
- 零遥测（设计默认，非可选）

### 技能系统

**40+ 内置工具**
- Execution：终端命令、代码运行
- Web：搜索、浏览器自动化
- Media：视觉、图像生成、TTS
- Coordination：子 Agent 委托、多模型推理
- Memory/Planning：记忆层交互

**Skills 结构**
```
~/.hermes/skills/
├── mlops/
│   ├── axolotl/
│   │   ├── SKILL.md          # 主指令（必需）
│   │   ├── references/       # 附加文档
│   │   ├── templates/        # 输出格式
│   │   ├── scripts/          # 辅助脚本
│   │   └── assets/           # 补充文件
│   └── vllm/
│       └── SKILL.md
├── devops/
│   └── deploy-k8s/           # Agent 创建的技能
│       ├── SKILL.md
│       └── references/
└── .hub/                     # Skills Hub 状态
    ├── lock.json
    ├── quarantine/
    └── audit.log
```

### 与 OpenClaw 对比

| 维度 | OpenClaw | Hermes |
|------|----------|--------|
| Gateway | 仅消息分发 | 完整闭环（消息+技能+定时） |
| 学习 | 外部机制 | 内置循环 |
| 记忆 | Hub 集中 | 四层分离 |
| 适用 | 模块化任务分发 | 长期知识积累 |

## 关联实体

- [[entities/hermes]] - Hermes Agent 工具
- [[entities/openclaw]] - OpenClaw 多 Agent 框架
- [[entities/skill-system]] - 技能创建和管理机制
- [[entities/hook-mechanism]] - 插件钩子机制

## 引用此来源的页面

- [[entities/hermes]] - 四层记忆架构、学习循环
- [[topics/multi-agent-collaboration]] - 单 Agent vs 多 Agent 对比

## 开放问题

- 单 Agent 架构的上限是什么？任务复杂度 vs 知识积累
- Honcho 用户建模层的实际效果如何？
- 3,575 字符限制是否足够？策展策略的权衡
- Nebius Token Factory 托管方案的经济性如何？
