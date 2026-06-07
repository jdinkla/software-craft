# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This repository **is a Claude Code plugin** named `software-craft`, published via the marketplace at `jdinkla/claude-marketplace` (it appears there as a `github` source pointing at this repo). It bundles slash commands for software architecture, engineering analysis, and content processing. There is no build system, no tests, and no compiled code — just markdown files with YAML frontmatter.

## Repository Structure

- **`.claude-plugin/plugin.json`** — The plugin manifest (`name`, `description`, `version`, `author`). The plugin name is `software-craft`.
- **`commands/`** — Claude Code slash command definitions, one `.md` per command (e.g., `adr-create.md`, `tech-debt.md`, `qa-docs.md`). The command name comes from the filename, so `adr-create.md` is invoked as `/adr-create`. Frontmatter fields: `description`, `argument-hint`, and optionally `allowed-tools`, `model`.
- **`specs/`** — Background research and design rationale (e.g., `PromptWriting.md` on single vs. multiple prompt design).
- **`AGENTS.md`** — Defines the three agent roles (Software Architect, DDD Practitioner, Quality & Maintenance Engineer) and their general principles.

## Working on the plugin

- Add a new command by creating `commands/<name>.md` with at least a `description` in frontmatter.
- After changing any manifest or frontmatter, run `claude plugin validate .` from the repo root.
- Installs resolve from the pushed GitHub repo, so changes must be committed and pushed before users see them.

## Agent Roles (from AGENTS.md)

Three specialized roles, each with assigned commands (in `commands/`):

1. **Software Architect** — ADR creation/refinement/review (`/adr-create`, `/adr-refine`, `/adr-review`)
2. **DDD Practitioner** — Domain analysis, bounded contexts, ubiquitous language (`/ddd-analyse`, `/qa-bssfn`)
3. **Quality & Maintenance Engineer** — Tech debt, test coverage, test naming (`/tech-debt`, `/test-coverage`, `/test-naming`)

Additional utility commands handle content processing: `/md-summarize`, `/md-condense`, `/md-translate`, `/x-summarize`, `/joke-statler-waldorf`.

## Conventions for Commands

- Each command is a self-contained `commands/<name>.md` file with YAML frontmatter (`---` delimited block at top); the command name comes from the filename
- Commands define a clear **role**, **instructions**, **output format**, and often **constraints**
- Design principle: prefer separate single-purpose commands over one multi-mode command (see `specs/PromptWriting.md`)
- Use `/check-agents-md` to verify agent outputs comply with AGENTS.md principles

## Key Principles (from AGENTS.md)

1. Be concise but precise — actionable insights, clear evidence
2. Context first — understand constraints before suggesting changes
3. No hidden assumptions — state assumptions explicitly
4. Follow the structure — adhere to output formats defined in templates
5. Incremental over radical — prefer small improvements over rewrites
