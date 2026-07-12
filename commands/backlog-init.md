---
name: backlog-init
description: Initialize the Backlog.md task tool in the current repo with the standard configuration (auto-commit on, no browser auto-open, editor code, bypass git hooks), update CLAUDE.md with the backlog workflow block, and commit the result.
argument-hint: [web-ui-port]
allowed-tools: Bash, Read, Edit
---

# /backlog-init — set up Backlog.md in the current repo

Initializes [Backlog.md](https://backlog.md) in the repo you are standing in,
matching the reference configuration used in the `news` repo. After this the
`backlog` CLI is usable, `CLAUDE.md` carries the managed backlog workflow
block, and the init is committed.

## Step 0 — Preconditions

- `backlog` must be on PATH (`backlog --version`). If missing, stop and tell
  the user (`npm i -g backlog.md` or `brew install backlog-md`).
- Must be run inside a git repository (the config relies on git integration).
  If not one, ask before `git init`.
- If `backlog/config.yml` already exists, the repo is already initialized —
  report that and stop. Do not re-run init over an existing board.

## Step 1 — Derive the variables

- `PROJECT` = basename of the git root (`basename "$(git rev-parse --show-toplevel)"`)
- `PORT` = `$ARGUMENTS` if given, else `6420`. Backlog's web UI binds
  this port; only matters when several repos run the UI simultaneously.

## Step 2 — Initialize

Run from the git root:

```bash
backlog init "$PROJECT" --defaults \
  --integration-mode cli \
  --agent-instructions claude \
  --check-branches true \
  --branch-days 30 \
  --bypass-git-hooks true \
  --default-editor code \
  --web-port "$PORT" \
  --auto-open-browser false \
  --task-prefix task
```

This creates `backlog/` with `config.yml` and appends the managed
`BACKLOG.MD GUIDELINES` block to `CLAUDE.md` (creating the file if absent).

Init cannot set `auto_commit`, so set it afterwards (never edit the YAML by
hand):

```bash
backlog config set autoCommit true
```

If init warns that `remoteOperations` is enabled but no git remote exists,
relay the warning — it is expected in fresh repos and resolves itself once a
remote is added; do not disable remoteOperations.

## Step 3 — Verify

The resulting `backlog/config.yml` must match this reference (only
`project_name` and `default_port` may differ):

```yaml
default_status: "To Do"
statuses: ["To Do", "In Progress", "Done"]
labels: []
date_format: yyyy-mm-dd
max_column_width: 20
default_editor: "code"
auto_open_browser: false
remote_operations: true
auto_commit: true
filesystem_only: false
bypass_git_hooks: true
check_active_branches: true
active_branch_days: 30
task_prefix: "task"
```

Then confirm the tool works: `backlog task list` (empty board is fine) and
check that `CLAUDE.md` contains the `BACKLOG.MD GUIDELINES START` marker.

## Step 4 — Commit

Stage and commit only what the init produced (message inline with `-m`):

```bash
git add backlog/ CLAUDE.md
git commit -m "Initialize Backlog.md task board"
```

If the repo has its own commit-message convention (see its CLAUDE.md), follow
that instead.

## Step 5 — Report

Tell the user: config written, CLAUDE.md updated, commit hash, the chosen
port, and any remote-operations warning.
