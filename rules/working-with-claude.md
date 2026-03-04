---
notice: "Maintained by the codex plugin. Source: github.com/cameronsjo/codex"
---
# Working with Claude

## Core Patterns

- **MUST** provide code first, explanations after — complete working examples with error handling
- **MUST** check obvious issues first (typos, imports, env)
- **MUST** use TodoWrite for multi-step tasks
- **MUST** create and maintain knowledge bases in repos/projects for context between sessions

## Using Agents & Skills

**Core Principle:** Use agents for complex/multi-step tasks. Use direct tools for simple operations.

**When to Use Agents:**

- **Exploration**: "find X", "how does Y work?", "where is Z?" -> use `Explore` agent
- **Multi-step research**: Search, analyze, and synthesize across multiple files
- **Specialized work**: python-expert, security-auditor, mcp-expert, etc.
- **Parallel work**: Launch multiple agents simultaneously for independent tasks
- **Complex debugging**: Investigation requiring multiple rounds of searching/testing

**When NOT to Use Agents:**

- Reading 1-2 specific known files (use `Read`)
- Searching for exact class/function name (use `Glob`/`Grep`)
- Simple edits to known files (do directly)

**Key Practices:**

- Launch agents in parallel when possible (single message, multiple Agent calls)
- Specify thoroughness for Explore: "quick", "medium", or "very thorough"

## Auto Memory

Claude automatically saves useful context to auto memory across sessions. Stored in `~/.claude/projects/<project>/memory/`. First 200 lines of `MEMORY.md` load at session start; topic files read on demand. Manage via `/memory`. Override path with `CLAUDE_CODE_AUTO_MEMORY_PATH` env var.

- **MUST** keep `MEMORY.md` under 200 lines — move details to topic files
- **SHOULD** review and prune stale or incorrect memories periodically
- **SHOULD** manually save key decisions, architecture patterns, and workflow preferences that auto-save might miss

## Agent Teams

Agent teams coordinate multiple Claude Code instances working together. Use for parallel reviews, cross-layer changes needing coordination, multi-angle research, or large refactors. Don't use for tasks a single subagent handles fine, sequential work, or quick lookups.

- **MUST** size tasks appropriately — target 5-6 tasks per teammate
- **MUST** avoid file conflicts — two teammates editing the same file causes overwrites
- **SHOULD** start with research/review tasks before using teams for code-writing
- **SHOULD** give spawn prompts full context — teammates get CLAUDE.md but NOT the lead's conversation history
- **SHOULD** encourage running in tmux so the user can observe agents working in parallel

### tmux Quick Reference

Suggest before spawning a team:

> **Getting in:** `tmux new -s agents` (or use `Ctrl+a T` / `Cmd+K` to pick an existing session via sesh)
>
> **Splitting panes:** `Ctrl+a |` (side-by-side) · `Ctrl+a -` (stacked)
>
> **Moving between panes:** `Ctrl+a h/j/k/l` (vim-style) · `Alt+Arrow`
>
> **Zooming:** `Ctrl+a z` (toggle fullscreen on current pane)
>
> **Scrolling:** `Ctrl+a [` (scroll mode, `j/k` to scroll, `q` to exit)
>
> **Cleanup:** `Ctrl+a x` (close pane) · `cs` (full cheatsheet)
