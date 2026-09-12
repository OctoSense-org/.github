# OctoSense

[English](README.md)

**运行在操作系统之上的 Agent 交互 Shell。**

OctoSense 是传统操作系统之上的一层，可运行于 Windows、macOS、Linux、Android、iOS 与鸿蒙。它看起来是你熟悉的 launcher 和应用，行为上却是一个能理解你的意图、感知周围变化、并在你开口之前重组这些应用的智能体。

## 为什么不是又一个聊天窗口

现在的 Agent 交互界面都是聊天框。无论消息里内嵌了什么渲染，交互本质上仍是线性、单向的文本流。这等于回到了图形界面出现之前的年代。用户在鼠标、多点触屏和应用这套交互上已经积累了几十年的习惯，让他们改用文本提示框，代价远大于收益。

OctoSense 走的是另一条路：保留人们已经熟悉的交互方式，把 Agent 的能力植入应用，而不是把人从应用里拉进聊天框。

## OctoSense 做什么

- **熟悉的入口。** 新闻、天气、行情、出行、视频、电商。每个入口都是稳定的起点，和今天的 launcher 一样，你也可以直接操作任何应用。
- **意图驱动的内容。** 入口之下，内容围绕你的意图、偏好和上下文生成与组织。出行入口会聚合天气、路线、酒店、门票和当地去处；新闻入口聚合你真正关注的内容。
- **主动，而非被动。** Agent 由时间和事件驱动，而不只是回答提问。降温、暴雨导致航班延误、目的地罢工、孩子学校日程变化、家人的健康信号，都会促使应用重新组织、提出建议、提前准备。
- **删繁就简。** 超级应用为十亿人打造，什么都有。OctoSense 把每个应用裁剪到你需要的部分，持续生成、持续进化，而不是给所有人同一个固定界面。
- **人在回路。** 重要决定留给你。Agent 把一天提炼成需要你确认的几件事，确认之后，下游的一切随之更新。
- **情绪价值和使用价值并重。** 风格、配色、字体、氛围随人和时刻变化：早晚、寒暑、节气。像章鱼一样，它在感知，也在变色。

你看到的每个应用都是触点。触点背后是同一个 Agent、同一份记忆、对你所处环境持续进化的整体理解，以及你允许它看到的数据：新闻、位置、天气等公开信号，邮件、消息、日历、可穿戴设备等私有数据。

## 技术构成

| 层 | 项目 | 作用 |
| --- | --- | --- |
| Shell | [OctoSense](https://github.com/OctoSense-org/OctoSense) | 跨平台 Agent 交互 Shell，基于 [Makepad](https://github.com/OctoSense-org/makepad)。应用作为 Agent 的触点在其中运行。 |
| 语言 | [Octoscript](https://github.com/OctoSense-org/Octoscript) | 由 Makepad 的 Splash 演化而来、面向 Agent 需求优化的动态 DSL。无需编译即可实时解释执行应用逻辑并生成界面。用起来像 JavaScript，底座是 Rust。 |
| 渲染 | [Octoscript-Makepad](https://github.com/OctoSense-org/Octoscript-Makepad) · [Octoscript-Android](https://github.com/OctoSense-org/Octoscript-Android) · [Octoscript-OH](https://github.com/OctoSense-org/Octoscript-OH) | 把 Octoscript 渲染到 Makepad、Android 原生控件和 OpenHarmony ArkUI。 |
| App Card | [Octoscript-AppCard](https://github.com/OctoSense-org/Octoscript-AppCard) | 用 Octoscript 构建的可组合、可嵌入的应用模板与 applet。Card 可以相互嵌入、嵌入流程，生成的 Card 又可以成为更高层的模板。 |
| 内核 | [Octos](https://github.com/ymote/octos) | 可嵌入的 Rust 原生 Agent harness。多轮交互、上下文与记忆、模型 provider、多 agent 并发、工具与沙箱、用户审批，全部通过 OS UI protocol 提供给上层应用。 |

一切都经由 Octoscript 动态执行，不需要编译，所以一个应用可以在几秒内换一种风格、多一个板块，或者变成另一个应用，由 Agent 的洞察驱动。

Card 之外还有一套 design system 流程：从一段文字描述生成 UI 概念图，概念图生成主题模板，一个主题模板又能派生出字体、配色、布局的无穷变化。主题来自用户的偏好，或按用户的选择生成，然后应用到所有 App Card。

## 参与

每个仓库都有各自的 README 和构建说明。想看整体运行效果，从 OctoSense 仓库开始。
