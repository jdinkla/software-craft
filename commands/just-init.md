---
description: Inspect the current repo and generate or improve a customized justfile (just command runner) tailored to its language, tooling, and workflows.
argument-hint: "[--force to fully overwrite an existing justfile instead of improving it]"
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(just*), Bash(ls*), Bash(cat*), Bash(find*), Bash(test*)
---

## Goal

Create **or improve** a `justfile` at the root of the **current repository**,
modeled on the conventions below but **customized to whatever this repo actually
is**. A justfile is a set of named command recipes run via the
[`just`](https://github.com/casey/just) command runner. The point is to give
this repo a single, discoverable set of workflow shortcuts (`just --list`).

**Two modes — decide based on whether a justfile already exists:**

- **No justfile exists** → generate a new one from scratch (Step 2).
- **A justfile already exists** → **improve the existing one in place**
  (Step 2b). Do NOT regenerate from scratch and do NOT clobber it. Keep every
  existing recipe and the project's own conventions; add what's missing, fix
  what's broken, and bring it up to the house style.

Arguments: `$ARGUMENTS` — `--force` is only relevant for an existing justfile:
it permits a full from-scratch rewrite instead of an in-place improvement.
Without `--force`, an existing justfile is **improved, never clobbered** (see
Safety).

## Step 1 — Inspect the repo

Do NOT assume the stack. Detect it. Look for:

- **Language / package manager** — e.g. `pyproject.toml` + `uv.lock` (uv),
  `requirements.txt`/`Pipfile` (pip/pipenv/poetry), `package.json` +
  lockfile (npm/pnpm/yarn/bun), `Cargo.toml` (cargo), `go.mod` (go),
  `pom.xml`/`build.gradle` (maven/gradle), `Gemfile`, `composer.json`,
  `Makefile`, `*.csproj`/`*.sln`, `mix.exs`, etc.
- **Lint / format / type tools** — ruff, black, flake8, mypy, pyright,
  eslint, prettier, biome, clippy/rustfmt, golangci-lint/gofmt, etc.
  Check config files AND dev-dependencies, don't just guess.
- **Test runner** — pytest, jest/vitest, `cargo test`, `go test`,
  `npm test`, junit, rspec, etc. Note any test markers/groups
  (e.g. unit vs integration) the project already uses.
- **Entry points / CLIs** — `[project.scripts]`, `bin` in package.json,
  `cmd/` dirs, declared binaries — these become run targets.
- **Existing automation** — a `Makefile`, CI workflows
  (`.github/workflows/*`), `tox.ini`, `noxfile.py`, npm scripts,
  pre-commit hooks. **Mine these for the real commands the project uses**
  and translate them into just recipes rather than inventing commands.
- **Env loading** — presence of `.env` / `.env.example` → add
  `set dotenv-load := true`.
- An **existing justfile** (`justfile`/`Justfile`/`.justfile`) — read it
  fully. If one exists, you're in **improve mode** (Step 2b): note its existing
  recipes, groups, variables, and conventions so you extend rather than replace
  them.

Use Glob/Grep/Read and a few `ls`/`cat` calls. Be quick but accurate — a wrong
package manager makes every recipe useless.

## Step 2 — Generate the justfile (no justfile exists)

Follow these conventions (the house style, taken from the reference repo):

1. **Header** comment block naming the repo + a `Run \`just --list\`` hint, and
   a one-line usage example if the repo has a dominant workflow.
2. A **`default:`** recipe that runs `@just --list --unsorted`.
3. `set dotenv-load := true` **only if** the repo has a `.env`/`.env.example`.
4. Group recipes under `# --- Section ---` divider banners, in this rough order
   (skip sections that don't apply): **Dev** (setup/lint/format/typecheck/test),
   then any **project-specific** workflow groups.
5. **Annotate every recipe with `just` attributes**, matching the reference:
   - `[group('<name>')]` to assign it to a section group (`dev`, `build`,
     `test`, plus any domain-specific groups you identify).
   - `[doc("<short imperative description>")]` — this is what `just --list` shows.
   Place both attribute lines directly above the recipe. Example:
   ```just
   [group('dev')]
   [doc("Run unit tests")]
   test *ARGS:
       <test command> {{ARGS}}
   ```
6. Prefer the project's **own** invocation style (e.g. `uv run pytest`,
   `npm run test`, `cargo test`, `go test ./...`) detected in Step 1 — do NOT
   hardcode `uv`.
7. Provide a small composite **`check:`** recipe chaining lint + format-check +
   typecheck when those tools exist (`check: lint fmt typecheck`).
8. Use `*ARGS` / parameters for recipes that should pass through flags
   (e.g. `test *ARGS:` forwarding to the runner), and named params with
   defaults where the reference does (`recipe ARG DEFAULT="x":`).
9. Only include recipes whose underlying tool/command actually exists in this
   repo. A justfile full of broken commands is worse than a short correct one.

Reference shape (Python/uv example — adapt names/commands to the detected
stack; do NOT copy the `uv run` commands verbatim):

```just
# justfile for <repo-name> — common workflow shortcuts
# Run `just --list` to see all targets

set dotenv-load := true

# --- Dev ---

[group('dev')]
[doc("Show available commands")]
default:
    @just --list --unsorted

[group('dev')]
[doc("Install dependencies")]
install:
    <install command>

[group('dev')]
[doc("Lint code")]
lint:
    <lint command>

[group('dev')]
[doc("Format code")]
fmt:
    <format command>

[group('dev')]
[doc("Type-check")]
typecheck:
    <typecheck command>

[group('dev')]
[doc("Run all quality checks")]
check: lint fmt typecheck

[group('dev')]
[doc("Run tests")]
test *ARGS:
    <test command> {{ARGS}}
```

## Step 2b — Improve the existing justfile (a justfile exists)

When a justfile is already present, **edit it in place** with surgical changes
rather than rewriting it. Treat the existing file as the source of truth for the
project's conventions and respect them. Use the `Edit` tool, not a wholesale
`Write`.

Apply these improvements, in order of priority:

1. **Fix breakage first** — recipes that reference tools/commands not present in
   the repo (per Step 1), wrong package manager, typos, broken `{{...}}`
   interpolation, or recipes that would fail to parse. A correct existing recipe
   is never "improved" into a different command without reason.
2. **Add missing standard recipes** the repo can actually support but the
   justfile lacks — e.g. `lint`, `fmt`, `typecheck`, `test`, `install`, a
   `check:` composite, a `default:` listing recipe. Only add recipes whose
   underlying tool exists (Step 1's rule #9 still applies).
3. **Mine new automation** — if CI workflows, a Makefile, npm scripts, or
   pre-commit hooks contain commands not yet represented as recipes, add them.
4. **Bring up to house style** without churning working recipes:
   - Add `[group('<name>')]` and `[doc("...")]` attributes to recipes missing
     them, matching the conventions in Step 2.
   - Add a `default:` recipe (`@just --list --unsorted`) if absent.
   - Add `set dotenv-load := true` if the repo has `.env`/`.env.example` and the
     justfile doesn't already set it.
   - Add `# --- Section ---` divider banners if the file groups recipes loosely.
5. **Preserve everything project-specific** — custom recipes, variables,
   parameters, comments, and ordering the author chose. Do not reorder or rename
   existing recipes unless it fixes an actual problem; renaming a recipe breaks
   anyone who calls it.

If the existing justfile is already in good shape, make only the few edits that
genuinely help (or none) and say so — do not manufacture changes.

`--force` overrides this step: with `--force`, do a full from-scratch
regeneration per Step 2, still preserving project-specific recipes you can't
otherwise reconstruct.

## Step 3 — Write and verify

- **Safety**: if a `justfile` (or `Justfile`/`.justfile`) already exists and
  `--force` was not passed, do NOT overwrite it wholesale — **improve it in
  place** via `Edit` (Step 2b). Only do a full `Write`/overwrite of an existing
  justfile when `--force` was passed.
- For a new justfile, write the file to the repo root.
- If `just` is installed, run `just --list` to confirm it parses and the
  descriptions render. If `just` is missing, note how to install it
  (`brew install just` / `cargo install just` / package manager) and verify
  by re-reading the file instead.
- Report a one-line summary: for a new file, detected stack + the recipes you
  created; for an improved file, what you changed (recipes added / fixed /
  annotated) or that it was already in good shape.
