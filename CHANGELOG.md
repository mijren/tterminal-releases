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

## v1.1.0

### Highlights

- **Features are worktrees.** The sidebar now lists projects › features. Each
  feature is its own git worktree with a generated name (rename any time), its
  own branch, browser URL, plan and tasks. Create one from the `+` next to a
  project; setup scripts (copy `.env`, install, `herd link`, an optional
  per-feature database) run as a visible pane.

- **Per-workspace tab bars.** Every project and feature keeps its own tabs:
  terminals, agents, editor files, Changes, History, Plan, Tasks and Browser.
  Split any two panes side by side; the columns and tabs stay clearly separated.

- **Files, Changes and History panel.** A third column browses the worktree,
  shows uncommitted or since-base changes with per-file revert, and lists the
  branch's commits. *Review all* opens a GitHub-style stacked diff with
  viewed marks that reset when a file changes.

- **In-app browser.** Native browser panes with device presets, a responsive
  drag edge, screenshots to the clipboard, and a Compare tab that shows the
  main checkout and the feature side by side.

- **Plan → tasks → review.** Each feature has a Plan tab (`.tterminal/plan.md`)
  with *Plan with …*, *Create tasks with …* and *Review all with …*. Tasks belong
  to a feature and their agents run inside its worktree; a feature shows
  **reviewed** once the plan carries a review and no task is open. Project
  settings define Planner, Implementer and Reviewer agents; App Settings › Tasks
  holds the editable prompt templates.

- **Finish in three stages.** The panel header walks *Pull request → Merge →
  Archive* in order: the Implementer opens the PR with `gh`, Merge runs
  `gh pr merge` (rebase by default, per-project strategy) and fast-forwards the
  main checkout — or, without a pull request, the Implementer commits what is
  left, rebases onto the base and fast-forwards it locally — and Archive runs
  the archive scripts, deletes the worktree and branch, and keeps a one-line
  history entry. Merge conflicts offer *Rebase with agent*.

- **Project settings in one place.** The project's gear (or ⌘⇧,) opens a
  modal with everything the project owns: General, Commands, Agents (with
  the Planner / Implementer / Reviewer / Git operations roles), Instructions (per-project
  workflow prompt overrides and prompt templates), Worktrees, Browser and
  Environment. Changes collect in a draft and
  save together with Save (⌘S); Discard drops them. General also decides
  what happens to tterminal.config.json in git: commit it on every save,
  keep it ignored, or leave it to you.

- **Diagnostics.** The bug icon in the sidebar footer opens a panel with the
  build and machine facts, the app's current state, a performance recorder
  (Dev builds), live app and frontend logs, and a Report section that saves
  or copies one Markdown report describing the problem with all of the above.

- **Setup and archive run in the background.** No terminal pane opens for a
  feature's setup or archive commands; the row shows *setting up* or
  *archiving…* (disabled) meanwhile, and a failure surfaces as a toast with
  the last lines of output.

- **Project environment applies everywhere.** Project settings › Environment
  variables (shared in the repo, or private to this machine) reach every
  terminal, command and agent the project starts, feature worktrees
  included; a command's own env still wins.

- **Local HTTPS in the browser tab.** Herd / Valet `.test` sites (and other
  hosts that can only be local) render even when macOS does not trust their
  certificate; other hosts keep the normal checks.

- **Browser Inspector and dev inspector removed.** The bookmarklet, the Chrome
  extension, the setup page and the Integrations settings section are gone,
  and so is the dev-only in-app annotation overlay with its MCP tools; the
  built-in browser tab, review comments and the Diagnostics panel cover that
  ground.

- **Menu bar item.** Shows how many agents are waiting for you and lists
  projects › features; pick one to jump to it. Toggle it in App Settings.

- **MCP.** Tasks carry `featureBranch`; sessions inside a feature default to it
  and `identify_session` reports it. New tools: `feature_list`, `feature_get`,
  `plan_get`, `plan_update`.

### Improvements and fixes

- Shorter buttons in a feature's Ship step, so their labels no longer get cut off: **Merge** (instead of "Merge PR #n"), **↻ Checks** (instead of "Re-run checks") and **PR** (instead of "Open PR"). Retrying after failed checks or a merge conflict shows **↻ Checks** or **↻ Merge** instead of "Re-run checks" / "Retry merge". Hover a button for its full description.

- **Checks and Review on the main project.** When you work directly on main, the Workflow panel now runs in four steps: Changes → Checks → Review → Push. **Checks** runs the same lint and test commands a feature runs before its pull request. **Review** hands your reviewer every commit you haven't pushed yet (plus uncommitted changes); it fixes what's in scope as a new commit and writes its findings into the Plan, and tterminal remembers what was reviewed so the next review only covers new commits. Nothing blocks you: **Push** just asks first when checks are failing or commits haven't been reviewed. Feature branches get Changes → Review → **Ship**, where Ship runs the checks and then the pull request → merge → archive steps: the review covers every commit on the branch, and Create pull request asks first if some haven't been reviewed (checks still have to pass before a pull request, as before).

- **Set your roles once for every project.** App Settings › Agents has a new **Default roles** card: choose the Planner, Implementer, Reviewer and Git agent (with model and effort) once, and every project uses them unless it picks its own for a role. In a project you've already set up, **Make these my defaults** (Project settings › Agents › Roles) copies its roles over in one click. Roles a project inherits read "Default: …".

- **Pick the model and effort for each agent.** In Project settings › Agents, every role (Planner, Implementer, Reviewer, Git operations) now has a model and an effort level under its agent — for example Reviewer = Codex · GPT-6.1-Sol · high — and Plan with…, Review all with…, the pull request and merge stages start the agent on exactly that. Codex's model list comes live from the Codex CLI you're signed in to (**Refresh models** re-reads it); Claude offers Fable, Opus, Sonnet and Haiku (always the newest) plus the current versions. In the + menu, Claude and Codex start on the model you used last, and their › lets you pick another model and effort before starting.

