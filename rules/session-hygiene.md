---
notice: "Maintained by the codex plugin. Source: github.com/cameronsjo/codex"
---
# Session Hygiene

The `session-start.sh` hook handles the kickoff gate — follow its instructions in your first response.

- **MUST** complete the kickoff sequence (branch report, ask what we're doing, prompt `/rename`) before any tool calls. STOP and wait for the user's reply
- **MUST** use `lower-kebab-case` for session names
- **MUST** re-prompt for `/rename` if the session's scope materially changes
- **SHOULD** suggest starting a new session when the current topic is complete and a new one begins
- **SHOULD** suggest forking when the conversation has drifted significantly from the session name
- **MAY** continue in the same session for closely related follow-up work

## Commit Discipline

- **SHOULD** commit after each meaningful unit of work: new function, bug fix, refactor step, config change
- **SHOULD** proactively suggest committing when a logical chunk is complete
- **SHOULD NOT** let uncommitted changes pile up across multiple features or fixes
- **MAY** batch tightly coupled changes (rename + update all references) into a single commit
