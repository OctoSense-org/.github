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
| Shell | [OctoSense-ROM](https://github.com/OctoSense-org/OctoSense-ROM) · [OctoSense-Desktop](https://github.com/OctoSense-org/OctoSense-Desktop) | The agent shell, built on [Makepad](https://github.com/OctoSense-org/makepad). OctoSense-ROM's `home/` is the phone shell, installed either as a Home app or burned into the ROM image (LineageOS on the OnePlus 6); OctoSense-Desktop is the desktop shell. Apps run inside it as touchpoints of the agent. |
| Language | [OctoScript](https://github.com/OctoSense-org/OctoScript) | A dynamic DSL evolved from Makepad's Splash and tuned for agents. Interprets app logic and generates UI in real time with no compile step. JavaScript-like on the surface, Rust underneath. |
| Renderers | [OctoScript-Makepad](https://github.com/OctoSense-org/OctoScript-Makepad) · [OctoScript-Android](https://github.com/OctoSense-org/OctoScript-Android) · [OctoScript-OH](https://github.com/OctoSense-org/OctoScript-OH) | Render OctoScript to Makepad, native Android widgets, and OpenHarmony ArkUI. |
| Apps | [OctoSense-System-Apps](https://github.com/OctoSense-org/OctoSense-System-Apps) | The first-party apps (News, Photos, Maps, Camera, Mail) as contained script apps, plus the AppCard assistant. The shells pin this repository and choose which apps to ship. |
| App store | [OctoSense-App-Hub](https://github.com/OctoSense-org/OctoSense-App-Hub) | The signed catalog, the admission gate, the `hub` publishing tool and `card-host`, the runner that contains every installed app under the permissions its manifest asks for. |
| Building apps | [OctoScript-App-Design-Flow](https://github.com/OctoSense-org/OctoScript-App-Design-Flow) | The app development harness: design flows (brief, sketch or generated image → app), a runnable template, the `octo` CLI, the script API reference and the path to publishing on the App Hub. |
| Kernel | [Octos](https://github.com/octos-org/octos) | An embeddable, Rust-native agent harness. Multi-turn interaction, context and memory, model providers, concurrent agents, tools, sandboxing and user approval, exposed to apps through the OS UI protocol. |

Because everything runs through Octoscript without a compile step, an app can change style, gain a section, or become a new app within seconds, driven by what the agent has learned.

A design system sits alongside the cards. From a written brief, generative AI produces a UI concept, the concept becomes a theme kit, and a theme kit can produce endless variations in font, color and layout. Themes are chosen from what a user likes, or generated from their choice, and applied to every App Card.

## Build an OctoSense app

Anyone, person or coding agent, can build an app for OctoSense and publish it on the App Hub. An app is a small bundle: a `manifest.json` that asks for the permissions it needs, a `main.splash` program, and its artwork. It runs contained, and it never collects a password: sign-in happens on OctoSense's own sheet.

**Coding agents: read these first, in order.**

1. [OctoScript-App-Design-Flow `AGENTS.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/AGENTS.md): the rules, the definition of done, and where to stop and ask a person.
2. [`flows/README.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/flows/README.md): pick the design flow for what you start from (a text brief, a sketch, a generated image), then follow that flow's `FLOW.md` step by step.
3. [`docs/QUICKSTART.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/QUICKSTART.md) and [`docs/SCRIPT-API.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/SCRIPT-API.md): build and run the app with `tools/octo`; use only documented APIs.
4. [`docs/PUBLISHING.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/PUBLISHING.md), with the App Hub's [publishing contract](https://github.com/OctoSense-org/OctoSense-App-Hub/blob/main/docs/PUBLISHING.md): stamp, screenshot, check, sign and submit.

The first-party apps in [OctoSense-System-Apps](https://github.com/OctoSense-org/OctoSense-System-Apps) are complete examples of the same shape.

## Get involved

Each repository has its own README with build instructions. OctoSense-ROM and OctoSense-Desktop are the places to see it all running.
