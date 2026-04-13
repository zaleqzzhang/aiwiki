# 深度解析 OpenClaw 在 Prompt / Context / Harness 三个维度中的设计哲学与实践

**类型**: article
**日期**: 2026-04-13
**来源**: https://mp.weixin.qq.com/s/JycTfNd7EnmWCnJK-QCf0Q
**作者**: 飞樰（阿里云开发者）
**深度**: 源码分析级
**标签**: #openclaw #prompt-engineering #context-engineering #harness-engineering #agent-architecture

## 摘要

从 Prompt Engineering、Context Engineering、Harness Engineering 三个维度深入源码分析 OpenClaw 的设计哲学。核心论点：OpenClaw 不是凭空诞生的新物种，而是将近年来 Agent 发展中的关键技术进行了系统性集成与升华。文章从 `buildAgentSystemPrompt()` 函数的 23 个模块拆解开始，到上下文压缩的分块摘要算法，再到 Hook 系统和三层安全沙箱，提炼出可复用的方法论。

## 关键要点

### 一、Prompt Engineering：动态组装与文件驱动

1. **System Prompt 23 模块拼装**：`buildAgentSystemPrompt()` 接收几十个参数，按固定顺序将模块"搭积木"式拼装。核心模块包括：身份标识（永远存在）→ 工具清单 → 安全准则 → Skills 条件加载 → 记忆召回 → Workspace 文件注入 → 心跳机制 → 运行时信息（永远存在）

2. **三种 PromptMode**：
   - `full`（完整模式）：主 Agent 与用户直接对话，所有模块全部加载
   - `minimal`（精简模式）：子 Agent 执行独立任务，只保留核心模块
   - `none`（极简模式）：基本上只有一行身份标识

3. **Markdown 驱动的文件注入**：AGENTS.md（总纲/骨架）→ SOUL.md（灵魂/人格）→ IDENTITY.md（身份证/外在标识）→ USER.md（主人档案）→ TOOLS.md（工具清单）→ HEARTBEAT.md（心跳任务）→ BOOTSTRAP.md（出生证明，首次启动后自删）→ BOOT.md（启动文件）→ MEMORY.md（长期记忆）

4. **极简主义措辞**："Quality > quantity" 一句话传达复杂指令，节省 Context Window 空间

### 二、Context Engineering：扩展、压缩和记忆

5. **Skills 渐进式披露**：默认不加载全部能力，通过 ClawHub 市场或用户导入获取新 Skill，任务需要时才将 Skill 名称和描述注入上下文。安全风险：Skill 包含可执行脚本，可能被植入恶意代码

6. **上下文压缩（Compaction）**：
   - 触发方式：手动（`/compact`）或自动（token 用量 > 上下文窗口 - 预留空间）
   - 分块参数：`BASE_CHUNK_RATIO = 0.4`（每块占 40%），`MIN_CHUNK_RATIO = 0.15`（最小 15%），`SAFETY_MARGIN = 1.2`（20% 缓冲）
   - 三级摘要策略：`summarizeInStages()`（顶层）→ `summarizeChunks()`（分块）→ `summarizeWithFallback()`（兜底，返回 "No prior history."）
   - 保留项：活跃任务、重要决策、TODO、承诺、不透明标识符（UUID/哈希原文保留）
   - 超时保护：5 分钟上限
   - 写锁：防并发

7. **精细化修剪（Pruning）**：
   - 头尾保留、中间省略（错误信息通常在首尾，JSON 核心定义在头部）
   - 裁剪不超过 50%
   - KV Cache 时间窗口过期后主动剔除旧片段

8. **Memory 双层管理**：
   - **MEMORY.md（长期记忆）**：自动注入系统提示词，截断到 200 行，保持精简
   - **memory/日期.md（每日笔记）**：追加写入，细节化日常交互
   - **写入策略**：显式写入（用户指令）+ 隐式闪存（Memory Flush，会话结束/新 Session/压缩时自动）
   - **召回机制**：BM25 + 向量双路召回 → SQLite 存储分块和索引
   - **时间衰减**：衰减系数 = e^(-λ × 天数)，λ = ln(2) / 半衰期（默认 30 天）。30 天减半，60 天剩 1/4，90 天剩 1/8

### 三、Harness Engineering：约束与引导控制

9. **Harness 定义**：确保模型"可控地做"。Prompt = What & How，Context = How Better，Harness = How Controlled

10. **Harness vs Workflow**：
    - Workflow：硬编码线性路径，模型只是节点，主导权在"人"
    - Harness：动态软约束，模型保留自主规划和迭代权，主导权在"AI"
    - 核心区别：**主导权归属**

11. **7 种 Hook 钩子**：`before_prompt_build` → `before_tool_call` → `after_tool_call` → `before_compaction` → `after_compaction` → `message_received` → `message_sending`

12. **三层安全沙箱**：
    - 第一层：文件系统沙箱（限制 Workspace 访问范围）
    - 第二层：命令执行沙箱（白名单 + Ask 模式 + safeBins 豁免）
    - 第三层：网络访问沙箱（白名单域名 + 防泄露）

13. **Human-in-the-Loop**：高风险操作暂停执行，等待人类确认

## 关联实体

- [[entities/openclaw|OpenClaw]] - 本文的分析对象，源码级深度剖析
- [[entities/harness-engineering|Harness Engineering]] - 文章的核心概念之一，提供了与 Workflow 的清晰辨析
- [[entities/context-engineering|Context Engineering]] - 上下文压缩与记忆管理的工程实现

## 引用此来源的页面

- [[topics/agent-architecture|Agent 架构]]
- [[entities/openclaw|OpenClaw]]
