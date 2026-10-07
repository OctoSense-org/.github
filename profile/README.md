# OctoSense

<img src="https://raw.githubusercontent.com/OctoSense-org/.github/main/profile/logo.svg" width="96" height="96" alt="OctoSense" />

English | [简体中文](README.zh-CN.md)

**An agent shell on top of your operating system.**

OctoSense is a layer built to run on Windows, macOS, Linux, Android, iOS and HarmonyOS. It looks like the launcher and the apps you already know, and it behaves like an agent that understands what you want, senses what is changing around you, and reshapes those apps before you ask.

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
| Shell | [OctoSense](https://github.com/OctoSense-org/OctoSense) | The agent shell, built on [Makepad](https://github.com/OctoSense-org/makepad), in one repository: the shell crate, its services (the octos kernel service, the app-agent broker, AI providers), the system apps, and both packagings. `desktop/` is the desktop shell; `phone/` is the phone shell, **OctoSense Home**, installed as a Home app on any Android phone; `rom/` builds the ROM image that preinstalls it (LineageOS on the OnePlus 6). Apps run inside it as touchpoints of the agent. |
| Language | [OctoScript](https://github.com/OctoSense-org/OctoScript) | A dynamic DSL evolved from Makepad's Splash and tuned for agents. Interprets app logic and generates UI in real time with no compile step. JavaScript-like on the surface, Rust underneath. |
| Renderers | [OctoScript-Makepad](https://github.com/OctoSense-org/OctoScript-Makepad) · [OctoScript-Android](https://github.com/OctoSense-org/OctoScript-Android) · [OctoScript-OH](https://github.com/OctoSense-org/OctoScript-OH) | Render OctoScript to Makepad, native Android widgets, and OpenHarmony ArkUI. |
| Apps | [OctoSense `apps/`](https://github.com/OctoSense-org/OctoSense/tree/main/apps) | The first-party apps (News, Photos, Maps, Camera, Mail and AI providers) as contained script apps, the host services behind them (Mail's `mail`, AI providers' `llm`), and the native AppCard assistant, which the shells build only when asked (`--features app-appcard`). They ship inside the shells, versioned with them. Formerly OctoSense-System-Apps (archived). |
| App store | [OctoSense-App-Hub](https://github.com/OctoSense-org/OctoSense-App-Hub) | The signed catalog, the admission gate, the `hub` publishing tool, and `card-host`, a development runner that runs one bundle with only the permissions its manifest asks for. |
| Building apps | [OctoScript-App-Design-Flow](https://github.com/OctoSense-org/OctoScript-App-Design-Flow) | The app development harness: design flows (a text brief or a generated image → app; a Sketch design kit → a theme kit), a runnable template, the `octo` CLI, the script API reference and the path to publishing on the App Hub. |
| Kernel | [Octos](https://github.com/octos-org/octos) | An embeddable, Rust-native agent harness. Multi-turn interaction, context and memory, model providers, concurrent agents, tools, sandboxing and user approval, exposed to apps through the Octos UI Protocol (OUP). |

Related: [makepad](https://github.com/OctoSense-org/makepad) (the Makepad fork every shell and renderer pins) · [makepad-html](https://github.com/OctoSense-org/makepad-html) (native HTML/CSS rendering for Makepad) · [OctoScript-website](https://github.com/OctoSense-org/OctoScript-website) (language guides, component catalog, WASM demos) · [robrix2](https://github.com/OctoSense-org/robrix2) (Matrix client on Makepad) · [octosense-org.github.io](https://github.com/OctoSense-org/octosense-org.github.io) (the OctoSense website).

Because everything runs through OctoScript without a compile step, an app can change style, gain a section, or become a new app within seconds, driven by what the agent has learned.

A design system sits alongside the cards. From a written brief, generative AI produces a UI concept, the concept becomes a theme kit, and a theme kit can produce endless variations in font, color and layout. Themes are chosen from what a user likes, or generated from their choice, and applied to every App Card.

## Build an OctoSense app

Anyone, person or coding agent, can build an app for OctoSense and publish it on the App Hub. An app is a small bundle: a `manifest.json` that asks for the permissions it needs, a `listing.json` for the store, a `main.splash` program, and its artwork. It runs contained and never collects a password: OctoSense handles sign-in for it.

Taking part in the [Agentic App Hackathon](https://create.gosim.org/agenticapp26/?lang=en)? This is the place to start; the hackathon page has the event details.

### Start here (about 5 minutes to a running app, plus one build)

You need macOS on Apple silicon (the verified platform; others are untested), Rust stable via [rustup](https://rustup.rs) with `~/.cargo/bin` on `PATH`, Python 3.9 or newer, a graphical session (the app runs in a real window), and about 1 GB of disk for the harness clone (`--depth 1` is fine), plus the build output.

```sh
mkdir octosense-ws && cd octosense-ws
git clone --depth 1 https://github.com/OctoSense-org/OctoScript-App-Design-Flow.git
git clone https://github.com/OctoSense-org/OctoSense-App-Hub.git
cd OctoScript-App-Design-Flow && python3 tools/setup-native.py      # adds the pinned runtime beside it
(cd ../OctoSense-App-Hub && cargo build --release -p octosense-card-host -p octosense-app-hub)
tools/octo doctor                                                    # finds hub and card-host, or says how to fix it
tools/octo new ~/apps/my-app --platform macos --id my-notes --name "My Notes"
tools/octo run ~/apps/my-app/bundle --port 8141 --detach
```

The [OctoScript-App-Design-Flow README](https://github.com/OctoSense-org/OctoScript-App-Design-Flow#quick-path) continues from there (edit, screenshot, `tools/octo check`, publish) and [QUICKSTART](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/QUICKSTART.md) has timings and the gotchas that cost the most time. What an app may do is a closed list of capabilities ([CAPABILITIES](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/CAPABILITIES.md)); installing your own bundle on a phone is not supported yet, so demo it in `card-host`, or from a local catalog in an OctoSense desktop shell built from source.

**Coding agents: read these first, in order.** Any coding agent works (Codex, Claude Code, Cursor, Gemini CLI, GitHub Copilot), and so does working by hand: every step is a shell command or a file edit, with no dependency on a particular agent, model or vendor.

1. [OctoScript-App-Design-Flow `AGENTS.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/AGENTS.md): the rules, the definition of done, and where to stop and ask a person.
2. [`flows/README.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/flows/README.md): pick the design flow for what you start from (a text brief or a generated image for an app; a Sketch design kit for a theme kit), then follow that flow's `FLOW.md` step by step.
3. [`docs/QUICKSTART.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/QUICKSTART.md) and [`docs/SCRIPT-API.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/SCRIPT-API.md): build and run the app with `tools/octo`; use only documented APIs.
4. [`docs/PUBLISHING.md`](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/PUBLISHING.md): finish the manifest and listing, capture screenshots and pass the gate. App Hub's [publishing reference](https://github.com/OctoSense-org/OctoSense-App-Hub/blob/main/docs/PUBLISHING.md) has every rule.
5. App Hub's [`docs/SUBMITTING.md`](https://github.com/OctoSense-org/OctoSense-App-Hub/blob/main/docs/SUBMITTING.md): sign, freeze the release and open the submission issue, step by step. Its three reference apps, GitHub Notes, Inbox Assistant and Google Calendar, are complete submissions to learn from.

The first-party apps in [OctoSense `apps/`](https://github.com/OctoSense-org/OctoSense/tree/main/apps) are complete examples of the same shape (each app's `bundle/`). For an app that uses a person's GitHub, Gmail or Google Calendar account, start from Design Flow's [connected-apps examples](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/tree/main/examples/connected-apps), the development copies of the three reference apps.

## Get involved

Each repository has its own README with build instructions and says whether an app builder needs it. [OctoSense](https://github.com/OctoSense-org/OctoSense) is the place to see it all running, on the desktop or a phone; its desktop shell can also install and run your own app from a local catalog before it is published ([PUBLISHING §4](https://github.com/OctoSense-org/OctoScript-App-Design-Flow/blob/main/docs/PUBLISHING.md#4-rehearse-the-store-path-locally)).
