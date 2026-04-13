# 认知缺口：Skill ≠ Tool

## 读者"以为懂"的认知

"Skill 不就是另一种说法的 Tool 吗？给 Agent 加个搜索 Skill、加个写文件 Skill——跟加个搜索工具、加个文件工具有什么区别？"

## 实际的关键区别

### Tool 是"手"，Skill 是"肌肉记忆"

Tool 让 Agent 能做某件事（搜索网页、读写文件、调用 API）。Skill 让 Agent 知道**什么时候用什么 Tool、按什么顺序用、常见陷阱是什么**。

一个具体的例子：

**Tool**：`web_search(query: string) → results`

**Skill**：
```yaml
trigger: 用户询问最新的技术趋势
workflow:
  1. 先搜英文关键词（覆盖率高）
  2. 再搜中文关键词（补充国内视角）
  3. 对比两次结果，交叉验证
  4. 如果信息有冲突，标注并呈现两种说法
common_pitfalls:
  - 不要只搜一种语言，信息会有偏差
  - 注意时效性，超过 3 个月的信息标注日期
```

Tool 是一行函数签名，Skill 是一段操作手册。Agent 有了 `web_search` Tool 就**能**搜索，有了上面的 Skill 就**会**搜索得好。

### 三者的协作关系

宝玉的分析给出了一个精确的类比：

- **CLI** 是 Agent 的"手"——执行命令
- **MCP** 是 Agent 的"另一种手"——调用外部服务
- **Skills** 是 Agent 的"肌肉记忆"——知道什么场景该用什么手、怎么用

三者不是替代关系，而是协作关系。Skill 不执行任何操作，它只告诉 Agent 应该执行什么操作。实际的执行仍然通过 Tool 完成。

### 为什么这个误解值得纠正

如果你把 Skill 当 Tool 来设计，你会写出这样的东西：

```yaml
name: format_code_skill
action: 调用 prettier 格式化代码
```

这不是 Skill，这是 Tool 的描述。真正的 Skill 应该是：

```yaml
name: code_formatting_workflow
trigger: 提交代码审查之前
overview: 确保代码风格一致
workflow:
  1. 先运行 ESLint 检查语法错误（不是格式问题）
  2. 修复语法错误后再运行 Prettier 格式化
  3. 检查 diff——如果 Prettier 改动超过 20 行，可能是缩进配置不一致
  4. 确认 .prettierrc 和项目标准一致
common_pitfalls:
  - 不要在有语法错误的代码上跑 Prettier，会产生更混乱的输出
  - 注意 Prettier 和 ESLint 的规则冲突（安装 eslint-config-prettier）
```

前者是"做什么"，后者是"怎么做好"。Agent 缺的不是"能力"（Tool 提供了），而是"经验"（Skill 沉淀了）。

## 正确的心智模型

@eng_khairallah1 的定义最到位：**"A Skill is a training manual for a new employee."**

新员工入职时，公司不会只给他一个工具箱（"这是锤子、这是扳手"）。还会给他一本操作手册（"遇到这种情况先用锤子、再用扳手、注意不要反了"）。

Tool = 工具箱。Skill = 操作手册。两者缺一不可。

## 写文章时可以这样切入

> "给 Agent 加了 20 个 Tool 但表现还是不好？问题可能不是 Tool 不够，而是 Agent 不知道'什么时候该用哪个'。这就是 Skill 要解决的——不是给 Agent 更多的手，而是给它'肌肉记忆'。"

## 相关页面

- [[craft/concepts/skill|概念：Skill]] - Skill 的完整概念讲解
- [[sources/building-claude-skills-guide|如何构建真正有效的 Claude Skills]] - Skill 设计的五组件方法
- [[entities/skill-system|Skill 系统]] - 两种 Skill 生成路径对比
