# Codex

Personal codex for Claude Code — how I work, communicate, and manage sessions. Opinionated, universal, mine.

Designed to be installed alongside the [rules](https://github.com/cameronsjo/rules) plugin. Rules covers *how to write code*. Codex covers *how to work with me*.

## Rules

All rules are unglobbed (no `paths:` frontmatter) — they load every session, everywhere.

| File | Domain |
|------|--------|
| `guiding-principles.md` | Core tenets, code review patterns |
| `working-with-claude.md` | Agents, skills, auto memory, teams |
| `plan-execution.md` | Plan discipline, context resilience |
| `session-hygiene.md` | Session lifecycle, commit discipline |
| `communication.md` | Writing style, bash discipline |
| `constraints.md` | Universal behavioral guardrails |

## Install

```bash
# Via workbench hub
claude plugin install codex@workbench
claude plugin enable codex@workbench

# Then run the init command to copy rules to ~/.claude/rules/
/codex-init
```

## Architecture

```
Personal env ◄── codex plugin ──► Work env
  (author)        (public)         (read-only)
```

Codex flows downstream. Work installs it but never writes back upstream. Each environment's `CLAUDE.md` retains only private/local content.

## License

MIT
