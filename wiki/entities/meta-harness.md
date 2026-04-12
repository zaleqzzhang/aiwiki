# Meta-Harness

**类型**: concept
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 定义

Meta-Harness 是一种 Harness 层持续学习的方法：让 Agent 在一批任务上运行 → 评估结果 → 存储日志 → 用编码 Agent 分析 trace → 建议修改 Harness 代码。这是一种"用 Agent 改进 Agent"的自动化流程。

## 工作流程

1. **运行任务**：Agent 在一批任务上执行
2. **评估结果**：判断成功/失败，记录问题
3. **存储 Trace**：将完整执行路径存入文件系统
4. **分析 Trace**：编码 Agent 读取 trace，识别模式
5. **建议修改**：编码 Agent 提出 Harness 代码的改进建议
6. **应用更新**：人类审核或自动应用修改

## 核心洞察

- **Trace 是数据源**：所有改进都基于实际执行记录
- **Agent 改进 Agent**：不需要人类手动分析，让编码 Agent 完成分析和修改
- **自动化程度高**：可以定期运行，持续改进

## 相关研究

- 论文：**Meta-Harness: End-to-End Optimization of Model Harnesses**

## 与其他概念的关系

- [[entities/langsmith|LangSmith]] → 提供 trace 收集能力
- [[entities/langsmith-cli|LangSmith CLI]] → 让编码 Agent 访问 trace
- [[entities/deep-agents|Deep Agents]] → 已应用此方法改进
- [[entities/continual-learning|持续学习]] → Harness 层的实现方式

## 相关来源

- [[sources/continual-learning-ai-agents|Continual Learning for AI Agents]] - 作为 Harness 层学习的代表性方法

## 实践启发

可用于改进 Atlas / WorkBuddy：
- 收集 Atlas 的执行 trace
- 定期分析：哪些类型的任务成功率高？哪些容易失败？
- 让编码 Agent（或我）分析 trace，提出 Harness 改进建议
- 更新 AGENTS.md、工具配置、流程规范

## 开放问题

- 改进频率如何设定？
- 如何避免改进导致的性能退化？
- 编码 Agent 的分析质量如何保证？
