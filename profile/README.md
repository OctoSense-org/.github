# OctoSense

**An agent shell on top of your operating system.**

OctoSense is a layer that runs on Windows, macOS, Linux, Android, iOS and HarmonyOS. It looks like the launcher and the apps you already know, and it behaves like an agent that understands what you want, senses what is changing around you, and reshapes those apps before you ask.

[中文说明](README.zh-CN.md)

## Why not another chat window?

Today's agent interfaces are chat windows. Whatever they render inside a message, the interaction is a linear, one-way stream of text. That is a return to the terminal era, before the desktop, the mouse and multi-touch made computing efficient and familiar. Users have decades of habit invested in those interfaces, and switching them to a text prompt costs more than it gives.

OctoSense takes the other road. It keeps the interaction patterns people already know, and puts the agent inside the apps instead of asking people to leave the apps for a chat box.

## What OctoSense does

- **Familiar entry points.** News, weather, markets, travel, video, shopping. Each stays a stable place to start, the way a launcher works today, and you can still operate any app directly.
- **Intent-driven content.** Behind each entry point, the content is generated and organized around your intent, preferences, and context. A travel entry pulls in weather, routes, hotels, tickets and what to do there. A news entry aggregates what you actually follow.
- **Proactive, not passive.** The agent is driven by time and events, not only by your questions. A cold front, a storm that delays flights, a strike at your destination, a change in a child's school schedule, or a family member's health signal can all reprompt the apps to reorganize, suggest, and prepare.
- **Less, not more.** Super apps are built for a billion people and carry everything anyone might need. OctoSense trims each app down to what you need, generating and continuously evolving it instead of shipping one fixed interface to everyone.
- **Human in the loop.** The important decisions stay with you. The agent distills the day into the few things that need your confirmation, and once you confirm, everything downstream updates.
- **Feeling as well as function.** Style, color, typography and mood adapt to the person and the moment: morning and evening, summer and winter, the turn of the seasons. Like an octopus, it senses and changes color.

Every app you see is a touchpoint. Behind all of them is one agent with one memory, one evolving understanding of your world, and the data you choose to let it see: public signals like news, location and weather, and private ones like mail, messages, calendars and wearable data.

## How it is built

| Layer | Project | Role |
| --- | --- | --- |
| Shell | [OctoSense](https://github.com/OctoSense-org/OctoSense) | The cross-platform agent shell, built on [Makepad](https://github.com/OctoSense-org/makepad). Hosts apps as touchpoints of the agent. |
| Language | [Octoscript](https://github.com/OctoSense-org/Octoscript) | A dynamic DSL evolved from Makepad's Splash and tuned for agents. Interprets app logic and generates UI in real time with no compile step. JavaScript-like on the surface, Rust underneath. |
| Renderers | [Octoscript-Makepad](https://github.com/OctoSense-org/Octoscript-Makepad) · [Octoscript-Android](https://github.com/OctoSense-org/Octoscript-Android) · [Octoscript-OH](https://github.com/OctoSense-org/Octoscript-OH) | Render Octoscript to Makepad, native Android widgets, and OpenHarmony ArkUI. |
| App Cards | [Octoscript-AppCard](https://github.com/OctoSense-org/Octoscript-AppCard) | Composable, embeddable app templates and applets, built with Octoscript. Cards nest inside cards and flows, and generated cards can become templates for the next ones. |
| Kernel | [octos](https://github.com/ymote/octos) | An embeddable, Rust-native agent harness. Multi-turn interaction, context and memory, model providers, concurrent agents, tools, sandboxing and user approval, exposed to apps through the OS UI protocol. |

Because everything runs through Octoscript without a compile step, an app can change style, gain a section, or become a new app within seconds, driven by what the agent has learned.

A design system sits alongside the cards. From a written brief, generative AI produces a UI concept, the concept becomes a theme kit, and a theme kit can produce endless variations in font, color and layout. Themes are chosen from what a user likes, or generated from their choice, and applied to every App Card.

## Get involved

Each repository has its own README with build instructions. OctoSense is the place to see it all running.
