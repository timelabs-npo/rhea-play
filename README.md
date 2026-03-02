# Rhea Play

Native macOS operations centre for the Rhea multi-model AI advisory system. 12 panes, one window, everything at a glance.

## What it is

Rhea Play is a SwiftUI macOS app that gives operators a unified view into the Rhea system: live agent communication, tribunal debates, proof chains, knowledge graphs, task queues, and model health — all in a single window with keyboard navigation.

It connects to any Rhea Tribunal instance (local `localhost:8400` or cloud `rhea-tribunal.fly.dev`) and polls each endpoint independently so panes degrade gracefully when services are down.

## Layout (12 panes)

```
┌─────────────────────────────────────────────────────┐
│  RHEA  [RADIO][DIALOG][GOVERNOR][TASKS][PULSE]...   │  ← sidebar nav
├──────────┬──────────────────────────────────────────┤
│          │                                          │
│ Sidebar  │           Active Pane                   │
│          │                                          │
│  ⌘1 RADIO│  Live agent communication feed          │
│  ⌘2 DIALOG  Tribunal — submit claims, see debate   │
│  ⌘3 GOV  │  Token budgets, cost, provider status   │
│  ⌘4 TASKS│  Task queue with claim/complete/block   │
│  ⌘5 PULSE│  System heartbeat and health metrics    │
│  ⌘6 ATLAS│  3D knowledge graph (Three.js via WKWebView) │
│  ⌘7 HIST │  SQL-backed session history browser     │
│  ⌘8 ALETH│  Immutable proof chain browser          │
│  ⌘9 RULI │  Ontology explorer — hypothesis lifecycle│
│  ⌘0 PROCS│  Running process monitor                │
│  ⌘- MODEL│  Available model registry across providers│
│  ⌘= NDI  │  Video transport status (libndi v6.2.0) │
│  ⌘, CONF │  Settings — API URL, auth, preferences  │
└──────────┴──────────────────────────────────────────┘
```

## Keyboard Shortcuts

| Shortcut | Pane     |
|----------|----------|
| `⌘1`     | Radio    |
| `⌘2`     | Dialog   |
| `⌘3`     | Governor |
| `⌘4`     | Tasks    |
| `⌘5`     | Pulse    |
| `⌘6`     | Atlas    |
| `⌘7`     | History  |
| `⌘8`     | Aletheia |
| `⌘9`     | Ruliad   |
| `⌘0`     | Procs    |
| `⌘-`     | Models   |
| `⌘=`     | NDI      |
| `⌘,`     | Config   |

Menu bar icon shows aggregate agent health at a glance.

## Build

### Prerequisites

- macOS 14.0+
- Xcode 15+ (or Xcode 16 beta for xcodeVersion 26.2)
- [xcodegen](https://github.com/yonaskolb/XcodeGen): `brew install xcodegen`

### Steps

```bash
# 1. Clone
git clone https://github.com/timelabs-npo/rhea-play.git
cd rhea-play

# 2. Generate the Xcode project (not committed — generated on demand)
xcodegen generate

# 3. Build (Debug)
xcodebuild -scheme RheaPlay -configuration Debug build

# 4. Build (Release)
xcodebuild -scheme RheaPlay -configuration Release build
```

Or open `RheaPlay.xcodeproj` in Xcode after step 2 and run from there.

Pre-built DMG is available in [Releases](https://github.com/timelabs-npo/rhea-project/releases).

## Dependencies

| Package | Purpose |
|---------|---------|
| [RheaKit](packages/RheaKit/) | Shared SwiftUI views and API layer (local package, also used by the iOS app) |
| [Pow](https://github.com/serg-alexv/Pow) | Physics-based SwiftUI transition effects |
| [GRDB.swift](https://github.com/groue/GRDB.swift) | SQLite — history browser, local task cache |
| [swift-collections](https://github.com/apple/swift-collections) | OrderedDictionary for pane registry |
| [KeychainAccess](https://github.com/kishikawakatsumi/KeychainAccess) | JWT token storage |
| [swift-markdown-ui](https://github.com/gonzalezreal/swift-markdown-ui) | Markdown rendering in Dialog pane |
| [Starscream](https://github.com/daltoniam/Starscream) | WebSocket for live Radio feed |

RheaKit is included as a local package at `packages/RheaKit/` — no separate clone needed.

## Architecture

SwiftUI + `NavigationSplitView`. Each pane is a self-contained view that polls its own Tribunal API endpoint. Panes are registered as an enum (`Pane`) with associated icon, label, and keyboard shortcut.

The app talks to one configurable base URL (default: `localhost:8400`). Every pane constructs its own URLSession requests — no shared data layer beyond `RheaStore` for auth state.

NDI pane requires `libndi` v6.2.0 installed locally at `/usr/local/lib`. On systems without it the pane shows a graceful "requires local server" message.

## Part of the Rhea ecosystem

- **Tribunal API** — backend at [timelabs-npo/rhea-project](https://github.com/timelabs-npo/rhea-project) (Python, Fly.io)
- **iOS app** — RheaPreview on TestFlight sharing the same RheaKit package
- **TimeLabs NPO** — [timelabs-npo](https://github.com/timelabs-npo)

## License

MIT — see [LICENSE](LICENSE).
