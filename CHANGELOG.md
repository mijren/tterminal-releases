---
title: Changelog
description: Product updates, release highlights, and upgrade notes for tterminal.
---

# Changelog

Notable changes to tterminal, written for the people who use it: what changed,
why it matters, and whether you need to do anything after updating.

## v0.2.1

*October 2026.* A Tauri kit, one list of checks, and a simpler New tab.

### Stack kits

- **Tauri.** Open a Tauri project and the Stack tab knows it: *Run app*
  (`⌘J R`) starts `tauri dev` and shows the build as it goes (crates
  compiled, rebuilding after a Rust edit, window open). Compile errors and
  panics come with file and line and *Send to agent*. Health runs clippy,
  tsc, ESLint, `cargo test` and `cargo fmt --check`; after an agent turn only
  the checks for what changed run. Actions build the release `.app` and
  `.dmg`, regenerate ts-rs bindings, make icons, and start iOS or Android dev.
  The app doesn't start when you open the project, because the first Rust
  build takes minutes.

### Changed

- **A simpler New tab.** The `+` now sits right after Plan and Messages and
  stays there however many tabs you open. Its menu has Terminal, Claude,
  Codex, Timeline, the three grids and *Run a command…*, each with a single
  key: `⌘T` then `C` opens Claude. Saved commands moved out of this menu;
  they're in the Stack tab and `⌘J`.
- **One list of checks.** Stack › Health now holds every check. Switch on
  *Before a pull request* for a check (lint, typecheck and tests are on by
  default) and the Workflow tab's Checks stage runs it on the whole project.
  If a check already passed on that commit with nothing uncommitted, it isn't
  run again and shows *from Health*. Checks you had under Project settings ›
  Worktrees keep running; *Move to Stack* there puts them in Health.
- **New tabs open beside you.** Whatever you open goes right after the tab
  you're in, not at the end of the strip.

## v0.2.0

*October 2026.* tterminal starts a new version line here: 0.2.0 is the
starting point, and each release counts up from it until 1.0, the version we
call stable.

This release turns tterminal into a workspace that knows your stack. Open a
Laravel, Flutter, Next.js or Python project and the dev server, the checks and
the commands you run every day are one click — or one key — away. The window is
lighter too: browsing, editing files and reading history now happen in the
tools you already use.

> **Updating from 1.1.x:** download 0.2.0 from the website once. The built-in
> updater only moves to higher version numbers, so it won't offer 0.2.0 to a
> 1.1.x install. From 0.2.0 on, updates arrive in the app as before.

### Stack kits

- **Laravel.** Opening a Laravel project starts `php artisan dev` (or your
  `composer run dev` on older apps), unless a tab already runs one. Every
  process — server, queue, logs, vite — is listed from the moment it starts,
  with its URL and error count. Pending migrations come with a Migrate
  button; test failures show file and line, *Send to agent* and *Re-run
  failed*. Missing `vendor` or `node_modules`? It offers to install them.
- **Your Herd domain, first.** When Laravel Herd serves the project (a linked
  site or a parked folder), the site link opens its domain
  (`https://shop.test`) instead of `localhost`.
- **Flutter.** *Run on* the device you used last, or pick a device, simulator
  or emulator (it boots first). While the app runs: hot reload, hot restart,
  a screenshot, logs per device, and copy the Firebase App Check debug token
  or the FCM token. Run on several devices at once. Health shows `dart
  analyze` problems, tests and formatting. Ready-made actions build the
  Android App Bundle or APK, the iOS IPA or web, and handle packages and pods.
- **Next.js.** The dev server starts with your package manager; compile and
  runtime errors show with file and line and *Send to agent*. Your routes are
  listed — a click opens the page, and `/orders/[id]` asks for the id.
  TypeScript and ESLint run after each agent turn; Vitest or Jest on demand.
  Monorepos: pick which Next app runs.
- **Python.** Django, FastAPI and Flask, with uv, Poetry, pipenv or a
  `.venv`. The dev server starts in the project's environment (with a Celery
  worker when you use Celery). Tracebacks show the exception and the line in
  your code. Migrations (Django or Alembic), Ruff after each agent turn, mypy
  or pyright and pytest on demand.
- **Checks after every agent turn.** When Claude or Codex finishes changing
  files, tterminal runs the project's checks on what changed — tests,
  formatting, type checks, or a hot reload and a screenshot for Flutter — and
  shows the result with *Send to agent* and *View changes*. Nothing goes to
  the agent unless you click.
- **⌘J, then one letter.** The routine commands from anywhere, including a
  focused terminal: restart the dev server, run tests, migrate, typecheck,
  build, open the site. Rebind it in App Settings › Keybindings.

### The Stack tab

- **Stack · Workflow · Changes.** The right-hand column now leads with Stack:
  everything that runs in the workspace, in three groups — *Services* keep
  running, *Health* checks pass or fail, *Actions* run when you ask.
- **Project settings › Stack.** One place for what starts when you open a
  project, what runs after each agent turn, and whether a new feature branch
  starts its dev server once it's set up. Every command — the kit's and yours
  — in one list per group: switch it on or off, drag to reorder, edit it, or
  reset a kit command. *Add a command* searches what the project already has
  (`php artisan` commands, composer and package.json scripts, Makefile
  targets) so you pick instead of typing.
- **First-open setup.** The first time you open a project with a kit, four
  short questions set it up; *Use defaults* skips them.

### Messages

- **Messages per branch, and Clear.** Each feature branch's Messages tab lists
  only what its own agents sent and received. *Clear* empties a tab when
  you're done (replies still on their way stay).

### Reliability

- **No leftover dev servers.** Dev servers and anything started in a terminal
  tab are stopped when you close the tab, when its shell exits, and — if
  tterminal ever stops unexpectedly — the next time it opens. Your servers
  keep their usual ports.
- **No more "address already in use" loops.** `php artisan serve` gets a free
  port when others are taken (a `SERVER_PORT` in your `.env` still wins).
- **Restart without the flicker.** Restarting keeps every process and its
  output on screen until each one is back.

### Changed

- **Browser settings are now Links.** Project settings › *Links* holds the
  project's URLs (site, admin, Telescope, Mailpit, docs) and the browser they
  open in. Your existing shortcuts are kept.

### Removed

- **The built-in browser and Share.** Pages open in your own browser.
- **The built-in file editor and the Files tab.** Files open in your own
  editor (*Open in editor* wherever a path is shown).
- **The History tab.** Use `git log`, your editor or GitHub for history.
- **The menu bar item.** CPU and RAM stay in the app's header.

Saved browser, editor and commit tabs are dropped when you update.
