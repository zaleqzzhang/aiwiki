---
title: "不懂 Harness Engineering，你所谓的 AI 转型只是在浪费预算"
source: "https://x.com/lewiskuo2/article/2043195969958007288"
author:
  - "[[Lewis爆肝战神 (@LewisKuo2)]]"
published: 2026-04-12
created: 2026-04-12
description: "Harness Engineering ：从硅谷爆火到应用落地同样用 Claude 或 GPT，有人让 AI 写了几行代码就卡住了，有人却让 AI 连续工作 6 个小时，交付了一个完整的项目。一个极端的案例来自 OpenAI3 名工程师，五个月，一行代码都没手写，指挥 Codex..."
tags:
  - "clippings"
---
![Image](https://pbs.twimg.com/media/HFrhoCxbEAAivD7?format=jpg&name=large)

## Harness Engineering ：从硅谷爆火到应用落地

同样用 Claude 或 GPT，**有人让 AI 写了几行代码就卡住了，有人却让 AI 连续工作 6 个小时，交付了一个完整的项目。**

一个极端的案例来自 OpenAI3 名工程师，五个月，一行代码都没手写，指挥 Codex Agent 写了 100 万行代码，做出了一个真实的产品。有内测用户在用，有 bug 要修，有功能要加。整个开发流程跑通了。

![Image](https://pbs.twimg.com/media/HFrZdzpakAA1or5?format=jpg&name=large)

差距在哪？2026 年初，OpenAI 和 Anthropic 几乎同时给出了答案：**Harness Engineering.**

这个词最近特别火。说白了就是给 AI 搭工程环境。有人把 AI 当聊天工具，有人把 AI 当长期合作的团队。区别就在这个“工程环境”上。

## Agent = Model + Harness

OpenAI 那个 100 万行代码的项目，工程师到底做了什么？

**他们没写代码，但做了三件事：设计环境、设定标准、建立反馈。这三件事合起来，就是 Harness。**

![Image](https://pbs.twimg.com/media/HFrbeNNbIAAMGJg?format=jpg&name=large)

Harness模式类比 [@odysseus0z](https://x.com/@odysseus0z)

Harness 这个词直译是“**马具**”。一匹马很强壮，但没有马鞍、缰绳、马镫，你骑不了它。AI 模型也一样，它很聪明，但你得给它一套“装备”，它才能真正干活。

LangChain 工程师 Viv 给了个更技术的定义：**Agent = Model + Harness。**模型提供智能，Harness 让这个智能能用起来。

![Image](https://pbs.twimg.com/media/HFrZnAXa8AAYAkm?format=jpg&name=large)

Harness 包括什么？系统提示词、工具、文件系统、沙箱、编排逻辑、各种检查机制。听起来很复杂，但本质就是三个问题：

AI 在哪干活？用什么干活？怎么知道干得对不对？

**这三个问题解决了，AI 就能持续工作。解决不了，AI 就只能陪你聊天。**

## 看 OpenAI 和 Anthropic 怎么解决

**OpenAI：给 AI 一个整理好的工作台**

OpenAI 团队最开始犯了个错误。他们把所有东西都写进一个文件：系统说明、架构规范、代码风格、注意事项，全塞进 AGENTS.md。结果 AI 被信息淹没了，性能反而下降。

后来他们发现，AI 跟人一样，需要的是一个整理好的工作台。需要的时候能找到东西就行。

他们把 AGENTS.md 精简到 100 行，只做“目录”。真正的内容放在结构化的 docs/目录里：设计文档、架构文档、计划文档分开存。AI 需要什么信息，去对应文件夹找。

这个改变背后有个洞察：AI 不需要一开始就知道所有事情，它需要在正确的时机拿到正确的信息。

除了文件系统，OpenAI 还给 AI 配了工具。bash、代码执行、沙箱环境。AI 可以自己写代码测试，出错了自己看日志，自己改。

更厉害的是，他们把日志、指标、UI 界面都暴露给 AI。AI 不只是写代码，它能看到代码跑起来是什么样，能判断自己写得对不对。

![Image](https://pbs.twimg.com/media/HFrZrZRbMAAKXJ6?format=jpg&name=large)

OpenAI 给 Codex 配了完整的可观测性堆栈。AI 可以查询日志、指标，看到应用运行的每个细节。

![Image](https://pbs.twimg.com/media/HFrZv_xbcAAqP8D?format=jpg&name=large)

有个细节很关键。他们花了好几个小时重写 Linter 的错误信息。目的是让 AI 能看懂“哪里错了，怎么改”。Linter 的受众从人变成了 AI。

**Anthropic：让 AI 学会自我检查**

Anthropic 工程师 Prithvi 发现了另一个问题：AI 评估自己的工作完全不靠谱。

不管做得好不好，AI 给自己的评价永远是“很好”。这在设计类任务上尤其明显。一个页面是精致还是平庸，AI 自己看不出来。

他的解决方案是把生成和评估拆成两个 AI。

生成器负责做前端页面。评估器拿到一个工具，能实际操作这个页面：点按钮、填表单、截图、检查功能。然后对照四个标准打分：设计质量、原创性、工艺、功能性。

评估器给出详细反馈，生成器根据反馈改进。这样来回 5 到 15 轮。

有个案例很有意思。Prithvi 让 AI 做一个荷兰艺术博物馆的网站。第 9 轮的时候，AI 做了个干净的深色主题页面，看起来还行。到第 10 轮，AI 突然推翻了整个设计，做了个 3D 空间：用 CSS 透视渲染了一个带棋盘地板的房间，艺术品挂在墙上，用户通过“门”导航到不同展厅。

这种创意的跃升，单个 AI 很难做到。

![Image](https://pbs.twimg.com/media/HFrZ0gLbcAA8KcK?format=png&name=large)

对比数据很直观。单 AI 跑 20 分钟花 9 美元，做出来的东西看着行，实际用不了。完整 Harness 跑 6 小时花 200 美元，交付了一个真正能玩的游戏，有精灵动画、AI 集成、导出功能。

OpenAI 的方法是另一个角度。他们定义了严格的架构规则：代码只能按 Types → Config → Repo → Service → Runtime → UI 这个方向依赖。违反规则的代码会被自动拦截。

有了这些约束，AI 写的代码就不会乱。速度不会下降，架构不会漂移。

## VergeX ：从摸索到实践

NoFx作为一个斩获了超过 11,500 颗 GitHub Stars、拥有 2.9k Forks 的开源的自主式 AI 交易助手，在 Trading 领域拥有海量的经验。

![Image](https://pbs.twimg.com/media/HFrZ4B0a0AACNdm?format=jpg&name=large)

现在原班人马通过对 Harness Engineering 的理解打造了**AI Trading Harness**

**他们将其归纳为4个核心模块**

**工具调用（Tool Use）：**Harness 负责去市场上把最新的价格、OI 数据抓过来喂给模型；等模型做完决定，Harness 再拿着 API Key 去交易所真正把订单发出去。模型永远只负责"想"，从不直接碰市场。

**上下文管理（Context Management）：**Harness 会持续维护一个备忘录，记录当前持仓状态、账户余额、过去的决策历史。每次调用模型前，Harness 都会把这个备忘录注入进去，防止模型失忆。Anthropic 在工程博客中把这个问题称为Context Rot（上下文腐烂），这是长时运行 Agent 最核心的挑战。

**架构约束（Architectural Constraints）：**这是最关键的一层。你在 VergeX 策略里写的那些规则（最大杠杆、最大仓位数），不只是写在 Prompt 里"请求"模型遵守，而是在后端代码层面强制执行。OpenAI 在工程实践中明确指出：通过强制执行不变量，而非对实施过程进行微观管理，这才是让 Agent 能够快速运行而不崩溃的核心原则。

**反馈循环（Feedback Loop）：**每次交易结束后，结果会被结构化地写回系统，让 Agent 在下一次决策时能够参考历史表现，形成持续优化的闭环。

![Image](https://pbs.twimg.com/media/HFrZ7rsboAA75v5?format=jpg&name=large)

**最近 Vergex** [@vergex\_ai](https://x.com/vergex_ai) **联合** [Z.ai](https://z.ai/) **开展 GLM5 Trading Odyssey 活动**

新用户完成邮箱注册后，可参与本次活动；已完成注册的老用户无需重复注册，在完成活动要求后同样可获得 500万 token 额度，白嫖教程连接我放在评论区了。

以上内容为个人学习与技术分享，交易有风险，入市须谨慎