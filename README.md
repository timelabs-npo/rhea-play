# Rhea Play

Native macOS operations centre for the Rhea multi-model AI system. 12 panes, one window, everything at a glance.

## Panes

| Pane | What it shows |
|------|--------------|
| Radio | Live agent communication feed |
| Dialog | Interactive tribunal — submit claims, see debate |
| Governor | Token budgets, cost tracking, provider status |
| Tasks | Persistent task queue with claim/complete/block |
| Pulse | System heartbeat and health metrics |
| Atlas | 3D knowledge graph visualization |
| History | SQL-backed session history browser |
| Aletheia | Immutable proof chain browser |
| Ruliad | Ontology explorer — hypothesis lifecycle |
| Procs | Running process monitor |
| Models | Available model registry across providers |
| NDI | Video transport status (libndi v6.2.0) |

## Build

```bash
xcodegen generate
xcodebuild -scheme RheaPlay -configuration Release build
```

Or download the DMG from [Releases](https://github.com/timelabs-npo/rhea-project/releases).

## Keyboard Shortcuts

`⌘1` through `⌘=` switches panes. Menu bar icon shows aggregate agent health.

## Architecture

SwiftUI + `NavigationSplitView`. Each pane is a self-contained view polling its API endpoint. Shares `RheaKit` package with the iOS app.

Connects to any Rhea Tribunal instance — local (`localhost:8400`) or cloud (`rhea-tribunal.fly.dev`).

Part of [TimeLabs NPO](https://github.com/timelabs-npo) open infrastructure.

## License

MIT
