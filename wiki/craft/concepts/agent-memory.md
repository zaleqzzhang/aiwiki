# 概念：Agent Memory（Agent 记忆）

## 一句话

Agent 积累自身经验的系统——不是检索外部知识（那是 RAG），而是"知道什么该记、什么时候用、什么时候忘"。

## 为什么需要这个概念

没有记忆的 Agent 就像每天失忆的员工：每次上班都要重新介绍自己、重新学习流程、重新犯同样的错误。这不只是体验问题，而是**效率和能力的根本瓶颈**。

Harrison Chase 的观察很精准：**专用 Agent（反复做同一任务）比通用 Agent 更需要记忆**。因为一次会话的经验可以立即应用到下一次。如果你的 Agent 是做邮件分类的，它上次学到"这个客户喜欢简短回复"——这个信息对下一次极其有价值。但没有记忆系统，它不知道。

## 与相邻概念的边界

- **vs RAG**：RAG 检索的是**外部文档**（预先准备的知识库），Agent Memory 积累的是**自身运行经验**（什么方法有效、什么会失败、用户偏好什么）。前者是查百科全书，后者是翻自己的工作日志。
- **vs Context Window**：Context Window 是模型单次对话的"工作台"，会话结束就清空。Memory 是跨会话持久化的，不随单次对话结束而消失。
- **vs State**：State 是当前任务的执行状态（"我现在在做第三步"），Memory 是跨任务积累的经验（"上次做类似任务时，第三步容易出错"）。

| 维度 | RAG | Agent Memory |
|------|-----|--------------|
| 数据来源 | 外部文档（预先准备） | Agent 自身的运行轨迹 |
| 更新时机 | 离线索引 | 实时/会话结束时 |
| 内容性质 | 事实性知识 | 经验性知识 |
| 组织方式 | 向量相似度 | 结构化分层（热/冷/程序性） |
| 淘汰机制 | 通常没有 | 主动策展（有限空间强制淘汰） |

## 设计直觉

Agent Memory 的核心设计问题不是"怎么存"，而是**"什么值得记"**。

存储是容易的——往数据库里写条记录。难的是策展（curation）：当记忆空间有限时，保留什么、丢弃什么？Hermes 用 3,575 字符的硬限制强制策展；MemOS 用无界存储 + 智能去重。两种哲学各有道理，但都在回答同一个问题：**如何让最重要的经验始终可及？**

更深层的设计选择是：**谁来决定什么值得记？**

- Agent 自治（Hermes 倾向）：Agent 最清楚什么值得记住，让它自己决定。代价是捕获率不完整（~60%）。
- 系统强制（MemOS 倾向）：Agent 会偷懒、会遗漏，系统 Hook 保证 100% 捕获，再用智能去重处理噪音。

这个选择映射到所有自治系统的治理困境：全面监控 vs 自主裁量。

## 理解的关键转折

**记忆不是一个功能，而是一个架构层**。

很多人把 Memory 当作一个"插件"——给 Agent 加个记忆模块就行了。但 Harrison Chase 指出，Memory 与 Harness 是深度绑定的：Harness 决定了短期记忆如何管理、长期记忆如何持久化、AGENTS.md 如何加载进 context、Skill 元数据如何展示、什么在压缩中保留什么丢失。

Sarah Wooders 的比喻更直接："Asking to plug memory into an agent harness is like asking to plug driving into a car." 记忆不是插上去的零件，而是 Harness 的有机组成部分。

这意味着：**如果你不拥有 Harness，你就不拥有 Memory**。你在封闭平台上积累的记忆，平台一改版就可能丢失。

## 展开时可用的例子

- **Hermes 四层记忆**：Prompt Memory（始终在线，3,575 字符）→ Session Search（按需检索，FTS5）→ Skills（渐进加载）→ Honcho（用户建模）。分层原则是按"需要的频率"排列：越常用越靠前。
- **LangSmith Agent Builder**：采用"文件即记忆"设计（memory/ 目录下分 preferences/、learnings/、context/），Agent 直接读写文件，不依赖外部服务。设计原则是便携性——记忆可以迁移到其他 Harness。
- **MemOS 无界记忆**：35,000+ 条记忆、65,524 条关系边，三层去重机制（内容哈希 → 相似度检测 → LLM 裁决），混合搜索（BM25 + 向量 + RRF 融合）。证明"无限存储"路径的工程可行性。

## 相关页面

- [[entities/hermes|Hermes]] - 四层记忆架构的代表
- [[entities/hook-mechanism|Hook 机制]] - 实现 100% 记忆捕获
- [[entities/skill-system|Skill 系统]] - 程序性记忆的具体形态
- [[sources/agent-builder-memory-system|Agent Builder 的记忆系统]] - "文件即记忆"设计
- [[sources/harness-memory-lockin|Your Harness, Your Memory]] - Memory 锁定风险
