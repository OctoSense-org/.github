# OctoSense

[English](README.md) | 简体中文

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
| Shell | [OctoSense-ROM](https://github.com/OctoSense-org/OctoSense-ROM) · [OctoSense-Desktop](https://github.com/OctoSense-org/OctoSense-Desktop) | 基于 [Makepad](https://github.com/OctoSense-org/makepad) 的 Agent 交互 Shell。OctoSense-ROM 的 `home/` 是手机 Shell，既可作为桌面应用安装，也可烧录进 ROM 镜像（OnePlus 6 上的 LineageOS）；OctoSense-Desktop 是桌面端 Shell。应用作为 Agent 的触点在其中运行。 |
| 语言 | [OctoScript](https://github.com/OctoSense-org/OctoScript) | 由 Makepad 的 Splash 演化而来、面向 Agent 需求优化的动态 DSL。无需编译即可实时解释执行应用逻辑并生成界面。用起来像 JavaScript，底座是 Rust。 |
| 渲染 | [OctoScript-Makepad](https://github.com/OctoSense-org/OctoScript-Makepad) · [OctoScript-Android](https://github.com/OctoSense-org/OctoScript-Android) · [OctoScript-OH](https://github.com/OctoSense-org/OctoScript-OH) | 把 OctoScript 渲染到 Makepad、Android 原生控件和 OpenHarmony ArkUI。 |
| 应用 | [OctoSense-System-Apps](https://github.com/OctoSense-org/OctoSense-System-Apps) | 系统自带应用（新闻、相册、地图、相机、邮件、AI 服务商），全部是受隔离约束的脚本应用；它们背后的宿主服务（邮件的 `mail`、AI 服务商的 `llm`）；以及原生的 AppCard 助手，Shell 只在明确要求时才构建它（`--features app-appcard`）。各个 Shell 固定引用这个仓库的版本，并选择要内置哪些应用。 |
| 应用商店 | [OctoSense-App-Hub](https://github.com/OctoSense-org/OctoSense-App-Hub) | 签名目录、准入检查、发布工具 `hub`，以及 `card-host`：按每个应用清单所申请的权限，把已安装的应用隔离运行。 |
| 应用开发 | [OctoScript-App-Design-Flow](https://github.com/OctoSense-org/OctoScript-App-Design-Flow) | 应用开发工具集：设计流程（文字描述或生成图 → 应用；Sketch 设计套件 → 主题套件）、可运行的模板、`octo` 命令行、脚本 API 参考，以及发布到 App Hub 的完整步骤。 |
| 内核 | [Octos](https://github.com/octos-org/octos) | 可嵌入的 Rust 原生 Agent harness。多轮交互、上下文与记忆、模型 provider、多 agent 并发、工具与沙箱、用户审批，全部通过 Octos UI Protocol（OUP）提供给上层应用。 |

相关仓库：[makepad](https://github.com/OctoSense-org/makepad)（所有 Shell 与渲染器固定引用的 Makepad 分支）· [makepad-html](https://github.com/OctoSense-org/makepad-html)（Makepad 的原生 HTML/CSS 渲染）· [OctoScript-website](https://github.com/OctoSense-org/OctoScript-website)（语言指南、组件目录、WASM 演示）· [robrix2](https://github.com/OctoSense-org/robrix2)（基于 Makepad 的 Matrix 客户端）· [octosense-org.github.io](https://github.com/OctoSense-org/octosense-org.github.io)（OctoSense 官网）。

一切都经由 Octoscript 动态执行，不需要编译，所以一个应用可以在几秒内换一种风格、多一个板块，或者变成另一个应用，由 Agent 的洞察驱动。

Card 之外还有一套 design system 流程：从一段文字描述生成 UI 概念图，概念图生成主题模板，一个主题模板又能派生出字体、配色、布局的无穷变化。主题来自用户的偏好，或按用户的选择生成，然后应用到所有 App Card。

## 开发 OctoSense 应用

任何人或编程 Agent 都可以为 OctoSense 开发应用，并发布到 App Hub。一个应用就是一个小包：`manifest.json` 声明所需权限，`main.splash` 是程序，再加上图片资源。应用在隔离环境中运行，并且从不收集密码：登录只在 OctoSense 自己的面板上进行。

参加 [Agentic App 黑客松](https://create.gosim.org/agenticapp26/)？从这里开始；赛事详情以黑客松页面为准。

### 从这里开始（约 5 分钟跑起一个应用，另需一次编译）

需要 Apple 芯片的 macOS（已验证的平台，其他平台尚未测试）、通过 [rustup](https://rustup.rs) 安装的 Rust stable（并把 `~/.cargo/bin` 加入 `PATH`）、Python 3.9 或更高版本、图形界面会话（应用在真实窗口中运行），以及约 1 GB 磁盘空间用于克隆开发工具仓库（可以用 `--depth 1`），另加编译产物。

```sh
mkdir octosense-ws && cd octosense-ws
git clone --depth 1 https://github.com/OctoSense-org/OctoScript-App-Design-Flow.git
git clone https://github.com/OctoSense-org/OctoSense-App-Hub.git
cd OctoScript-App-Design-Flow && python3 tools/setup-native.py      # 在旁边拉取固定版本的运行时
(cd ../OctoSense-App-Hub && cargo build --release -p octosense-card-host -p octosense-app-hub)
tools/octo doctor                                                    # 找到 hub 和 card-host，或给出修复方法
tools/octo new ~/apps/my-app --id my-notes --name "My Notes"
tools/octo run ~/apps/my-app/bundle --port 8141 --detach
```

后续步骤（修改、截图、`tools/octo check`、发布）见 [OctoScript-App-Design-Flow README](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/README.zh-CN.md)，耗时与最常见的坑见 [QUICKSTART](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/QUICKSTART.md)。应用能做什么由一份封闭的权限清单决定（[CAPABILITIES](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/CAPABILITIES.md)）；暂不支持把自己的应用包安装到手机上，演示请用运行器，或在 OctoSense-Desktop 中从本地目录安装。

**编程 Agent 请按顺序先阅读：** 任何 Agent 都可以（Codex、Claude Code、Cursor、Gemini CLI、GitHub Copilot），不用 Agent 也可以：每一步都是一条 shell 命令或一次文件修改，不依赖特定的 Agent、模型或厂商。

1. [OctoScript-App-Design-Flow `AGENTS.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/AGENTS.md)：规则、完成标准，以及哪些环节必须停下来请人确认。
2. [`flows/README.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/flows/README.md)：按起点选择设计流程（做应用：文字描述或生成的界面图；做主题套件：Sketch 设计套件），然后逐步执行该流程的 `FLOW.md`。
3. [`docs/QUICKSTART.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/QUICKSTART.md) 和 [`docs/SCRIPT-API.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/SCRIPT-API.md)：用 `tools/octo` 创建并运行应用；只使用文档中列出的 API。
4. [`docs/PUBLISHING.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/PUBLISHING.md) 以及 App Hub 的[发布规范](https://github.com/OctoSense-org/OctoSense-App-Hub/blob/main/docs/PUBLISHING.md)：打戳、截图、检查、签名、提交。

[OctoSense-System-Apps](https://github.com/OctoSense-org/OctoSense-System-Apps) 中的系统应用就是同样结构的完整示例。

## 参与

每个仓库都有各自的 README 和构建说明，并注明开发应用是否需要它。想看整体运行效果，从 OctoSense-ROM 和 OctoSense-Desktop 开始；OctoSense-Desktop 还能在发布前从本地目录安装并运行你自己的应用（[PUBLISHING §4](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/PUBLISHING.md#4-rehearse-the-store-path-locally)）。
