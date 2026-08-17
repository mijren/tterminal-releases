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

## v0.9.6

_Release date: 2026-08-17_

### Highlights

- One sidebar. The project rail column is gone — its job moved into a
  compact **Projects** dropdown at the top of the sidebar, giving the width
  back to your terminals. The dropdown lists open projects first (with RAM,
  a live status dot, and uncommitted-change counts) and the rest of your
  recents below; clicking any project puts its section in the sidebar and
  jumps straight to it. Projects that aren't open yet open on their Tasks
  board — starting agents and terminals stays exactly where it was, on the
  section's own controls.
- Press `\` to open the Projects dropdown from anywhere, then ↑/↓ and Enter
  to switch projects without touching the mouse. Tab/terminal search moved
  to `⌘\` (both rebindable in Settings → Keyboard). Tiny colored dots on the
  closed dropdown tell you at a glance when an agent in another project is
  busy (pulsing) or waiting on you (amber).
- The ⇅ button beside the dropdown opens manage mode: drag projects into the
  order you want, ✕ removes one from the app. Esc or a click outside puts
  everything away.
- Workspace slots grew quick actions: every slot header now has Restart,
  Stop, Eject, and Close side by side, so you can manage a process without
  ejecting it back to the sidebar first. Stop keeps the tab around for
  inspection; Close stops the process and frees the slot.

### Task Map

- Agents are now cleaned up the moment their card moves on. Tasks record
  which phase their process serves, so an implementer whose card reached
  Needs review (or Done) is stopped and unhooked even if the move happened
  while you were in another project — no more closing leftover panes by
  hand before a review can start. Dragging a card to the next column counts
  too.
- Starting a task from the board no longer yanks you to the new agent's
  pane. The agent launches in the background — the toast and the card's
  running marker confirm it — and you stay on the board to dispatch the
  next card.
- Task agents always start in the task's own project now. Previously, with
  several projects and agents running, a dispatch could inherit the
  project of whatever pane you happened to be focused on and implement in
  the wrong checkout.
- The initial prompt paste verifies itself. If an agent spawns but its
  composer stays silent (startup dialogs and slow TUIs could swallow the
  paste), tterminal re-pastes before submitting — killing and relaunching
  the agent by hand shouldn't be needed anymore.
- Boards refresh whenever you re-enter the Tasks page, and a board that is
  still loading no longer masquerades as "No tasks yet".

## v0.9.5

_Release date: 2026-08-17_

### Highlights

- Task Map autopilot: hand a whole board to your agents and walk away. Tell an
  agent which tool implements (say, Codex) and which reviews (say, Claude), and
  tterminal works the queue one task at a time — implement, review, then the
  next task whose dependencies are done. Failed tasks halt the run so nothing
  builds on a broken step, and when the queue drains you get a summary of what
  landed and what didn't. Runs keep going while you work in another project,
  and each project can have its own.
- Play and stop buttons on the task cards themselves. To do and Needs review
  get a ▶; a running task gets a ■. A greyed-out ▶ always says why in its
  tooltip — no implementer assigned, no reviewer, or still waiting on another
  task.
- Stopping a task in progress asks its agent to undo its work first. The agent
  reverts what it changed, comments on what it rolled back, and hands the card
  to To do — so a cancelled task leaves your working tree as it found it
  rather than half-edited. Stopping a review just ends the review; the card
  stays put for another run.
- Plans can now name their agents up front: `todo_create_batch` accepts an
  implementer and a reviewer per task, so a plan arrives with the right tool
  already assigned to each piece of work.

### Fixes

- Task cards can be dragged between columns. Cards would lift but never drop,
  so the board could only be reordered by editing a task.
- Starting a task from the board now takes you to the agent it launches
  instead of leaving you on the previous pane.
- Opening another project's Tasks or Changes tab lands on that page on the
  first click. It used to bounce you to the last agent you had open in that
  project and need a second click.
- Agents can be started for a project you are not currently looking at, and
  agents that came from the global library or were found on your machine are
  now visible to `list_available_agents` instead of appearing unconfigured.
- Long prompts sent to a running agent — rollbacks, prompt templates — arrive
  as one message and actually send, rather than landing in the composer split
  across lines and waiting for you to press Enter.
- A project parked on Tasks or Changes stays there after you click into
  another project's agent.

## v0.9.4

_Release date: 2026-08-16_

### Highlights

- Task Map: every project's todo list is now a kanban board ("Tasks" in the
  project launcher) where AI agents work through tasks for you. Stage tasks
  with acceptance criteria, assign an implementer and a reviewer to each, and
  start them when ready — agents pick up the task prompt, report back with
  comments, and move cards through To do → In progress → Needs review → Done.
  Cards support drag and drop between columns, a skip-review option for small
  tasks, and per-column clearing.
- Commands can now close their tab automatically when they finish
  successfully. Turn on "Close on success" for one-shot tasks like builds or
  migrations: the completion notification fires first, then the tab tidies
  itself away. Failures and manual stops keep the tab open so the output
  stays inspectable.
- Switching between the Tasks, Changes, and Editor pages is instant now.
  Pages you have opened stay warm behind the workspace instead of reloading
  their git scan or task list on every visit — and a rendering fix means an
  open Changes page no longer slows the rest of the app down.

### Improvements

- Running processes gained a Restart hover control in the process sidebar —
  stop and start fresh in one click, next to Stop and Close.
- The bottom of the projects column repeats the three header shortcuts (add
  project, universal terminal, app settings), so they are reachable from
  either end of a long project list.
- App Settings gained a Tasks section where the prompts given to task
  implementers and reviewers can be customized and reset, without having to
  know the underlying prompt-template tags.
- Agent tabs are numbered per project now, and numbers are reused: the first
  Claude in a project is "Claude" again even if other projects have their
  own, and closing "Claude 2" frees that name for the next launch instead of
  counting up forever.

### Fixes

- Arrow-key navigation in the process sidebar follows the visible order
  everywhere, including across projects. Stepping onto another project's
  pane or page switches to it directly — previously the highlight could skip
  rows, land on the wrong tab, or show two selections at once.
- Command-1 through Command-9 select the tab their sidebar number badge
  actually points at; the shortcut order and the visible order could
  previously disagree once a project had task agents or reordered groups.
- Toggling "Notify on success", "Auto-restart", or restart file patterns on
  a command saves immediately again — those switches previously looked saved
  but only persisted after pressing Save now.
- Commands that close their tab on success always deliver their completion
  notification first, even for quick runs finishing under the usual
  notification threshold.

### Upgrade Notes

- No project migration is required.
- Task Map data lives in the existing per-project todos store; agents
  interact with it through the MCP `todo_*` tools.

## v0.9.3

_Release date: 2026-08-12_

### Highlights

- Command-Down and Command-Up now step through running processes in the order
  the process sidebar shows them, from anywhere — including while you are
  typing in a terminal. Previously they followed an internal launch order, so
  the jump looked random once tabs or projects had been rearranged.
- Project Settings saves as you type again. Edits could previously sit on
  "saving" indefinitely, and only the Save now button committed them.
- A command is approved once. Editing an approved command no longer asks for
  approval again.

### Improvements

- Adding a command keeps its editor open while you type — the first keystroke
  used to close it — and new commands start enabled, so they appear in the
  project's launcher right away.
- A project's commands now appear in its launcher menu without having to open
  that project first, and stay current when its settings are edited from
  elsewhere.
- Project headers in the process sidebar no longer shift when hovered: the
  quick actions fade in over the header instead of pushing its contents
  around, and the header keeps a fixed height.
- The collapse arrow on a project header now lines up with the group's spine,
  and the first project clears the "TABS" keyboard hint.

### Fixes

- Opening project settings no longer leaves a process highlighted alongside
  them, and Escape returns you to the tab you were on with its keyboard focus
  restored.
- An edit made immediately before closing Project Settings is no longer lost.

### Upgrade Notes

- No project migration is required.
- Command approvals now survive edits to the command. This also means a
  command definition changed by something other than you — an agent editing
  `tterminal.config.json`, or a config that arrives with a branch — will not
  ask to be reviewed again. Use "Reset trust" on a command in Project Settings
  to require approval once more.

## v0.9.2

_Release date: 2026-08-10_

### Highlights

- Project groups in the process sidebar can now be collapsed without stopping
  their processes. Collapse state is remembered across app restarts.
- Project headers now show their custom icon and reveal quick actions for
  Finder, the preferred code editor, project settings, and the project
  launcher when hovered.
- A busy process that temporarily stops reading terminal input no longer makes
  typing lag in every other terminal or agent.

### Improvements

- New processes are grouped by type, keeping agents, terminals, and commands
  easier to scan as a project grows.
- Workspace pane drop targets are clearer and more reliable while rearranging
  running processes.

### Fixes

- Closing a workspace now terminates the processes it owns instead of leaving
  them running without a visible workspace.
- Processes removed from a project remain removed after restarting the app.

### Upgrade Notes

- No project migration is required.
- Existing processes continue running when their project group is collapsed.

## v0.9.1

_Release date: 2026-07-22_

### Highlights

- Command-Up/Down and Command-[ / ] now move only between running processes,
  skipping stopped entries while preserving their normal arrow-key access in
  the sidebar.
- Agents can be renamed with F2 or from the process row's right-click menu.
- The app now starts fresh installs in its dark Tide theme and uses one
  consistent `tterminal` name and app mark across Welcome, loading, settings,
  updates, macOS chrome, and packaged icons.

### Improvements

- Process selection uses one fixed, theme-aware accent instead of inheriting a
  project's color. The sidebar now shows only the active placement highlight,
  while project colors remain identity markers on their groups.
- Development builds include a guarded Settings -> Developer action that clears
  app-owned development data, stops running processes, and restarts into the
  first-run experience without modifying project folders or project config.
- Production and development app icons now match the Tide palette, while the
  development icon retains its red status cursor for quick identification.

### Fixes

- Returning to tterminal after a background agent finishes no longer redirects
  the workspace automatically. Navigation occurs only after clicking the
  system notification or the in-app View action.
- Sidebar keyboard movement no longer leaves both the previous selection and
  the new cursor position highlighted at the same time.

### Upgrade Notes

- No project migration is required.
- Existing saved theme preferences are retained. Dark is the default only for
  fresh or reset app data.

## v0.9.0

_Release date: 2026-07-18_

### Highlights

- A new three-surface workspace makes projects, open processes, and the active
  terminal visible at the same time: the project rail stays on the left, the
  resizable process sidebar groups tabs by project, and the main workspace
  holds terminals, split workspaces, Editor, and Changes.
- The unified global header reports running processes, total RAM, and agents
  that need attention. Selecting it opens Activity, a cross-project view with
  controls for every live process.
- Named split workspaces can be saved per project and launched again with their
  selected layout and agents. Workspace dividers are draggable, resettable,
  and persisted.
- Running processes remain owned by the native app across UI reloads and Mac
  sleep/wake cycles, reconnecting with buffered output intact and without
  duplication.

### Improvements

- Project rows show only their Git file-change count; process activity is kept
  in the process sidebar and Activity view instead of adding dots to projects.
- Process groups follow the same project order as the project rail, including
  after drag reordering. Long project lists scroll and auto-scroll while
  dragging.
- Project launcher menus list agents and commands directly, provide quick
  Reveal in Finder and Open in Editor actions, and place Saved workspaces
  directly below Browser shortcuts.
- Keyboard navigation moves between the project rail, process sidebar, and
  workspace with Command-Left/Right. Arrow keys preview tabs, Enter or Space
  commits focus, and Command-1–9 follows visible sidebar order.
- Backslash searches every process and workspace. Command-P selects a project
  and opens its launcher menu.
- Git change counts appear on project rows and the Changes page. Editor and
  Changes are regular project menu actions.
- Universal terminals run in the home directory as project-neutral tabs. When
  selected, no project is highlighted in the project rail.
- Notifications distinguish visible work, background work, user-requested
  stops, and genuine crashes. Repeated crashes are grouped into one useful
  notification.
- Scratchpads and Todos, including their UI, settings, dependencies, desktop
  commands, and MCP tools, have been removed. The MCP server remains focused on
  process control, coordination locks, timers, and prompt templates.

### Fixes

- Command-W closes the active tab through the same safe lifecycle as its close
  button instead of closing the application window.
- Terminal output defaults to the stable DOM renderer on macOS, avoiding
  corrupted or overlapping glyphs on affected systems.
- Fresh launches open a clean workspace with keyboard focus in the process
  sidebar, even when process recovery completes after the initial render.
- Moving processes into split workspaces no longer causes duplicate terminal
  text, repeated reflows, or jumps within scrollback.
- Cross-project tab selection, drag reordering, and command launches retain the
  correct project ownership and visible ordering.

### Upgrade Notes

- Upgrading from a pre-0.9 release performs a one-time cleanup of saved open
  panes, tabs, and workspace layouts. Projects and configured agents and
  commands remain available.
- No project migration is otherwise required. Existing project files, running
  processes, commands, agents, and saved workspaces remain available.

## v0.8.0

_Release date: 2026-07-14_

### Highlights

- The sidebar is now one unified, scrollable workspace for every Project.
  Projects keep their own Terminals, Agents, Commands, Browser shortcuts,
  Todos, Scratchpads, Files, and Changes together, while the workspace follows
  the row selected with the mouse or keyboard.
- Processes can now be managed across Projects without first switching the
  active workspace. Start, stop, restart, focus, search, and notification
  actions preserve the exact Project, command, and pane they belong to.
- Processes can be marked as favorites from the sidebar or command palette.
  `Command-Shift-]` moves to the next favorite and `Command-Shift-[` moves to
  the previous favorite in the sidebar's visible order, including stopped
  Processes that can be started again.
- Section headings now open card grids for Terminals, Agents, Commands, and
  Browser shortcuts. Hold Command to reveal numbers, then press
  `Command-1` through `Command-9` to activate the matching card or open its
  browser link.

### Improvements

- Project Settings now opens inside the workspace from each Project header, so
  the sidebar remains visible and usable while configuration is open.
- Project switching is faster and more predictable: Project contexts, command
  metadata, Git state, process rows, and workspace focus are cached and restored
  independently.
- Auto-start now plans launches per Project, reuses the correct stopped pane,
  avoids duplicate agents and commands, and respects command trust before a
  process starts.
- Sidebar navigation uses one animated selection treatment for mouse and
  keyboard input, smooth row movement, and consistent section ordering across
  active and inactive Projects.
- The terminal search and process palettes group results by Project and can
  focus, start, or restart the exact matching pane across the whole workspace.
- Project data access has been consolidated behind shared domain modules,
  reducing duplicated filesystem, notes, Git, and configuration logic in Tauri
  commands.
- New Projects start with an empty Commands list instead of suggested commands,
  leaving configuration clean until commands are explicitly added.

### Fixes

- Restarting a stopped or auto-started command now refreshes the same visible
  pane instead of starting an older retained session in the background.
- Starting a stopped agent in another Project now targets the selected instance
  rather than a live or more recent sibling of the same agent type.
- Sidebar wheel input is contained within the sidebar, so scrolling Project
  Settings or long process lists no longer moves the workspace column with it.
- Cross-project navigation now preserves the selected row, pane, focus region,
  branch badge, RAM usage, and notification destination instead of falling back
  to the first Project or process.
- Running-process and favorite-process shortcuts now continue from the current
  sidebar position and follow the visible top-to-bottom order.
- Orphaned process panes can now be inspected and cleaned up instead of
  remaining invisible after their Project or launcher disappears.
- Universal Terminal keyboard input, restored pane cleanup, process lifecycle
  races, and project-scoped start/restart behavior have been corrected.

### Upgrade Notes

- No project migration is required. Existing panes, favorites, Project order,
  and sidebar preferences are retained.
- The sidebar layout has changed substantially; review App Settings -> Sidebar
  if you want to adjust subgroup order or visibility after updating.

## v0.7.9

_Release date: 2026-06-29_

### Highlights

- Agent panes can now show compact auto-summaries and state classifications in
  the sidebar. Enable auto-summarization in App Settings -> Agents, choose the
  summarizer tool, set the cadence, and tterminal will summarize recent terminal
  activity after human input, quiet periods, or longer busy runs.
- The file editor now highlights many more languages out of the box, expanding
  beyond the previous core set to cover common web, backend, systems, config,
  and scripting files.
- Agents are easier to hand off between each other: process rows and cards can
  copy a ready-to-use MCP reference so another agent can inspect the process
  status and recent output without guessing the right command.

### Improvements

- Agent summary results now surface as sidebar state badges such as `IDLE`,
  `PERMISSION`, `THINKING`, `WORKING`, and `ERROR`, with the latest summary used
  as the agent's secondary context when available.
- Auto-summarization runs through a bounded native headless command with a
  timeout, captured output limits, configurable headless args, and `{prompt}`
  placeholder support for CLIs that expect the prompt as an argument.
- MCP process lookups can resolve copied process IDs across visible projects,
  making cross-project frontend/backend agent coordination more reliable.
- Copying a process reference from a row or card restores focus back to the
  terminal pane that was active before the context menu opened.

### Fixes

- Closing a secondary project window no longer tears down every running PTY in
  the app, so agents and commands survive normal window/project switching.
- Removing a running agent now asks for confirmation before stopping and
  removing it, reducing accidental loss of in-progress agent work.
- Agents with nested subagents now prompt before closing descendants, so you can
  choose whether to keep child agents running or close them with the parent.

### Upgrade Notes

- Auto-summarization is disabled by default. To use it, enable it in App
  Settings -> Agents and choose a configured summarizer tool.
- No migration is required for existing projects.

## v0.7.8

_Release date: 2026-06-28_

### Fixes

- The project list no longer caps recent projects at 10, so adding more
  projects keeps them visible and scrollable instead of hiding older entries.
- Browser shortcuts can now stay enabled for any project type, including Python
  projects, even before shortcut rows are added. The sidebar also exposes the
  Browser shortcuts section so it is easier to open and configure those links.

### Upgrade Notes

- No action required after updating.

## v0.7.7

_Release date: 2026-06-20_

### Highlights

- The app's command overlays — the ⌘K command palette, the terminal and agent
  launcher, and the prompt-template picker — now share one consistent design.
  Each has the same header, row layout, status badges, and a new footer that
  spells out the available keyboard shortcuts, so they all look and behave like
  one system. The agent pickers in the sidebar were brought in line with the
  same styling.

### Fixes

- Terminals that suddenly rendered with heavier, off-looking text (showing a
  "CANVAS" badge in the corner) after the system reclaimed the GPU — for example
  after the window sat in the background or moved between monitors — now recover
  to crisp GPU rendering on their own when you return to the pane, instead of
  staying degraded until you reopened it.

### Upgrade Notes

- No action required after updating.

## v0.7.6

_Release date: 2026-06-18_

### Highlights

- Projects now restore their saved agents and terminals after closing and
  reopening the app, so your workspace comes back with restartable panes instead
  of forcing you to add everything again.
- Notification clicks now route back to the exact project and agent pane that
  produced the alert, even when macOS only reactivates the app instead of
  delivering a direct notification-click callback.

### Improvements

- The startup splash screen now keeps the branding centered and avoids the
  duplicate left-aligned logo/text while the app is starting.
- Project Settings now shows a shorter, prioritized set of suggested commands
  so the Commands page stays compact and focuses on common workflows like dev,
  start, test, build, lint, typecheck, and migrations.

### Fixes

- Removing a project now clears its saved agent and terminal pane state, so
  importing that project again starts clean as expected.

### Upgrade Notes

- No action required after updating.

## v0.7.5

_Release date: 2026-06-18_

### Highlights

- Sidebar sections (Terminals, Agents, Commands, Todos, and more) are now shown
  as distinct cards — each with a colored spine, a section icon, and its live RAM
  total in the header — so the whole sidebar reads as one consistent system at a
  glance. Cards collapse from the header, and per-row start/stop/restart controls
  appear on hover.
- The Changes view gained a real review workflow: mark files as viewed (tracked
  by a progress ring in the header), group changes by folder, jump to the next
  unviewed file, and switch between unified and side-by-side (split) diffs.

### Improvements

- Each section card and changed-file row shows a proportional +/− churn bar, and
  a whole-project RAM total now sits above the section cards.
- Long runs of unchanged context in a diff collapse to an expandable bar so large
  files stay easy to scan.
- The Files and Changes views at the top of the sidebar now match the section
  cards; Changes highlights in amber only when there is something to review.

### Upgrade Notes

- No action required after updating.

## v0.7.4

_Release date: 2026-06-15_

### Highlights

- MCP agents can now hand work across projects, so a coordinator can start an
  enabled agent in another local project and keep the child pane grouped in the
  sidebar.
- The project sidebar body has been redesigned into a denser command-tree
  layout, making running panes, launchers, project views, and status cues easier
  to scan together.
- Project option toggles are now wired into the workspace, so project feature
  settings immediately shape what the sidebar and workspace expose.

### Improvements

- Shell panes that are started outside the project root now still appear in the
  sidebar Terminals list instead of disappearing from project context.
- Notification clicks now focus only the pane they were created for, reducing
  unexpected focus jumps when switching between projects.

### Fixes

- Legacy restored panes are cleaned up on project open so stale saved sessions
  do not linger in the sidebar.
- MCP-spawned orphan shells are now detected and closed more reliably when a
  project is opened or reopened.

### Upgrade Notes

- No action required after updating.

## v0.7.3

_Release date: 2026-06-14_

### Highlights

- Project auto-start settings are now grouped by process type, matching the
  sidebar order for agents, commands, and terminals.
- Universal Terminal can now be minimized instead of closed, keeping the
  session alive while its sidebar icon shows that it is still running.
- Cross-project process notifications now surface completions, crashes, and
  waiting agents even when you are working in another project.

### Improvements

- Routine Start and Stop actions are quieter; restart and bulk lifecycle
  actions now show one focused update instead of a cascade of start/stop
  messages.
- Notification and toast View actions now preserve the exact target pane across
  project switches, so clicking them lands on the agent or command that needs
  attention.
- Legacy project agents are migrated into the local agent store more safely, and
  migrated agents can be started directly from the sidebar without deleting and
  re-adding them.

### Upgrade Notes

- No action required after updating. Projects with legacy synced agents are
  cleaned up locally the next time they are opened.

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
