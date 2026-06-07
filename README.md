# tterminal

Native desktop command center for AI-assisted developers.

tterminal brings terminals, project commands, AI coding agents, live process
monitoring, files, diffs, and an inspector-aware browser into one keyboard-driven
workspace.

## Download

Current public beta: `v0.6.1`

- [Download for macOS Apple Silicon](https://github.com/mijren/tterminal-releases/releases/latest/download/tterminal_0.6.1_aarch64.dmg)
- [View all releases](https://github.com/mijren/tterminal-releases/releases)

The app is not distributed through the App Store.

## Online Updates

Starting with `v0.6.0`, tterminal includes signed online updates through Tauri's
updater.

Users on builds before `v0.6.0` must install `v0.6.0` manually once. After that,
future versions can update over the internet.

Updater manifest:

```text
https://github.com/mijren/tterminal-releases/releases/latest/download/latest.json
```

## Platform Status

| Platform | Status |
| --- | --- |
| macOS Apple Silicon | Public beta |
| macOS Intel | Planned |
| Linux | v1.x roadmap |
| Windows | v2 roadmap |

## Privacy

- No in-app analytics.
- No terminal content, command output, environment variables, or file paths are
  collected for analytics.
- GitHub Releases may report download counts.
- Crash reporting, when added, should be opt-in and scrub terminal/project data.

## macOS Gatekeeper

The current beta is not Apple Developer ID signed/notarized yet. macOS may show a
Gatekeeper warning on first launch until signing and notarization are added to the
release pipeline.
