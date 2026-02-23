# Codex

Personal codex for Claude Code — how I work, communicate, and manage sessions. Opinionated, universal, mine.

Designed to be installed alongside the [rules](https://github.com/cameronsjo/rules) plugin. Rules covers *how to work with code*. Codex covers *how to work with me*.

## Rules

All rules are unglobbed (no `paths:` frontmatter) — they load every session, everywhere. Installed to `~/.claude/rules/` with a `codex-` prefix, creating an ownership boundary.

| Source file | Installed as | Domain |
|-------------|-------------|--------|
| `guiding-principles.md` | `codex-guiding-principles.md` | Core tenets, code review patterns |
| `working-with-claude.md` | `codex-working-with-claude.md` | Agents, skills, auto memory, teams |
| `plan-execution.md` | `codex-plan-execution.md` | Plan discipline, context resilience |
| `session-hygiene.md` | `codex-session-hygiene.md` | Session lifecycle, commit discipline |
| `communication.md` | `codex-communication.md` | Writing style, bash discipline |
| `constraints.md` | `codex-constraints.md` | Universal behavioral guardrails |

## Install

```bash
# Via workbench hub
claude plugin install codex@workbench
claude plugin enable codex@workbench

# Then run the init command to copy rules to ~/.claude/rules/
/codex:init
```

The init command uses md5 hashing to detect changes — it never reads file contents into the LLM context. After running, it self-destructs from cache and reappears when the plugin updates.

## Architecture

```
Personal env ◄── codex plugin ──► Work env
  (author)        (public)         (read-only)
```

Codex flows downstream. Work installs it but never writes back upstream. Each environment's `CLAUDE.md` retains only private/local content.

## License

MIT
