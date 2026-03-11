# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

A collection of specialized prompt templates (`.md` files) for AI agents, focused on software architecture, engineering analysis, and content processing. There is no build system, no tests, and no compiled code — just markdown files with YAML frontmatter.

## Repository Structure

- **Root `.md` files** — Standalone prompt templates, each defining a single agent role or task. Frontmatter fields: `name`, `description`, `argument-hint`.
- **`commands/`** — Claude Code slash command definitions (e.g., `qa-docs.md`, `qa-bssfn.md`). These use frontmatter fields like `allowed-tools` and `model`.
- **`skills/`** — Claude Code skill definitions with `SKILL.md` files. Each skill has its own subdirectory (e.g., `skills/work/`, `skills/check-models/`).
- **`specs/`** — Background research and design rationale (e.g., `PromptWriting.md` on single vs. multiple prompt design).
- **`AGENTS.md`** — Defines the three agent roles (Software Architect, DDD Practitioner, Quality & Maintenance Engineer) and their general principles.

## Agent Roles (from AGENTS.md)

Three specialized roles, each with assigned prompt templates:

1. **Software Architect** — ADR creation/refinement/review, Kotlin architecture review (`adr-*.md`, `arch-review-kotlin.md`)
2. **DDD Practitioner** — Domain analysis, bounded contexts, ubiquitous language (`ddd-analyse.md`, via `qa-bssfn.md`)
3. **Quality & Maintenance Engineer** — Tech debt, test coverage, test naming (`tech-debt.md`, `test-coverage.md`, `test-naming.md`)

Additional utility prompts handle content processing: `md-summarize.md`, `md-condense.md`, `md-translate.md`, `x-summarize.md`, `joke-statler-waldorf.md`.

## Conventions for Prompt Templates

- Each prompt is a self-contained `.md` file with YAML frontmatter (`---` delimited block at top)
- Prompts define a clear **role**, **instructions**, **output format**, and often **constraints**
- Design principle: prefer separate single-purpose prompts over one multi-mode prompt (see `specs/PromptWriting.md`)
- Use `check-agents-md.md` to verify agent outputs comply with AGENTS.md principles

## Key Principles (from AGENTS.md)

1. Be concise but precise — actionable insights, clear evidence
2. Context first — understand constraints before suggesting changes
3. No hidden assumptions — state assumptions explicitly
4. Follow the structure — adhere to output formats defined in templates
5. Incremental over radical — prefer small improvements over rewrites
