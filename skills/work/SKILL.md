---
name: work
description: Start working on the task matching the current branch name
user-invocable: true
---

# Work on Current Branch Task

Follow these steps to start working on the current task:

1. **Read project instructions**: Load and understand `CLAUDE.md` in the repository root

2. **Get current branch**: Run `git branch --show-current` to identify the current branch

3. **Find the matching task**: Use `bd show <branch-name>` to view the task details
   - The branch name should match a beads issue ID (e.g., `beads-123`)
   - If the branch is `main`, inform the user there's no task to work on for main branch

4. **Claim the task if needed**: If status is not `in_progress`, run:
   ```
   bd update <branch-name> --status=in_progress
   ```

5. **Work on the task**: Based on the task description and requirements:
   - Break down the work into steps using TodoWrite
   - Implement the solution
   - Follow any conventions specified in CLAUDE.md
   - you find the ports for frontend and backend in .env. Do not assume 3000.

6. **When complete**: Follow the session close protocol:
   - `git status` - check changes
   - `git add <files>` - stage code changes
   - `bd sync` - commit beads changes
   - `git commit -m "..."` - commit code
   - `bd close <branch-name>` - close the task
   - `bd sync` - sync beads
   - `git push` - push to remote

Work is NOT complete until pushed to remote.
