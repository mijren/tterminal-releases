---
title: Changelog
description: Product updates, release highlights, and upgrade notes for tterminal.
---

# Changelog

Notable changes to tterminal are collected here for public release notes and
website pages.

Release entries should be written for users first: explain what changed, why it
matters, and whether they need to do anything after updating.

## Unreleased

No unreleased changes yet.

## v0.7.2

_Release date: 2026-06-11_

### Highlights

- Agent availability now lives in App Settings as a global library. Projects can
  add as many reusable or project-only agent entries as they need without
  managing separate per-agent enable toggles.
- Project Settings now keeps configuration sync controls in the Configuration
  file section instead of showing a large sync banner on every settings page.

### Improvements

- The Add Agent picker now lists globally enabled agents directly and includes
  quick links for project-only custom agents and the global agent library.
- Project AI agent settings now show only agents saved for that project, with an
  explicit empty state after all project agents are removed.
- Deleting every project agent now persists as an empty project agent list
  instead of repopulating from detected or proposed defaults.

### Upgrade Notes

- No action required after updating.

## v0.7.1

_Release date: 2026-06-11_

### Highlights

- Command palette navigation now starts with active terminals, so `Command-K`
  can jump directly to running or starting panes.
- The focused pane appears first in the Active terminals section, making it easy
  to confirm where you are before switching elsewhere.

### Fixes

- Project sidebar reordering now uses a full dashed drop placeholder so the
  destination is clear while dragging.
- Sidebar section arrows and add buttons now only expand, collapse, or add; they
  no longer navigate the workspace when clicked.
- Project settings and agent selections are saved more reliably across machines
  without interrupting typing or clearing user-entered values.

### Upgrade Notes

- No action required after updating.

## v0.7.0

_Release date: 2026-06-11_

### Highlights

- Improved the Files editor so opened files keep the intended CodeMirror layout,
  syntax colors, gutters, and editor styling in the packaged desktop app.
- Project launchers now restore more reliably after reopening a project,
  including agents that were already assigned to that project.
- New terminals and newly saved commands now open directly into their running
  pane instead of leaving you on the section card grid.

### Fixes

- Fixed file previews that could render as plain line numbers followed by
  unstyled text when the editor's runtime styles were not available quickly
  enough.
- Fixed configured project agents not appearing after removing and re-adding or
  reopening a project.
- Fixed the Terminals section "+" action so the new terminal becomes the active
  workspace pane immediately.
- Fixed the Commands "Save command" flow so the newly saved command starts and
  opens immediately.
- Fixed project reordering in the project rail and sidebar so dragging a project
  no longer triggers the central file-drop overlay.

### Upgrade Notes

- No action required after updating.

## v0.6.8

_Release date: 2026-06-09_

### Highlights

- Improved launcher and pane focus behavior so starting, stopping, and
  returning to commands or agents keeps you on the pane you were working with.
- Project Settings now has clearer save state controls, including manual
  refresh and Save now actions for fresh or empty projects.

### Improvements

- Agent grid card right-click menus now match the sidebar actions for renaming
  and deleting panes.
- Renaming an agent from its grid card now opens the same in-app rename dialog
  used by the sidebar.
- Browser shortcuts use the URL as a reliable identity, so editing or reloading
  shortcuts is less likely to create duplicate or blank entries.
- The terminal "Bottom" button now detects both xterm buffer position and the
  actual viewport scroll position, so it appears more reliably when you land in
  the middle of a long agent or terminal session.

### Fixes

- Fixed sidebar Start/Stop flows that could leave a command showing stale
  stopped-session text until it was clicked again.
- Fixed stopped commands or agents unexpectedly returning focus to the agent
  section instead of staying on the pane that was just stopped.
- Fixed browser shortcut creation on fresh projects when the shortcut label was
  empty or derived from the URL.
- Fixed project settings saves that could stay stuck on "saving" for empty
  projects before a config file existed.

### Upgrade Notes

- No action required after updating.

## v0.6.7

_Release date: 2026-06-09_

### Highlights

- Section cards (Terminals, Commands, Agents, Browser shortcuts) are now live
  tiles. Each card shows a preview of the program's most recent output in a
  built-in console strip, plus a CPU gauge ring and live memory use, so you can
  see what every process is doing at a glance without opening it.

### Features

- Section cards are color-coded by type — agents violet, terminals mint,
  commands amber, browser shortcuts blue — across the top accent, glyph, status
  dot, and CPU ring. The "New ..." card is pinned first in every section and
  takes on the same type color when you hover it.
- Todo tags are now normalized and reusable, so tags you have used before can be
  reused across todos for quicker, more consistent tagging.
- The todo modal's title is now a multi-line text area (it doubles as the
  description); long titles truncate to two lines on the board, with the full
  text shown when you open the card.

### Improvements

- The active and animated "live" borders now appear only on the projects rail
  (the leftmost column), where the active project already shows an accent border,
  activity bar, and pulsing live ring. The project sidebar no longer duplicates
  these cues: the keyboard-focus glow on its edge and the animated border around
  the project name at the top have been removed, leaving a single calm divider.
- The card output preview always renders on a dark console surface, so it stays
  readable even when you use a light terminal color theme. Section grids show
  four cards per row at a consistent card size matching the scratchpad library.
- The Kanban board now updates immediately when you add a todo from the sidebar
  "+" button, instead of needing a project switch before the new card appears.
- Dismissing the new-todo dialog without typing anything no longer leaves a
  blank card behind; todo descriptions can also be left blank when you want one.
