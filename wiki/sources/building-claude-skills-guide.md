# 如何构建真正有效的 Claude Skills

**类型**: article
**日期**: 2026-04-09
**来源**: https://x.com/eng_khairallah1/status/2042165607999644099
**作者**: [[entities/eng-khairallah1|@eng_khairallah1]]
**标签**: #ClaudeSkills #Skill设计 #生产实践

## 摘要

Claude Skills 社区注册表有 80,000+ Skills，但大多数质量堪忧。文章总结出高质量 Skill 的五组件结构和五种失败模式，提供了完整的测试协议。核心观点：Skill 是训练手册而非代码，质量取决于指令的精确性和测试的充分性。

## 关键要点

### Skill 的本质

> "A Claude Skill is a markdown file called SKILL.md that teaches Claude how to perform a specific task. Think of it as a training manual for a new employee."

**关键区别**：
- 无 Skill：Claude 给通用、最佳猜测的回答
- 有 Skill：Claude 遵循特定流程，按特定格式输出，应用特定质量标准

### Skill 的五组件结构

**组件 1：YAML Trigger Header**
```yaml
---
name: proposal-generator
description: >
  Generates professional business proposals...
  Use this skill when user says 'write a proposal', 'draft a proposal'...
  Do NOT use for internal project plans, SOWs, or technical specifications.
---
```

**三个规则**：
1. 激进激活：至少 5-7 个显式触发短语
2. 负边界：明确说明何时**不**激活
3. 第三人称：用 "Generates..." 而非 "I can..."

**组件 2：Overview**
- 一段话告诉 Claude 这个 Skill 做什么、何时激活
- 写给 Claude，不是给人类

**组件 3：Step-by-Step Workflow**
```markdown
## Workflow

1. Collect information from user (ask if not provided):
   - Client name and company
   - Project description
   ...

2. Read template at references/proposal-template.md

3. Generate proposal with these sections:
   - Executive Summary (3 sentences max)
   ...
```

**每条指令必须**：
- 一个清晰动作
- 祈使句（"Read the file..." 而非 "The file should be read..."）
- 足够具体，只有一种解读方式

**组件 4：Output Format Specification**
```markdown
## Output Format

- Document type: Markdown
- Total length: 500-800 words
- Headings: H2 for main sections
- Tone: professional, confident, direct
- Do NOT include: filler phrases, template language
```

**组件 5：Examples and Edge Cases**
- 至少两个示例：正常路径 + 边界情况
- 3-5 个更好
- 一个具体示例值 50 行抽象指令

### 五种失败模式

| 失败模式 | 症状 | 诊断 | 修复 |
|----------|------|------|------|
| Silent Skill | 从不激活 | YAML description 太弱 | 添加更多触发短语 |
| Hijacker | 错误请求时激活 | description 太宽泛，缺负边界 | 添加明确的负边界 |
| Drifter | 激活正确，输出错误 | 指令歧义 | 用具体、可测试的指令替换 |
| Fragile Skill | 正常输入正常，异常输入崩溃 | 边界处理不完整 | 添加边界条件指令 |
| Overachiever | 添加未请求的内容 | 只说了要做什么，没说不做什么 | 添加显式范围约束 |

### 测试协议（必做）

**五项测试**：
1. Happy path：完整理想输入
2. Minimal input：最小可能输入
3. Edge case input：异常输入（矛盾、极短/长、不同语言、拼写错误）
4. Negative test：不应激活的请求
5. Repeat test：相同输入三次，检查一致性

**修复循环**：
```
Fix failure → Update SKILL.md → Test again → Repeat until all 5 pass
```

### 部署方式

- 个人使用：`~/.claude/skills/`
- 项目特定：`.claude/skills/` in project directory
- 重启 Claude 后生效

## 核心框架

```
高质量 Skill 公式
├── 结构（5 组件）
│   ├── YAML Trigger（激活条件）
│   ├── Overview（一句话说明）
│   ├── Workflow（步骤化流程）
│   ├── Output Format（输出规范）
│   └── Examples（具体示例）
├── 避免（5 种失败）
│   ├── Silent（不激活）
│   ├── Hijacker（乱激活）
│   ├── Drifter（输出漂移）
│   ├── Fragile（脆弱）
│   └── Overachiever（过度输出）
└── 测试（5 项必做）
    ├── Happy path
    ├── Minimal input
    ├── Edge case
    ├── Negative test
    └── Repeat test
```

## 关联实体

- [[entities/skill-system|Skill 系统]] - Skill 的设计方法论
- [[entities/harness-engineering|Harness Engineering]] - Skill 是 Harness 的一部分
- [[entities/eng-khairallah1|@eng_khairallah1]] - 作者

## 引用此来源的页面

- [[entities/skill-system|Skill 系统]] - 补充 Skill 设计的具体方法论

## 实践启发

对 Atlas / WorkBuddy：
- 当前 AGENTS.md 类似 Skill 的概念
- 可以将 AGENTS.md 改进为更结构化的 Skill 格式
- 添加明确的触发条件和负边界
- 添加测试协议验证 AGENTS.md 的有效性

对庆正：
- 可以为常用任务创建 Skills（如写公众号文章、生成财报解读）
- 每个 Skill 遵循五组件结构
- 建立测试流程确保 Skill 质量
