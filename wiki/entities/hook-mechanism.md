# Hook 机制

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 定义

Hook 机制是 Agent 系统中的事件拦截器。在 Agent 生命周期的关键节点（启动前、结束后等）自动触发预定义逻辑，实现系统级强制执行。核心价值：**不给 Agent 选择的机会，系统强制执行**。

## 核心特征

- **事件驱动**：在特定事件发生时自动触发
- **系统级强制**：不依赖 Agent 自觉
- **可扩展**：可定义自定义 Hook 逻辑
- **无侵入**：不需要修改 Agent 核心代码

## 关键 Hook 类型

### before_agent_start（启动前）

**触发时机**：Agent 每次启动时

**典型用途**：
- 自动检索并注入相关记忆
- 自动注入相关 Skill（Auto-recall）
- 注入系统级规范（AGENTS.md）

**实现逻辑**：
```python
@hook('before_agent_start')
def inject_memory(session):
    # 1. 提取任务关键词
    # 2. 混合检索相关记忆和 Skill
    # 3. 按相关度排序，取 Top-N
    # 4. 注入到 System Prompt
```

**效果**：Agent 想不看都不行。

### agent_end（结束后）

**触发时机**：每轮对话或任务结束时

**典型用途**：
- 自动捕获完整对话
- 智能去重
- 提炼 Skill

**实现逻辑**：
```python
@hook('agent_end')
def capture_memory(session):
    # 1. 内容哈希精确去重
    # 2. Top-5 相似度检测（阈值 0.75）
    # 3. LLM 最终裁决（DUPLICATE/UPDATE/NEW）
    # 4. 写入数据库
```

### 其他 Hook

- **before_tool_call**：工具调用前检查参数、权限
- **after_tool_call**：工具调用后记录结果、处理异常
- **on_error**：错误发生时记录、告警、反思

## 为什么 Hook 能实现 100% 捕获？

**传统方案的困境**：
- Agent 忙着处理任务，忘了调用 memory 工具
- Agent 觉得"这个不重要"，但其实很重要
- Session 崩了，还没来得及存

**Hook 的解决**：
- 不是 Agent "选择"去存记忆
- 而是系统在结束时**自动捕获**
- Agent 无法绕过

## 对比：有 Hook vs 无 Hook

| 维度 | 无 Hook | 有 Hook |
|------|---------|---------|
| 捕获率 | ~60%（依赖 Agent 自觉） | ~100%（系统强制） |
| 数据完整性 | 可能遗漏重要信息 | 完整轨迹 |
| 实现复杂度 | 低 | 中（需要 Hook 框架支持） |
| Agent 负担 | 需要主动调用 memory | 无负担 |

## Auto-recall：Hook 的进阶应用

**问题**：生成了 Skill，但如何让它在正确时机出现？

**解决**：通过 before_agent_start Hook 自动检索并注入相关 Skill。

**流程**：
1. Agent 收到新任务
2. Hook 拦截，提取任务关键词
3. 混合检索相关 Skill
4. 注入到 System Prompt
5. Agent 开始时已"看到"所有相关经验

**效果**：前一只工蜂的失败，变成后一只工蜂的本能。

## 与其他概念的关系

- [[entities/openclaw|OpenClaw]] → 原生支持 Hook
- [[entities/memos|MemOS]] → 利用 Hook 实现 100% 捕获
- [[entities/harness-engineering|Harness Engineering]] → Hook 是实现三根支柱的关键
- [[entities/auto-recall|Auto-recall]] → 基于 Hook 的 Skill 注入机制

## 相关来源

- [[sources/hermes-vs-memos-harness-engineering|给你的 Agent 搭操作系统]] - Hook 机制的详细说明

## 实践启发

对于 Atlas / WorkBuddy：
- OpenClaw 原生支持 Hook → 可利用
- 可实现：
  1. 每次任务结束自动记录到工作记忆文件
  2. 每次会话开始自动注入相关记忆
  3. 自动去重和提炼 Skill

## 开放问题

- Hook 会不会影响 Agent 性能？
- 如何设计 Hook 的执行顺序？
- 如何处理 Hook 失败的情况？
