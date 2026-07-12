# software-craft

A [Claude Code](https://code.claude.com) plugin: a curated set of slash commands
for high-level software engineering — architecture, Domain-Driven Design,
Architecture Decision Records, quality & maintenance, and content processing.

## Install

```shell
/plugin marketplace add jdinkla/claude-marketplace
/plugin install software-craft@jdinkla-marketplace
```

Pull updates later with `/plugin marketplace update jdinkla-marketplace`.

## Documentation

- [`CLAUDE.md`](CLAUDE.md) — repository structure, the command groups, key
  principles, and the conventions for writing commands.
- [`commands/`](commands/) — one self-contained `.md` file per command; the
  filename is the command name, the frontmatter `description` says what it does.
- [`specs/`](specs/) — background and design rationale (e.g. single- vs
  multi-prompt design).

## License

MIT.