- Todo board columns now scroll independently, so cards keep their full height
  instead of shrinking as a column fills up.
- Completed todos are no longer listed in the sidebar todo section, keeping it
  focused on open work.
- Moving through the sidebar with the arrow keys now carries a single highlight
  with the cursor, instead of leaving the previously clicked item highlighted as
  well.

## v0.6.6

_Release date: 2026-06-08_

### Highlights

- Added a Kanban-style todo board so project todos can be scanned, filtered,
  edited, and moved through workflow columns without leaving the board.
- Added a startup splash screen so tterminal shows a branded loading state while
  the application finishes opening.
- Sidebar section headers (Terminals, Commands, Agents, Browser shortcuts) now
  open a card grid that summarizes everything in the section at a glance. Start,
  stop, or restart each item right from its card, click a card to jump straight
  to it, and use the "New ..." card to create one without digging through menus.

### Features

- Todo cards now support tags and priority metadata.
- Todo cards open in an in-board modal for creating and editing title, status,
  notes, tags, priority, and acceptance criteria.
- Sidebar todo clicks now open the Kanban board and focus the selected todo in
  the modal instead of switching to a standalone editor.
- Kanban cards can be moved between Todo, In Progress, Blocked, and Done
  columns with pointer-based drag movement.
- Kanban card context menus now support copying a todo link and deleting the
  card from the board.
- MCP todo tools now support board review, direct tag replacement, and
  acceptance-criteria checklist creation for agent-created todos.

### Improvements

- Redesigned the light theme ("Arctic Clean") with a cooler blue-white surface
  palette, more visible separators, deeper text, and a vivid blue accent so the
  interface reads cleaner and more legible in light mode. Dark mode is
  unchanged.
- Refreshed the todo board look: a gradient hero with a completion progress
  bar and dotted count chips, status-tinted columns, cards with a state-colored
  left stripe, priority pills, a hover lift, and a dedicated blocked banner, plus
  a polished create/edit modal with an icon header.
- Reworked scratchpads into a filterable library: opening Scratchpads now shows
  a grid of every pad with a content preview, author, and edit time, plus search,
  an All/Mine/Agents filter, and sorting. Right-click a card to copy its link or
  delete it. The single-pad editor gets a fresh full-width header and a centered
  "paper" writing column.
- Set the native window and webview startup background to match the splash,
  preventing a white flash before the app UI renders.
- Todo board styling now follows the active light or dark theme.
- App Settings now describes the expanded Kanban todo MCP surface and todo
  sidebar behavior.
- Unified the to-do icon across the app: the sidebar, settings, add menu, and
  todo board now all use the same rounded-checkbox icon instead of three
  different glyphs.
- Enlarged the sidebar section icons so each section reads more clearly in the
  spine.
- Running items in the new section grids stand out with a live accent stripe and
  a glowing status dot, so active terminals, commands, and agents are easy to
  tell apart from idle ones.
- Softened the card hover shadow in light mode so hovering no longer looks like a
  heavy black drop shadow.

### Fixes

- Fixed todo card movement so dragged cards stay visible above columns and keep
  the grabbed point aligned with the pointer.
- Fixed todo card clicks after opening a todo from the sidebar so board cards
  continue to open their edit modal reliably.
- Fixed agent and command completion alerts so in-app notifications appear even
  while tterminal is focused, including completions from other open projects.

### Upgrade Notes

- No action required after updating.

## v0.6.5

_Release date: 2026-06-07_

### Highlights

- Added a user preference for automatic update checks.
- Improved agent launch behavior so newly added agents open directly in their
  workspace pane.
- Added pane-scoped agent renaming from the sidebar.
- Improved live project indicators for active panes, including custom project
  icons.

### Improvements

- Background update checks can now be disabled from App Settings.
- Manual update checks remain available even when automatic checks are disabled.
- Active project animations now use quieter borders and reduced glow.

### Fixes

- Fixed agent rename behavior by replacing the native prompt with an in-app
  rename dialog.
- Fixed live project dots that could disappear behind custom project images.

### Upgrade Notes

- Automatic update checks remain enabled by default. You can disable them from
  App Settings while keeping manual update checks available.

## v0.6.4

_Release date: TBD_

### Highlights

- Native macOS desktop command center for AI-assisted development.
- Multi-pane terminal workspace for shells, project commands, and AI agent CLIs.
- Agent presets for Claude, Codex, Cursor, Aider, Copilot, and custom tools.
- Project rail and sidebar for switching projects, launching panes, and tracking
  live activity.
- Files, diffs, scratchpads, todos, and local MCP tools in one workspace.
- Signed update flow using GitHub Releases and Tauri's updater.

### Features

- Start, stop, restart, and focus terminal panes from the sidebar.
- Add multiple agent instances per project and distinguish them by pane label.
- Monitor live CPU and memory usage for running panes.
- Open project files and inspect git changes without leaving the app.
- Store shared project commands in `tterminal.config.json`.
- Keep local-only app settings and project state on the user's device.

### Platform

- macOS Apple Silicon public beta.
- Distributed outside the Mac App Store through signed and notarized builds.
- Public release assets are published through GitHub Releases.

## Release Entry Template

Use this template for each new public release.

```md
## vX.Y.Z

_Release date: YYYY-MM-DD_

### Highlights

- User-facing headline change.
- User-facing headline change.
- User-facing headline change.

### Features

- New capability or workflow.

### Improvements

- Existing workflow that became faster, clearer, or more reliable.

### Fixes

- Bug fix described in user-facing terms.

### Upgrade Notes

- Anything users need to know before or after updating.
```
