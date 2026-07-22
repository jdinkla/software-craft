---
name: handover
description: Write a handover.md capturing the current session's state, decisions, and next steps so the next agent session can continue after a /clear.
argument-hint: [output-path]
allowed-tools: Bash, Read, Write, Glob, Grep
---

# /handover — write a handover file for the next session

You are about to lose your conversation context (the user will run `/clear` or
start a new session). Write a handover file so the next agent session can pick
up the work without re-deriving anything.

Output path: `$ARGUMENTS` if given, else `handover.md` at the repo root (or
the current directory if not in a repo).

## The one rule that matters

Code, git history, and CLAUDE.md survive a `/clear` — the conversation does
not. Spend the handover on what is **only in the conversation**:

- decisions made and *why* (including options that were rejected)
- dead ends already tried, so the next session doesn't repeat them
- what is done-and-verified vs. done-but-unverified vs. not started
- surprises and gotchas discovered along the way
- the user's actual intent, where it is broader or narrower than the diff suggests

Do **not** summarize file contents, restate what `git log` shows, or explain
the codebase — the next session can read those itself. Point at them instead
(`file:line`, commit hashes).

## Steps

1. Capture repo state: current branch, `git status --short`, and a one-line
   summary of uncommitted changes (`git diff --stat`). List any untracked
   files that belong to the work in progress.
2. If a task tool is in use (e.g. `backlog/`), note the ID and status of the
   task(s) being worked on rather than duplicating their content.
3. Write the handover file using the template below. Overwrite an existing
   handover file — it describes a previous session and is now stale.
4. Confirm to the user: path written, and remind them the next session should
   start with "read handover.md".

## Template

```markdown
# Handover — <YYYY-MM-DD>

> For the next agent session: read this, then delete it once you have
> absorbed it. It describes conversation state, not repo state — verify
> anything time-sensitive against the actual repo.

## Task
<What we are doing and why, in one or two sentences. The user's goal, not
the mechanical activity.>

## State
- **Done, verified:** <with how it was verified — test run, manual check>
- **Done, NOT verified:** <be honest here; this is the most valuable line>
- **In progress:** <exactly where work stopped, mid-file if applicable>
- **Not started:** <known remaining work>

## Repo state
<Branch, uncommitted changes summary, relevant commits by hash. Untracked
files that are part of the work.>

## Next steps
1. <Ordered, concrete, starting with the very next action.>

## Decisions & why
- <Decision> — <reason>. <Rejected alternative, if discussed.>

## Dead ends — do not retry
- <What was tried, why it failed.>

## Gotchas
- <Non-obvious things discovered: flaky test, misleading error, env quirk.>

## Open questions for the user
- <Anything blocked on user input. Omit section if none.>
```

## Constraints

- Write for a reader with **zero** conversation access. No "as discussed",
  no shorthand invented during the session.
- Keep it short: a handover the next session won't finish reading is a
  failed handover. Aim for under a page; cut the least valuable material
  first, never the "NOT verified" and "dead ends" sections.
- Report state faithfully: if tests fail or something was skipped, the
  handover says so plainly.
- If there is no meaningful work in progress, say so to the user and write
  nothing — an empty handover is noise for the next session.
