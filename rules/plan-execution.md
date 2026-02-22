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

- **SHOULD** treat commits as checkpoints for state recovery
- **SHOULD** externalize state (plans, task lists, spec files) so it survives compaction
- **SHOULD** prefer file-based state over conversational state
- **SHOULD** write key decisions and findings to auto memory before long operations
- **SHOULD** use TaskCreate as breadcrumbs — task lists survive compaction
