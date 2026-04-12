# @eng_khairallah1

**类型**: person
**创建日期**: 2026-04-12
**最后更新**: 2026-04-12
**来源数量**: 1

## 简介

Claude Skills 设计专家，《How to Build Claude Skills That Actually Work》作者。专注于 Skills 的设计模式、失败模式和测试方法，总结出高质量 Skill 的五组件结构。

## 核心观点

- Skill 是训练手册，不是代码
- 高质量 Skill = 五组件 + 避免五种失败模式 + 五项测试
- 指令必须具体可测试，"格式漂亮"不可测试，"用 H2 标题，段落不超过 3 句"可测试
- 一个具体示例值 50 行抽象指令

## 主要贡献

- 提出 Skill 五组件结构（YAML Trigger + Overview + Workflow + Output Format + Examples）
- 总结五种失败模式（Silent/Hijacker/Drifter/Fragile/Overachiever）
- 建立五项测试协议（Happy path/Minimal/Edge case/Negative/Repeat）

## 相关来源

- [[sources/building-claude-skills-guide|如何构建真正有效的 Claude Skills]] - 完整 Skill 设计方法论

## 与其他实体的关系

- [[entities/skill-system|Skill 系统]] → 提供 Skill 设计的具体方法论
- [[entities/harness-engineering|Harness Engineering]] → Skill 是 Harness 的可复用组件
