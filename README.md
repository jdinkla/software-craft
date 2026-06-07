# software-craft

A [Claude Code](https://code.claude.com) plugin: a curated set of slash commands
and skills for high-level software engineering — architecture, Domain-Driven
Design, Architecture Decision Records, quality & maintenance, and content
processing.

## Install

```shell
/plugin marketplace add jdinkla/claude-marketplace
/plugin install software-craft@jdinkla-marketplace
```

Pull updates later with `/plugin marketplace update jdinkla-marketplace`.

## Commands

### Architecture & ADRs
| Command | What it does |
|---|---|
| `/adr-create` | Create a new Architecture Decision Record from notes |
| `/adr-refine` | Refine and improve an existing ADR |
| `/adr-review` | Review an ADR for quality and soundness (no rewrite) |
| `/arch-review-kotlin` | Architecture review of a Kotlin repository |

### Domain-Driven Design
| Command | What it does |
|---|---|
| `/ddd-analyse` | Extract the domain model from a legacy repository |
| `/qa-bssfn` | Audit the system against Dan North's "Best Simple System for Now" |

### Quality & Maintenance
| Command | What it does |
|---|---|
| `/tech-debt` | Identify concrete technical debt in the repository |
| `/test-coverage` | Analyze test coverage from a JaCoCo report (branch coverage) |
| `/test-naming` | Check unit-test naming consistency |
| `/qa-docs` | Classify project docs (Diátaxis) and propose reorganization |
| `/check-agents-md` | Verify outputs comply with `AGENTS.md` principles |

### Content processing
| Command | What it does |
|---|---|
| `/md-summarize` | Turn unstructured text into structured professional markdown |
| `/md-condense` | Condense markdown to its logical skeleton |
| `/md-translate` | Translate German markdown to English (preserving formatting) |
| `/x-summarize` | Summarize a blog post into a 250-char X teaser |
| `/joke-statler-waldorf` | Summarize and roast input Statler-&-Waldorf style |
| `/normal` | Just follow the request, output markdown |

## Skills

Auto-invoked / user-invocable capabilities (see `skills/`):

- **work** — start working on the task matching the current git branch
  (uses [beads](https://github.com/steveyegge/beads) `bd`).

> Note: `work` is tailored to a specific personal workflow (beads issues).
> Adapt it to your project.

## Roles

`AGENTS.md` groups the commands into three engineering roles — Software
Architect, DDD Practitioner, and Quality & Maintenance Engineer — and lists the
general principles all of them follow. `specs/` holds background and design
rationale (e.g. single- vs multi-prompt design).

## License

MIT.
