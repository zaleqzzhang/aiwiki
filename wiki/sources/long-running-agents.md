# Effective Harnesses for Long-Running Agents

**类型**: article
**作者**: Justin Young (Anthropic)
**日期**: 2026-04-13
**来源**: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
**标签**: #harness #long-running #multi-context-window #anthropic

## 摘要

Anthropic 官方文章，解决长时间运行 Agent 跨上下文窗口工作的核心挑战。提出两阶段解决方案：Initializer Agent 设置环境基础，Coding Agent 增量推进并保持"清洁状态"。核心洞察是 Agent 需要快速理解工作状态，通过 Feature List、进度文件、Git 历史实现会话间记忆传递。

## 关键要点

### 核心问题
- **上下文窗口限制**：复杂项目无法在单个窗口完成
- **离散会话**：每个新会话从零记忆开始
- **类比**：工程师轮班工作，每人到达时无上一班记忆

### 两大失败模式

| 模式 | 表现 | 后果 |
|------|------|------|
| One-shot 症候群 | 试图一次性完成所有任务 | 上下文耗尽，功能半实现，无文档 |
| 过早宣布完成 | 看到进展就宣布项目完成 | 遗漏功能，质量不达标 |

### 两阶段解决方案

1. **Initializer Agent**（首次会话）
   - 创建 `init.sh` 脚本（运行开发服务器）
   - 创建 `claude-progress.txt`（进度日志）
   - 初始 Git commit（展示新增文件）
   - 编写 Feature List（JSON 格式，200+ 功能项）

2. **Coding Agent**（后续会话）
   - 增量推进：每次只做一个功能
   - 保持清洁状态：可合并到主分支的代码质量
   - 会话结束：Git commit + 进度更新

### 核心机制

**Feature List（功能清单）**
```json
{
  "category": "functional",
  "description": "New chat button creates a fresh conversation",
  "steps": [
    "Navigate to main interface",
    "Click the 'New Chat' button",
    "Verify a new conversation is created"
  ],
  "passes": false
}
```

**会话启动例程**
1. `pwd` - 确认工作目录
2. 读 Git logs 和进度文件
3. 读 Feature List，选最高优先级未完成功能
4. 运行 `init.sh` 启动开发服务器
5. 基础端到端测试验证

**测试验证**
- 使用 Puppeteer MCP 进行浏览器自动化
- 端到端测试（像人类用户一样操作）
- 截图验证功能完整性

### 设计原则

- **增量优于一次性**：避免上下文耗尽
- **清洁状态**：每次会话结束时代码可合并
- **显式优于隐式**：强指令防止模型偷懒
- **JSON 优于 Markdown**：模型更难不当修改结构化文件

## 关联实体

- [[entities/harness-engineering]] - Harness Engineering 方法论
- [[entities/skill-system]] - 任务轨迹提炼为可复用经验
- [[entities/hermes]] - 同样采用会话间记忆传递

## 引用此来源的页面

- [[entities/harness-engineering]] - 增量推进、会话间记忆传递实践

## 开放问题

- 单一通用 Agent vs 多 Agent 架构哪个更适合长时任务？
- 如何将这些实践推广到非 Web 开发领域（科研、金融建模）？
- Feature List 的粒度如何确定（200+ 是否过多）？
