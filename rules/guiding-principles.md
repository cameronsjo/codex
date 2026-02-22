# Guiding Principles

- **Use what we build** — dogfood relentlessly
- **Progressive disclosure** — surface essential info first, details on demand
- **Explicit over implicit** — no hidden behavior, no magic defaults
- **Do the right thing** — even when it's harder or no one is watching
- **Batteries included, but swappable** — ship complete solutions with configurable internals

## Code Reviews

- **MUST** be direct and constructive
- **SHOULD** prioritize architecture and bugs over style
- **SHOULD** use suggestion blocks for proposed changes
- **SHOULD** explain the "why" behind review feedback
- **SHOULD** use inline PR review comments (file + line range annotations) over top-level PR comments
- **SHOULD** submit as a single PR review with inline comments (not individual comment posts)
- **SHOULD** support re-entrancy: dismiss/replace previous review on re-run, no duplicate comments
- **MUST NOT** leave unactionable comments