- **tterminal now supports Claude Code and Codex only.** The agent library, the Add agent picker, role pickers, the Welcome screen and MCP setup offer just these two (MCP setup also keeps Claude Desktop). Cursor, Aider, Kimi, Gemini, OpenCode and custom agents are gone, and you can no longer add a custom agent; you can still change where Claude Code and Codex are installed and their default arguments. Agent launchers for other tools saved in a project's `tterminal.config.json` or on this Mac are left in place but no longer appear. Cursor stays available as an editor for "Open in editor".

- **Plan and Messages are always there.** Every project and feature now opens with two fixed icon tabs at the start of the tab bar: Plan and Messages. They can't be closed, and your agents, terminals and commands follow them. **Messages** is the record of agents talking to each other: every question or task an agent here sent to, or received from, another agent (in this project or another one), with a one-line summary, who asked whom, the status, and the reply. Select one to read the whole thread; the agents online right now are listed underneath. `⌘⌥1`–`⌘⌥9` still start at your first own tab.

- **Plans are now phases and steps.** The Plan tab shows a title, a goal and numbered phases, each with a short description and a checklist of steps. Agents write the plan through tterminal's `plan_update` tool and tick steps off with `plan_step` as they work, so you can see at a glance which phase is in progress and how far along it is. You can edit, reorder, tick and delete phases and steps yourself; the goal and each phase's description open in a Markdown editor with a formatting toolbar (headings, bold, italic, lists, checklists, links, code) and a preview, and the plan now uses the full width of the window. Plans are stored in each workspace's `.tterminal/` folder, which is excluded from git: they never appear in `git status` and are never committed. Every project and every new feature worktree gets one automatically; an existing `plan.md` is carried over as the plan's goal.

- **Tasks are gone; plans and agent messages stay.** The Tasks board, the task tools agents used (`todo_*`, `assign_todos_to_agents`), autopilot runs, cross-project task handoffs, the "Create tasks with" button on the Plan tab and Settings › Tasks are removed, along with the `tterminal todo` CLI commands and the `/api/todos` HTTP routes. What remains is the Plan of each project and feature, and agents asking each other questions or handing each other work through `agent_ask`. Agents connected to tterminal are now told to write a plan you ask for into the Plan tab (through `plan_update`) instead of creating a Markdown file in your repository. Existing task data is left on disk but no longer shown. **Close completed automatic agents** now lives in Settings › Workflow.

- The Checks panel's live output no longer grows and shrinks while a check runs. It is now a fixed two-line glance at what the check is doing right now; hover a line to read it in full.

- Fixed automatic operation agents failing to finish ("the tterminal window did not apply complete … — is a project open?") after the first agent had already finished since tterminal started. The pane stayed open and the workspace didn't refresh. Every agent can now finish, however many finished before it.

- Fixed Codex agents failing to finish an automatic operation ("agent_finish requires a tterminal-managed agent"), so their pane never closed and the workspace never refreshed. Codex hides tterminal's pane id from MCP tools; tterminal now works out which pane the agent is running in by itself. The same fix gives Codex agents the right name in Messages and in `whoami`.

- Fixed `php artisan dev` (and other commands that run several processes side by side) leaving `artisan serve`, queue workers, Pail and Vite running after you stopped, restarted or closed them. Each restart left another server behind, until Laravel reported "Failed to listen on 127.0.0.1:8000 … 8007 (Address already in use)". Stopping a command now stops everything it started. Servers orphaned by earlier versions keep running until you quit them (or restart your Mac).

- Fixed Escape in Project settings: it no longer reaches the agent behind the window (which interrupted it), and in the Edit command dialog it closes only that dialog instead of the whole settings window. Selecting text in a command field and releasing the mouse outside the dialog no longer closes it.

- Dev servers now shut down cleanly when you remove a project, when tterminal relaunches after an update, and when an agent stops or closes them through MCP. Before, these were killed outright, so Laravel projects could be left with a stale `public/hot` file that broke page styles until it was deleted. Force kill is still immediate.

- Fixed clicking a branch or project in the sidebar sometimes landing on a different branch (or on a branch when you clicked the project itself) when it belonged to another project. One click now opens what you clicked.

- **Pause and Resume** for agents and commands (pane header or ⋯ menu): Pause freezes the process where it is and Resume continues it, with nothing lost. The old pause-looking button was Interrupt, which sends ⌃C and ends most commands. It now has its own icon.

- Fixed Restart moving an agent or command tab to the end of its group. A restarted tab now keeps its place.

- **Hand off to another agent:** from an agent's ⋯ menu, pick another agent, add an optional instruction, and it starts in the same folder with the previous agent's recent terminal output as its first message. You can stop the original agent in the same step.

- **Summarize what it did:** from a running agent's ⋯ menu, get a short bullet recap of its session (what it changed, where it stands) in a dialog you can copy. It uses the Summarizer tool chosen in Settings › Agents › Auto-summarization; automatic summaries do not need to be on.

- **Restart and resume conversation** for Claude Code and Codex agents (pane ⋯ menu or right-click the tab): restarts the agent and picks its most recent conversation in that folder back up (`claude --continue` / `codex resume --last`) instead of starting empty. A plain Restart still starts fresh.

