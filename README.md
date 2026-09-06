# Rhea Play

**Keep the whole argument in view.**

Rhea Play is a native macOS operations application for the Rhea agent ecosystem. Agent messages, competing model answers, tasks, budgets, history, research records, and service health share one window and a keyboard-driven navigation rail.

An agent says the work is finished. The queue still shows it open. The budget moved, but the expected record never arrived. Those are useful disagreements. Play brings their separate views close enough that an operator can follow the trail.

**Current form:** SwiftUI client source with 12 operational panes plus Config, a menu-bar interface, and a bundled RheaKit package. Service data and actions depend on the connected backend. The window shows one selected pane at a time.

## Thirteen places to look

| Shortcut | Pane | View into the system |
|---|---|---|
| `⌘1` | Radio | Agent communication |
| `⌘2` | Dialog | Tribunal questions and responses |
| `⌘3` | Governor | Budgets and provider state |
| `⌘4` | Tasks | Task queue and actions |
| `⌘5` | Pulse | Health and activity |
| `⌘6` | Atlas | Embedded web visualization |
| `⌘7` | History | Recorded sessions |
| `⌘8` | Aletheia | Research and proof-related records |
| `⌘9` | Ruliad | Ontologies and hypotheses |
| `⌘0` | Procs | Process/session views and controls |
| `⌘-` | Models | Provider/model information |
| `⌘=` | NDI | Media-service status and controls |
| `⌘,` | Config | Connection and preferences |

The list is defined by [PlayShell.Pane](Sources/PlayApp.swift). A record displayed under “proof” is still a record from a service; the pane's name does not establish its truth or immutability.

## Follow the signal

```text
Selected pane ↔ Rhea service endpoints
      ↑
Shared RheaKit state + pane-specific requests
      ↓
Window / navigation / menu-bar view
```

[RheaStore](packages/RheaKit/Sources/RheaKit/RheaStore.swift) refreshes shared state. [RheaAPI](packages/RheaKit/Sources/RheaKit/RheaAPI.swift) provides service calls; some views also make their own requests. [PlayApp.swift](Sources/PlayApp.swift) contains the shell and additional pane implementations.

This is where the map of a system becomes useful: put a message next to its task, a task next to its cost, and a claimed result next to its record. Their relationship is an investigation path, not automatic proof that one caused the other.

NDI status, discovery, and test-pattern controls call backend endpoints under `/cc/ndi`. **NDI runtime availability belongs to that server.** Installing a media library on the Mac does not by itself establish an operational media service.

## Build from source

Use Xcode, XcodeGen, and a macOS SDK supporting the macOS 14 deployment target. [project.yml](project.yml) records the project settings; it currently names Xcode 26.2 and Swift 5.9. [RheaKit's manifest](packages/RheaKit/Package.swift) lists its dependencies.

```bash
xcodegen generate
xcodebuild -scheme RheaPlay -configuration Debug \
  -derivedDataPath /tmp/rhea-play-build \
  CODE_SIGNING_ALLOWED=NO build
```

The generated project is `RheaPlay.xcodeproj`. Open it in Xcode to inspect or run the app. This is a local build entrance; distribution additionally requires your signing and provisioning configuration.

## Know which server is on screen

[AppConfig](packages/RheaKit/Sources/RheaKit/AppConfig.swift) defaults macOS to `https://rhea-tribunal.fly.dev`. Localhost is the conditional default for the iOS simulator, not for this Mac app.

The current startup migration also replaces saved localhost/private-network addresses with the cloud address on non-simulator launches. Config offers an API address field, but a local selection should not be assumed to survive a relaunch. Confirm the displayed target before using service controls.

The horizon is a control room that makes contradictions harder to overlook. First, make every indicator answerable to its source.

## The surrounding system

Start at [the Rhea family entrance](https://blueshoes.space/rhea/).

- [Rhea / Tribunal](https://github.com/timelabs-npo/rhea-project) contains the coordination and service work.
- [Rhea Atlas](https://github.com/timelabs-npo/rhea-atlas) develops the browser instrument.
- [Rhea iOS](https://github.com/timelabs-npo/rhea-ios) carries related SwiftUI views to the phone.
- [Rhea CLI](https://github.com/timelabs-npo/rhea-cli) provides the terminal command surface.

MIT — see [LICENSE](LICENSE).
