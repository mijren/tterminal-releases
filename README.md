# tterminal

Native desktop command center for AI-assisted developers.

tterminal brings terminals, project commands, AI coding agents, live process
monitoring, files, diffs, and an inspector-aware browser into one keyboard-driven
workspace.

## Download

Current public beta: `v0.6.2`

- [Download for macOS Apple Silicon](https://github.com/mijren/tterminal-releases/releases/latest/download/tterminal-macos-aarch64.dmg)
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

Current public releases are Apple Developer ID signed and notarized. macOS may
still show a normal first-launch confirmation for apps downloaded outside the App
Store.