- **Mouse controls for every tab.** A pane shown as a tab now has a header with its name, status, memory, and **Interrupt**, **Restart** and **Stop** buttons (**Start** once stopped), so you no longer need ⌘⇧S / ⌘⇧R. Its **⋯** menu (also on right-click of the tab, and on each split slot's header) adds Force kill, Find / Copy / Save / Clear output, Jump to top or bottom, Send prompt template, Rename, Copy agent ID, Reveal folder in Finder, Open folder in editor, Copy path and Close. Dev servers show their port as a clickable `:5173 ↗` chip, with Copy URL and Edit command in the menu.

- **Find in output:** press ⌘F in any terminal to search its scrollback. Matches are highlighted; ⏎ / ⇧⏎ step through them and esc closes the bar.

- **Pull from origin** and **Push to origin** now run git directly, so a clean pull or push finishes in about a second with no agent pane. If git fails (diverged history, rejected push, conflicting local edits), your Git operations agent starts automatically with git's error message and resolves it. **Commit all changes** still uses the agent.

- Projects with at least one open feature branch now stay under **Active** in the sidebar, even when no agent or terminal is running.

- The new-feature dialog now closes the moment you click **Create**; progress shows as a *Creating worktree…* row in the sidebar, and the dialog reopens with the error if creation fails.

- Clicking a file in Changes (or a commit in History) now moves the highlight to it, so arrow keys continue from where you clicked instead of the first row.

- Main-project Workflow now shows when origin has new commits ("3 new commits on origin/main") with a one-click Pull, plus ↓/↑ counts on Pull and Push. The app fetches origin quietly in the background (on focus and every 5 minutes, never touching your files); **Check** fetches immediately. This replaces the agent-driven Fetch origin button. Fetching uses your existing credential helper or ssh-agent and never prompts.

- Suppressed the automatic macOS Writing Tools popup in workspace windows to mitigate a native crash seen while typing in agent terminals. Embedded browser pages keep their existing behavior.

- Fixed a feature branch showing as merged after you pressed Update from main because a different branch had merged. Merged detection now only counts the branch's own commits.

- Replaced the menu-bar waiting count and project list with a resource dashboard: current-project CPU/RAM charts, system RAM, app-host memory, installed version, and update controls. Clicking the icon opens it directly below the menu bar without raising the main window; clicking again hides it, and moving into the dashboard keeps it open. Live refreshes no longer rebuild and dismiss the menu while you move over it.

- Main-project Workflow now offers Refresh, Commit all changes, Fetch origin, Pull from origin, and Push to origin. Fixed existing Pull/Push controls being hidden for GitHub-backed projects. Commits stay local until you choose Push; Git operations use the configured Git agent.

- MCP autorun guidance now defaults to parallel independent tasks and automatic progress tables at startup, each observed stage change, and completion. You no longer need to request periodic reports; explicit serial execution and quiet-mode preferences are respected.

- Fixed a black window when font downloads stall during startup or a webview reload. The interface now paints using local fallback fonts without waiting for internet access.

- Fixed release and production diagnostic builds failing to compile after adding Stop checks.

- Added Stop checks to branch Workflow. It stops the running check and skips the remaining queue; stopped checks can be run again and do not unlock PR or merge.

- Shortened the Workflow’s Pull request stage label to PR to prevent wrapping.

- Added Workflow as the first tab in the right panel. Checks, pull requests, merge/archive actions, and Git controls now live there, leaving Files, Changes, and History more room. New feature workspaces open on Workflow.

- The startup screen now lists saved projects to choose from. Add and ⌘T require a selected workspace, so opening a terminal no longer silently selects the first project.

- Reordered the Add menu pages to Plan, Tasks, Browser, then Timeline.

- Added Plan to the main project workspace’s tab-bar + menu. Edit or preview a project plan, plan with an agent, create project-level tasks, and review changes without creating a feature branch.

- The new-feature dialog now has one compact Create feature button. Creating a branch no longer automatically starts the Planner; start planning from the feature’s Plan tab.

- The command-copy picker now excludes Shell and other terminal launcher entries.

- Project Settings › Commands now lets you copy selected saved commands from another project. Choose a source project, pick the presets, and save; existing commands are kept.

- Fixed open file tabs losing unsaved edits and jumping to the top after switching tabs or workspaces. Drafts, cursor position, scrolling, and undo history now stay with each open file.

- New MCP autopilot runs use separate implementation and review capacity, with related review batches of up to three tasks. Independent feature worktrees can run in parallel; tasks sharing a checkout stay serialized.
- Reviews flush early for blockers, urgency, wait limits, and partial final batches. Every task keeps its own verdict, and shared reviewers remain open until all members finish.
- Autopilot status now reports phase/queue timings and batch membership to help diagnose slow runs. Existing saved runs keep their previous scheduling behavior.

- Connected ⌘Z and ⌘⇧Z, along with Edit → Undo/Redo, to the focused file editor’s undo history.

- Fixed file drag-and-drop uploads in the embedded browser by letting websites handle dropped files.

- Opening a folder now switches to that project’s workspace, including when returning from Universal Shell to an already-open project.

- Fixed ⌘O (Open Folder) doing nothing after opening a project. It now works in project and Universal workspaces, including while typing in a terminal.

- The Universal Shell tab-bar `+` now opens a terminal immediately, without a launcher menu or workspace options.

- Universal Terminals now appears under Active while a shell is running, instead of being placed below inactive projects.

- Universal terminals now show a dedicated Universal Shell header instead of the last project's name and branch. Their workspace hides project tools and supports multiple shell tabs and splits in your home directory.
- Review agents fix issues within their assigned scope and verify the result instead of sending fixes back to an implementer.
- Automatically created task, review, test-fix and Git agents close when finished. Disable this in App Settings › Tasks; manually opened agents stay open.
- PR creation and merging refresh their next action on completion, with clearer progress and a corrected merge watcher.
- Main project checkouts now offer Pull from origin and Push to origin through the Git operations agent.
- The feature cleanup button is now simply Archive.

- Project config saves default to Commit on every save, consistently across settings and command edits. Manual and Ignore remain available. Unrelated staged changes are excluded, and commit failures show a warning without losing saved settings.
- Failed checks have a View failure dialog with readable command output, exit status and Fix with Implementer, replacing the clipped hover tooltip.


- Feature checks show completed-check percentage, the active check number, elapsed time and live command output. The percentage measures finished checks; individual tools may not report their own progress.


- Browser tabs now explain failed pages and missing scripts/styles, with Retry, details, and Open externally. Browser failures appear in diagnostics reports, and tabs wait for their viewport to be ready before opening.
- New Laravel/Herd workspace profiles configure Vite with the feature site's HTTPS certificate, preventing blank React pages and missing admin styles caused by HTTP development assets. Existing workspaces need their Vite certificate paths updated and the Dev process restarted; standard saved Herd profiles receive the missing defaults for future features.


- Reduced project-switching work: file watchers no longer index every file for rename matching, and eight recent projects keep their watchers ready for reuse.
- Terminal resizing skips hidden panes and unchanged grids and avoids measuring layout twice, while preserving shell prompt layout and scrollback position.
- Git changes refresh feature information only for the affected project instead of refreshing every project in the sidebar.

- Reviewing your own pull request no longer offers Approve or Request changes, which GitHub refuses for the author; the panel says so and keeps Comment.

- While the sidebar has focus, ⌘↑ and ⌘↓ walk its project and feature rows, and ⌘[ and ⌘] jump from project to project.

- ⌘← and ⌘→ now move keyboard focus between the three columns (sidebar, workspace, panel), wrapping around. Tabs switch with ⌘[ and ⌘], which used to focus the next or previous running process; that action is still available as ⌘⇧↓ / ⌘⇧↑ and ⌘↓ / ⌘↑, and can be rebound in Settings › Keybindings.

- Adding a project no longer opens the Add agent window afterwards; the project simply opens, and agents start from its + menu when you want one.

- ⌘P now switches to the project you pick instead of opening the old project menu, which is gone along with the hidden project rail that hosted it. Managing commands from a launcher goes to Project settings.

- Fixed reopening a feature's browser after closing its serve tab showing a dead page: the dev server is started again whenever the browser is shown without one, and the page reloads as soon as it answers.

- The timeline moved from a fold under the features to its own page: hover a project row and click the calendar icon (or pick Timeline from the main checkout's + menu) to see what shipped on a full-width rail, with Copy as Markdown and Release notes in the header.

- Laravel projects served by Herd no longer start a dev server when their browser tab opens; the site is already there, and running Vite or a build is your call from the tab bar. Fixed the browser's first open sometimes starting two serve tabs.

- Fixed feature setup failing on its first step in projects whose profile clones folders from the main checkout: the runner executed the step's label instead of its command.

- Fixed a feature created in a project whose Worktrees settings were saved before this release getting no `.env` and no per-feature rewrites: those older settings are now completed from the stack profile (copy, clone, environment, serve, checks, share) while keeping your own setup and archive lines. Retry setup on an existing feature also restores the files it is missing.

- The sidebar now groups projects into Active (something is running in them) and Inactive, active ones first. The Worktrees profile is a dropdown instead of a row of chips, the timeline is drawn on a vertical rail with dated dots, and the New feature dialog no longer lists the share command in its preview.

- Review someone else's work in its own worktree: the New feature dialog's "From a branch or PR" tab lists open pull requests and remote branches, and opening one gives you the same browser, editor and agents you have for your own features. The panel offers Review all with your Reviewer, Approve, Request changes and Comment, review comments can be posted to the pull request in one click, and Archive removes only your local copy.

- Share a feature: the browser toolbar's Share button publishes the site on a public URL (Herd's share, or a Cloudflare quick tunnel for Next.js, Vite, Django and FastAPI projects) and copies the link once the tunnel is up. Expo, React Native and Flutter projects copy the address a phone on the same Wi-Fi can open.

- A feature that falls behind its base now says so before merge time: the sidebar row shows `behind n`, and the feature panel offers Update from main, which rebases the worktree (conflicts hand over to Rebase with agent). Merge waits until the branch is up to date.

- Each project gains a Timeline fold under its features: what was merged or dropped, when, with the pull request, the lines changed and the tasks it carried. Copy it as Markdown, or draft release notes from the features merged since the last tag and hand them straight to `gh release create`.

- Project settings › Worktrees now shows your stack as a profile: the header names what was detected, one click on a profile chip fills in its defaults, and the block is edited as rows instead of text areas: the `.env` keys rewritten per feature, folders to clone, files to copy, setup, the dev-server command, check rows and archive. Your branch prefix and merge method stay when you switch profiles.

- The New feature dialog starts with what you are about to do. Type it and the branch name follows; pick the base branch from a freshly fetched list (origin/main, develop, a release branch) without touching your main checkout; choose Feature, Bugfix (`fix/`) or Experiment (`try/`) to set the prefix and seed the plan with matching headings; see exactly what the stack profile will run; and press Create & plan to have your Planner start as soon as setup finishes.

- A Checks stage now sits before Pull request in the feature panel when the project's stack defines checks (lint, tests, typecheck, build). Run them from the panel, watch each row pass or fail with its timing, and hand a failing row to your Implementer with one click. Create pull request unlocks once every row passes on the current commit; a new commit sends the rows back to queued, and the sidebar shows `checks n failing` until they are green.

- Each feature can run its own dev server. Projects whose stack has a serve command (Next.js, Vite, Expo, Django, FastAPI, Laravel without Herd, or Vite beside Herd) start it on the feature's own port the first time you open the feature's browser tab, and the tab waits until the server answers. The sidebar row shows the port with a green, amber or grey dot, the browser toolbar shows the server with its uptime and a restart button, and the row's right-click menu can start, restart or stop it. The server runs as a normal `serve` tab, so its output is one click away.

- Creating a feature is faster and its copy of `.env` now works: the folders listed under Worktrees (such as `vendor` and `node_modules`) are cloned from the main checkout in an instant before the install runs, and the per-feature keys (`APP_URL`, `CACHE_PREFIX`, `VITE_PORT` for Laravel) are rewritten so the feature's site, cookies and Vite server point at the feature, not the main checkout.

- New projects get worktree defaults for more stacks: Django, FastAPI and plain Python (uv, Poetry or pip), Expo and React Native, and Flutter, alongside Laravel, Next.js and Vite. Each proposal now also lists the folders to clone from the main checkout, the `.env` keys to rewrite per feature (Laravel gets `APP_URL`, `CACHE_PREFIX` and `VITE_PORT`), a dev-server command and the checks to run. Existing projects keep their saved settings.

- Sidebar rows are tidier: a project shows its expand/collapse arrow only when it has features underneath, and every feature row starts with a small dot in the project's color, lined up under the project's thumbnail.

- Right-click an agent's tab to copy its agent ID, ready to paste into another agent for direct agent-to-agent messages.

- Fixed duplicated prompt lines after dragging a terminal into a workspace slot or otherwise revealing it at a new width.

- Multi-pane workspaces are back. The tab bar's + menu (⌘T) ends with a Workspace row: pick 2 columns, 3 columns or 2×2 to open an empty workspace tab, then drag tabs into its slots or add terminals from a slot. Double-click the tab to rename it. Closing a workspace tab returns its panes to ordinary tabs instead of stopping them. The Tabbed layout and saved workspaces, which had no way to be created, are gone.

- Sidebar project rows now show the main checkout the way feature rows are shown: a spinner while an agent is working there, the branch, and the uncommitted +/− lines. Hover the numbers for the changed and untracked file counts.

- Improved tab reordering with a preview that stays under your pointer, a clearer insertion marker, reliable drops between tabs, and automatic scrolling along crowded tab bars. Press Escape to cancel a drag.

- Fixed sidebar project thumbnails showing image data as text. Projects now show their uploaded image or their generated letters when no image is set.

- Updated Settings → Tasks and MCP help to match review-and-finish, including guidance for older custom reviewer prompts.

- Moved sidebar project colors to the cards’ outer left borders, removing the inner colored line for a cleaner layout.

- Fixed user-created terminal tabs disappearing after quitting and reopening the app. With Restore stopped tabs enabled, they remain available to restart across repeated launches.
- Command-K now offers New agent… for every project, using the agent picker even when no agent has previously been saved. New terminal remains available for projects with terminals enabled, including projects without existing processes.

- Sidebar projects now sit in subtle panels with more space between groups, taller headers, and clearer project names, making it easier to see which processes belong together.

- Task reviewers now review, fix, and finish ordinary issues themselves. Unresolved reviews escalate instead of automatically cycling back to implementation. MCP guidance now covers coherent task planning, independent concurrency, concise handoffs, and separately requested final audits.

- Prevented overlapping Git refreshes from flooding the backend. Git scans and diff reads use bounded background workers, while routine badges skip per-file line counts. Diagnostics now distinguish Git queueing and execution time.


- Spread terminal resize work across frames to reduce multi-pane resize bursts while preserving final terminal geometry.
- Improved diagnostics with automatic stall context, separate resize-stage timings, output backlog and pane metrics, and longer useful event history. Live metrics fetch summaries rather than full recordings.
- The diagnostics card can be minimized. Exported recordings now offer Copy path and Reveal in Finder actions.


- Added a production diagnostics build with performance recording and WebKit developer tools, using your existing production data and MCP connection.

### Upgrade notes

- Existing projects keep working unchanged; the main checkout shows
  project-level tasks. Feature worktrees live under
  `~/tterminal/worktrees.noindex/<project>/<slug>` by default (App Settings).
- Notifications now name the feature an agent belongs to.

## v1.0.3

### Highlights

- **Cleaner quick panel.** Command+K groups compact process rows under clear
  project headings, with indented child items and a subtle vertical guide. Each
  project path is shown once. Smaller type and action labels replace repeated subtitles and badges.

- **Picker selections open their pane.** Selecting a terminal, agent, or command
  from Command+K or the command picker now brings its pane into view. Selecting a
  stopped process also reveals the pane when restarting it.

- **Project-scoped terminal picker.** Add terminal shows the workspace's own items
  first. Use Other projects to choose another project's agents, commands, existing
  terminals, or a new shell in that project.

- **Tabbed workspaces.** Choose Tabbed when creating a workspace to group terminals
  in a scrolling tab strip without the grid's slot limit. Add new terminals, move
  existing ones into the group, or choose agents and commands. Background terminals
  keep running, the selected tab is remembered, and saved tabbed workspaces can
  reopen their shells and launchers. Each inner tab has a × close button, and
  Command+Shift+Left/Right switches tabs within the active workspace.

- **Less work during project switching.** Recent file watchers are reused,
  watcher setup runs off the async runtime, and simultaneous configuration reads
  are combined. Project-list metadata changes no longer trigger a full config
  refresh.

- **More useful performance recordings.** Whole-session counts and peaks survive
  raw-sample eviction, percentile estimates remain available, and agent queue
  samples distinguish busy agents, earlier exchanges, unavailable targets, and
  pending delivery.

- **Reliable task handoff scope.** Assignment prompts specify the Dev/release MCP
  server and destination board path for verdicts and status updates. Missing-task
  errors now identify the environment and project searched.

- **Inactive project actions.** Hover a project in the Projects drawer and click Open to expand its agent
  and command choices, or remove it immediately with ×. Project files are preserved.

- **Live handoff counts.** Sidebar task counts now update when agents add or
  advance tasks in another visible project, even before its board is opened.

- **Start a new project directly.** Adding a folder selects its workspace and
  opens the agent picker. Choose an agent or a custom command to start, without
  the settings-first detour.

- **Find installed editors.** Application and project settings now have searchable
  editor lists with Refresh. Zed and other supported editors are detected in
  Applications even without a command-line launcher. Added Windsurf, VSCodium,
  and VS Code Insiders. Saved preferences remain visible if an editor is missing.

- **Choose handoff agents.** Select a configured agent for the default implementer
  or reviewer, or enter a custom agent ID. Project settings now offers explicit
  feature switches and a Hide pinned shortcuts option. Disabled features disappear
  from the sidebar and the project's show/hide menu.

- **Record from the sidebar.** Dev keeps Start/Stop recording, slowdown markers,
  export and expandable metrics above the workspace controls. The pane status bar
  and sidebar footer now share the same height for a continuous bottom edge.

- **Test in a separate Dev app.** Development builds have their own MCP connection,
  boards, sessions and credentials, so release and Dev can run together. Dev MCP
  setup uses a separate `tterminal-dev` registration.

- **Record slowdowns.** Dev offers an optimized diagnostics build and an optional
  performance recorder with slowdown markers, timing summaries and local JSON
  export. Measure switching, MCP requests, agent waits and resource usage while
  working in your projects.

## v1.0.2

### Highlights

- **Keep agent names stable.** Task agents keep their task number and implementer
  or reviewer role instead of changing their displayed names. For other agents,
  right-click and choose Lock agent name to preserve your chosen name across
  terminal updates, agent rename requests, and app restarts. You can still rename
  that agent yourself or turn automatic renaming back on.

- **Hand tasks between projects.** Agents can discover projects and short agent
  IDs, create linked tasks on another board, and queue them for implementation
  and review. Progress and results return to the source task and agent. Task
  cards now offer Hand off, and project settings lets you choose default workers
  and opt into automatic execution. Existing paused runs stay paused, and retrying
  the same handoff does not create duplicate tasks.

- **Copy a short agent ID.** Agent menus copy a compact `ag_` address that other
  agents can resolve and use directly, replacing the long clipboard instructions.

- **Task cards are easier to scan and read.** Larger titles, notes previews, and
  separated metadata give the board more breathing room. Opening a task shows
  formatted Markdown for notes, acceptance checklists, and comments in a wider
  reading view. Use Edit to change the source, and write multiline comments
  without losing the draft if posting fails.

- **Project settings actions stay readable.** Copy path, Reveal in Finder, and Copy
  link keep their labels on one line, including when a custom project icon adds
  the Remove icon action.

- **Universal Terminals disappear when closed.** They now have a separate,
  neutral group without project pins or settings. Closing the last one clears
  the group, including leftover home-directory pages and empty workspaces from
  older sessions.

- **Close an active project from its header.** The new × after the ⋯ button stops
  its processes and closes its panes, pages, and workspaces. The project leaves
  the active sidebar but stays saved in the Projects drawer. Its autopilot run
  is paused so it does not start another task while closing.

- **Empty browser shortcuts respond on the first click.** The Add shortcut dialog
  opens immediately and closes when cancelled or submitted, without waiting for
  an unrelated button click.

- **Project settings open from the empty start page.** Clicking a project's gear
  shows its settings immediately, without switching panes or pins. The empty
  workspace no longer displays a project name or branch before you select a
  surface.

- **Autopilot reports phase handoffs promptly.** A waiting coordinator now hears
  when implementation is ready for review, review requests rework, or a task
  finishes, without waiting for the 10-minute timeout. Workers are instructed to
  report completion before going idle, and the scheduler continues the next
  phase automatically.

- **No more "What do you want to start?" screen.** Starting something lives
  in the Projects drawer and the project ⋯ menu, so an empty workspace now
  just says so and points there. The drawer's launcher reads "Select an
  agent", "New terminal", and the project's commands.

- **Project settings are a popover now, not a page.** The gear on a project
  opens a compact panel beside it: change the icon, rename inline, copy the
  path or link, pick which editor and browser open things, toggle the file editor and
  incoming asks, or remove the project. The old settings page and its "Legacy local
  data" card are gone. Agents, commands, and browser shortcuts are managed
  from the project ⋯ menu: hover any row for edit and remove, and the command
  editor gained an **Advanced** fold for run-on-open, restart, file watching,
  working directory, and environment.

- **The sidebar shows only what is running.** Projects with nothing open no
  longer take up space as empty sections. They live behind a full-height
  - **Projects** strip on the sidebar's edge — the badge counts them — and `\`
  or a click slides a drawer over the sidebar. Pick a project, choose what to
  start it with (an agent, a shell, or one of its commands), and it appears in
  the sidebar as the process starts. Close a project's last tab and it goes
  back to the drawer. The Projects dropdown and its sort mode are gone; drag a
  section's name to reorder the sidebar instead — a dashed slot shows where it
  will land.

- **Editor, Changes, Tasks and Browser are pinned under every active project**
  as a compact strip. Changes shows the Git file count, Tasks shows how many
  cards are open (hover for doing / review / todo), and Browser opens the
  project's shortcuts with an **Add shortcut…** that saves straight to the
  project config. Agents working on task cards are listed under their own
  - **Tasks** label below the running processes. Right-click any of the four
  to hide it for that project; the ⋯ menu on the project name brings it back.
  `⌘1`–`⌘9` now number only processes and workspaces.

- **Fewer editor reloads during builds and agent work.** The editor now rereads
  the open file only when that file or an enclosing directory changes. Writes
  elsewhere in the project no longer repeatedly transfer the open document.
  Changes to the open file and returning to the editor still refresh it.

- **Less rendering work when agents are busy.** Running several
  agents at once used to make the whole app feel heavy — most of all when
  switching between panes, which is exactly when you least want to wait. Four
  things were behind it, and all four are fixed. Panes were re-measuring their
  scroll position on every write and every scrolled line, and each measurement
  forced the browser to re-lay-out a page the other panes had just changed;
  those checks are now done at most once a frame, and background panes skip them
  altogether. Background panes also repaint far less often. Switching to a pane
  was doing its most expensive work — rebuilding the GPU text atlas — twice per
  switch, and then immediately throwing away the atlas it had just built; now it
  happens once. And the two things that ran on every scrap of output from every
  pane, decoding it and scanning it for "this agent is waiting on you", both got
  much cheaper. Nothing about behaviour changes: same scrollback, same
  notifications, same titles.

- **Graphite, everywhere.** The light theme has been re-based onto the same
  palette as dark. Where it was "Sea Glass" — a teal-white ramp that gave every
  panel, border and label a blue-green tint — it now uses the graphite ramp
  inverted: one lightness curve, one hue, shared with dark mode, so the two
  themes finally read as one product. The accent stays the same turquoise,
  darkened step for step so it holds its contrast as text on white; every accent
  step is matched to the value it replaced, so nothing gets harder to read.

Several surfaces that had been missed in the first Graphite pass are fixed
too — the settings cards (which still sat on a navy gradient), the diff hunk
and fold strips, the console preview inside section cards, and the theme
preview mockups in Appearance settings, which were still painting the old
navy. The terminal now uses the same neutral black in light mode as in dark,
so terminal colors render identically whichever theme you are in.

- **Task agents are named after their task.** A pane working a task map card
  reads as `Task #7 · Implementer` / `Task #7 · Reviewer` rather than the agent
  runtime that happens to run it ("Claude 2"). Autopilot and the board's Assign
  already did this; now an orchestrator can too — `spawn_agent` / `start_agent`
  take a `todoId` (and optional `role`), which names the pane, assigns the card,
  and hands the agent the task's own implement/review briefing, so `prompt`
  becomes optional. The task name and its card lineage also survive a restart:
  restored task panes used to come back wearing the runtime's name.

## v1.0.1

_Release date: 2026-09-01_

A new dark theme, plus three fixes to things that were slower or more broken
than they looked.

**Graphite — a new dark mode.** The previous dark theme, Tide, built its
surfaces on a grayscale that was not actually gray: every step carried a
strong blue cast, and since all panels, borders and text resolve through that
scale, the whole app read as dark blue. Graphite replaces it with near-neutral
surfaces and a turquoise accent. Beyond looking cleaner, it makes terminal
output more accurate — a colored interface shifts how the terminal's own
colors are perceived, and the terminal background is now fully neutral so
those colors render true. **The light theme is unchanged in this release** and
still carries its original blue-green cast, so the two no longer match; light
gets the same treatment next.

- Fixed Restart on an agent launched from a task killing it without bringing
  it back. Restarting now respawns the agent as it always did for terminals
  and launcher agents.
- Kept large Task Maps responsive during redraws by resolving dependency
  blockers once per board update and skipping unchanged card subtrees when
  switching between panes.
- Smoothed sidebar resizing: the drag handle no longer trails the cursor when
  you move the mouse quickly.

## v1.0.0

_Release date: 2026-08-30_

tterminal 1.0 — the first stable release. This round focuses on making the
app feel fast and trustworthy at scale: a major performance overhaul for
many-project workspaces, reliable Task Map autopilot, calmer and de-duplicated
notifications, and a hardened HTTP API.

- Added a Notifications setting to silence alerts when an agent is waiting for
  input without disabling process completion or crash notifications.
- Agent waiting notifications now fire at most once per waiting episode and no
  longer repeat when a terminal pane remounts, reattaches to its PTY, asks to
  notify through MCP, or is observed by more than one project window.
- Stopped agent notifications from appearing after you watched the agent
  finish and then switched panes or windows. Terminal focus reports and resize
  redraws during a switch also no longer look like fresh agent activity.
- Fixed Task Map autopilot falsely reporting “agent never started” when a pane
  registered late or its timestamps were slightly out of order. Dropped agent
  launches are now retried automatically before an attempt is consumed, and a
  failed delivery retry no longer stops unrelated work prematurely.
- Added a prompt-delivery watchdog to Task Map autopilot. If an agent pane
  opens but never receives its task, tterminal now closes and relaunches only
  that silent pane within 90 seconds, without spending a task attempt. Panes
  with task output are left untouched, and the timeout is configurable.
- Made HTTP API failures consistent for scripts: malformed or incomplete JSON
  now returns the documented JSON error envelope, invalid todo statuses cannot
  partially apply another field, and discovery includes example requests.
- Activity now remembers whether subprocess trees should start expanded, with
  an expand/collapse-all toggle beside the sort control.
- Made project switching and live pane updates smoother by isolating the
  sidebar, workspace, and window chrome, and by settling project config and Git
  state together instead of redrawing the whole project view several times.
- Reduced background rendering work in terminal panes, project recents, and
  the Activity overlay so focus changes and unchanged process data stay idle
  without disrupting terminal scrollback.
- Improved sidebar responsiveness for people with many projects by avoiding
  repeated launcher-menu construction and settings parsing during renders.
- Kept the sidebar responsive during live RAM updates by avoiding a full
  cross-project process-tree rebuild on every metrics sample.
- Isolated live process titles, busy/attention state, summaries, and CPU/RAM
  samples by pane, so one active process no longer redraws sibling panes or
  the whole project window every second.
- Limited live process-status sidebar updates to the affected project group
  instead of reconciling every open project's rows.
- Kept Tasks, Changes, and Files pages warm across recent project switches,
  and while Project Settings is open, preserving drafts and scroll position
  while rechecking retained content in the background when a project becomes
  active again.

## v0.9.9

_Release date: 2026-08-21_

### Several projects at once no longer cross wires

Running four projects side by side, each with agents on the MCP server, hit
three places where one project's work leaked into another's.

- **An agent can no longer re-point another agent's project.** `select_project`
  wrote a single machine-wide file that outranked each agent's own working
  directory, so whichever agent called it last silently redirected every other
  agent that had not named a project explicitly. The choice is now scoped to
  the session that made it.
- **Spawning and restarting now works for projects you are not looking at.**
  Only the project currently open in the window ever performed these, while
  the agent was told the action succeeded — so three of every four spawns
  vanished into a reported success. They now run for whichever project asked.
- **Actions that cannot be applied say so.** A spawn refused because a command
  is awaiting trust review, or because no project is open, comes back as a
  real error naming the reason instead of a silent no-op.
- **A background agent can no longer launch work into the wrong checkout.**
  `start_agent` inferred its parent from whatever pane you happened to be
  focused on; it is now scoped to the project that asked.
- Cross-agent `agent_ask` auto-start is verified too: a pane that could not be
  restarted is reported offline immediately, instead of leaving your question
  queued until it times out.

### Settings tells you whether your agent CLIs are actually connected

App Settings → MCP now checks each supported CLI and shows what it found.

The one worth knowing about is **"Old helper path"**. Registering tterminal
records the exact location of the helper inside the app bundle — and that
location moves when you reinstall, move the app, or run a development build.
The registration keeps pointing at the old one, nothing anywhere says so, and
the failure eventually shows up as "the agent never started". One click
repairs it.

Rows also distinguish "not installed" from "not connected", and a check that
simply failed never overwrites a badge that was green a moment ago.

### Trust a whole project's commands at once

Approving a dozen commands one dialog at a time was the most common reason a
"start all", an autopilot run, or a delegated task stalled half-finished.
**Trust all** is now offered both in the review dialog and in Project Settings
→ Commands. It approves that project only, and it resumes everything that was
waiting on the reviews it clears.

### Copy from a remote editor into your clipboard

Terminal programs can now put text on your system clipboard using OSC 52 —
which is how yanking in a remote tmux or neovim over ssh finally reaches your
machine. On by default; there is a toggle in App Settings → Terminal.

Clipboard _reads_ are never answered, on or off. Honoring one would hand
whatever you last copied to any program that can write to a pane, including
one on the far side of an ssh session.

### Activity shows what is actually running

The Activity overlay now shows CPU alongside memory, sorts by name, CPU, RAM
or status, lists any ports a pane is listening on (click to open), and expands
any pane running more than its own shell to show the processes inside it.

### Drive tterminal from a script

New: a local HTTP API and a **`tterminal` command-line tool** for anything that
is not an MCP client — shell scripts, CI, a launcher.

```sh
tterminal status
tterminal process list
tterminal todo list --status in_progress
```

It listens on your machine only, on a port picked at startup, and every request
needs a token the app generates and stores privately. The CLI reads both by
itself, so there is nothing to configure. Requests coming from a web page are
refused outright.

See the new [HTTP API & CLI reference](../http-api.md) for every route and
command. The CLI is not bundled with the app yet — build it with
`cargo build --release -p tterminal-cli` and put it on your `PATH`.

## v0.9.8

_Release date: 2026-08-20_

### Project configuration keeps itself in sync

- **Browser shortcuts (and everything else in `tterminal.config.json`) now
  sync themselves.** Editing the file in an editor, pulling it from git,
  letting an agent write it, or changing it in a second window updates the
  sidebar, the card grid and Project Settings on its own — no reopening
  settings and pressing Save. Shortcuts belonging to a project that is open
  but not active refresh too, so you no longer have to switch to a project to
  see them.
- Project Settings no longer parks on an "outdated" notice waiting for a
  click. When the panel has nothing unsaved it simply takes the file's
  version. If you are mid-edit when the file changes underneath you, it says
  so and your edits still win — Refresh is the way back to what is on disk.

### Cross-agent asks

- Cross-agent asks now reach background projects reliably. Multiline messages
  are delivered to Claude Code and other terminal agents as one paste and one
  submission, and terminal write failures are reported on the exchange instead
  of being silently dropped. `agent_ask_wait` resumes timed-out exchanges and
  `conversations_list` exposes thread history; the obsolete handoff and
  scratchpad MCP tools have been removed.
- Agent context menus can copy a durable pane reference ready to paste into
  another agent's instructions. New cross-agent asks announce who asked whom,
  and both participating agent rows show an `ASK` badge until the exchange
  completes.
- Projects now accept incoming cross-agent asks by default and expose a
  project-settings toggle to turn them off. Disabled projects clearly refuse
  `agent_ask` calls without queuing a message or starting an agent pane.

## v0.9.7

_Release date: 2026-08-20_

### Highlights

- **Heavy sessions stay fast.** Running a lot of agents used to hit a wall:
  everything slowed down at once, and switching between the Tasks board, a
  task pane, and an agent pane became noticeably heavy. Every open terminal
  held a GPU rendering context for the whole session, and past a limit the
  system started taking them back — panes dropped to a much slower renderer
  and stayed there, and every switch after that reclaimed one context by
  taking it from another pane. Terminals now release their context while off
  screen and pick one up when you look at them, so the limit stops being
  reached at all.
- Background terminals no longer redraw at full speed for nobody. Agents
  streaming output behind the active tab still receive every byte — your
  scrollback is exactly the same when you switch back — they just stop
  repainting sixty times a second while hidden.
- An idle window now costs nothing. The workspace used to re-render once a
  second for the whole session whether or not anything was happening; it now
  updates when a number you can actually see has changed. RAM figures step in
  whole megabytes instead of flickering on noise below that.
- The process monitor got much cheaper on a busy machine. It was reading
  detailed information about every process on your computer once a second,
  and walking each agent's process tree in a way that got slower as the
  machine got busier — the exact moment you least want it to. Both now scale
  with what tterminal is actually running.
- Returning to the window is smooth again with many terminals open, and
  switching panes does noticeably less work per switch.

### Running two copies of tterminal

- **A second instance can no longer take the MCP connection.** tterminal
  serves MCP on one socket per user, and it used to claim that socket
  unconditionally at startup. Opening a second copy — most often a
  development build next to the installed app — silently took the connection
  away from the first, which kept running but became unreachable. Every MCP
  client was quietly rerouted to the newer copy, so agents started from the
  older one were invisible to them, and agents started by the newer one died
  with it when it closed.
- Whichever copy has the connection keeps it. A second copy leaves it alone
  and says so: App Settings → MCP names the instance that holds it, instead
  of showing a bare "not listening".
- That status is now honest in general. It used to be derived from a file
  existing, which stayed true when the connection had died and when a
  different copy owned it — so the indicator read healthy through exactly the
  outage it was there to report.
- `whoami` reports which copy of the app answered it, so an agent can tell
  when it is talking to the wrong one.
- **If you run a development build alongside the installed app**, the
  development build will now come up without an MCP connection and show that
  conflict, rather than taking the connection from the installed app. Quit
  the installed app first if you want the development build to serve MCP.

### Task Map

- Restarting the app mid-run no longer wrecks the queue. Autopilot tracks its
  agents in memory while the queue itself lives on disk, so a run resumed
  after a restart was looking for agents the new process had never started —
  concluded they had never launched, failed the task, and skipped every
  remaining task in the queue. A restart is now recognised for what it is:
  work lost with the previous instance, re-dispatched on the next pass.
- The related error message no longer sends you hunting through your agent
  configuration when the cause was the app restarting or a second instance
  holding the MCP connection. It now says which of those to check.
- Task cards keep working after a restart. A task's record of which agent is
  working it could only be resolved during the session that started it, so
  after a restart a card showed its agent as dead and its pane could never be
  cleaned up. Tasks now store a handle that survives.

### Fixes

- A task agent winding down no longer piles up cleanup attempts. Every write
  to the task file — and agents write it often — started another one against
  the same pane.
- The MCP connection no longer stops accepting clients after a single
  transient error.

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
