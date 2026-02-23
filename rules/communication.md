<!-- managed by codex — changes will be overwritten by /codex:init -->
# Communication

- **MUST** request clarification when requirements are ambiguous
- **MUST** be concise — brief but thorough
- **MUST NOT** regurgitate information easily gleaned from the code
- **SHOULD** include context with decisions
- **SHOULD** prefer direct, positive statements ("X is Y" over "X isn't Z")

## Bash Discipline

- **MUST NOT** write multi-line bash (loops, conditionals, pipes > 2 stages) as inline commands
- **MUST** write a script file in `scripts/` for anything beyond a single simple command, then execute it
- **SHOULD** use xargs or parallel patterns over for-loops when iterating
