<!-- managed by codex — changes will be overwritten by /codex:init -->
# Plan Execution Discipline

Plans are contracts, not suggestions.

- **MUST** follow plan steps exactly as written
- **MUST** stop immediately when reality deviates from the plan (unexpected API, missing dependency, wrong assumptions)
- **MUST** undo all code changes from the current batch when a deviation is encountered
- **MUST** report what the plan didn't account for and replan with new information before resuming
- **MAY** make improvements during execution (better naming, combining steps, cleaner abstractions) — as long as plan intent is preserved
- **MUST NOT** silently adapt to deviations. If you chose to do it differently, that's an improvement. If reality forced you, that's a deviation and the plan is wrong

## Context Resilience

API rate limits during compaction and `/clear` erase detailed context. Externalize state to durable stores.

- **MUST** use feature branches — commits are checkpoints, branches are save files
- **MUST** mark known-broken commits: `wip: broken — <reason>`
- **SHOULD** commit aggressively — uncommitted work is invisible to recovery
- **SHOULD** use detailed commit messages as decision logs
- **SHOULD** externalize state (plans, task lists, spec files) so it survives compaction
- **SHOULD** prefer file-based state over conversational state
- **SHOULD** write key decisions and findings to auto memory before long operations
- **SHOULD** use TaskCreate as breadcrumbs — task lists survive compaction

## Plans

Two plan systems exist — prefer skill-based plans for anything non-trivial:

| | Native (`EnterPlanMode`) | Skill (`writing-plans`) |
|---|---|---|
| **Location** | `~/.claude/plans/<random>.md` | `docs/plans/YYYY-MM-DD-<name>.md` |
| **Naming** | Random adjective-noun | Date + descriptive name |
| **Scope** | Global, not project-tied | In project repo |
| **Survives `/clear`** | Path lost from context | Committed to git |
| **Discoverable** | Must glob + guess | `ls docs/plans/` |
| **Use for** | Quick, throwaway plans | Implementation plans |

- **MUST** write active plan path and summary to auto memory after any plan approval
- **MUST** clear the plan reference from auto memory when execution completes
- **SHOULD** prefer `writing-plans` skill for plans that will outlive the current session
- **SHOULD** re-read the plan file after `/clear` or context recovery
