---
name: init
description: Install codex rules to ~/.claude/rules/. Self-destructs after running so it reappears when the plugin updates with new rules.
---

Install interaction patterns, session management, and workflow rules from the codex plugin to the user's global Claude Code rules directory.

## Steps

1. **Show available rules** - List all `.md` files in this plugin's `rules/` directory (relative to `${CLAUDE_PLUGIN_ROOT}`), showing which ones already exist in `~/.claude/rules/` and which are new or updated.

2. **Detect conflicts** - For each rule that already exists in `~/.claude/rules/`, diff the plugin version against the installed version. Categorize each file as:
   - **New** - doesn't exist yet
   - **Unchanged** - identical content, no action needed
   - **Conflict** - both exist with different content

3. **Detect filename conflicts** - Check for same-purpose rules with different filenames that would cause silent duplicate loading. Known aliases to check:
   - `guiding-principles.md` vs `core-tenets.md`
   - `communication.md` vs `writing-style.md`
   - `constraints.md` vs `blocking-rules.md`
   If an old-named file exists and the plugin ships the new name, flag it and offer to remove the old file after installing the new one.

4. **Ask which to install** - Use AskUserQuestion to let the user choose:
   - "Install all" (recommended) - copies everything, overwrites conflicts with plugin version
   - "Install new only" - skip files that already exist in ~/.claude/rules/
   - "Review conflicts" - for each conflicting file, show a summary of differences and ask: overwrite, skip, or merge (append plugin-only sections)

5. **Handle conflicts** - When "Review conflicts" is selected:
   - For each conflicting file, read both versions and summarize the key differences
   - Use AskUserQuestion per file: "Overwrite with plugin version", "Keep existing", or "Merge (keep both, deduplicate)"
   - For merge: combine both files, remove duplicate sections, preserve user customizations that don't exist in the plugin version

6. **Copy rules** - For each selected rule file:
   - Copy to `~/.claude/rules/` with flat structure
   - Remove any flagged old-name duplicates the user approved for cleanup
   - Report what was installed, skipped, merged, and cleaned up

7. **Self-destruct** - After successful installation:
   - Find this plugin's cache directory by looking for `codex` plugin in `~/.claude/plugins/cache/`
   - Delete ONLY this command file from the cached copy: `~/.claude/plugins/cache/*/codex/commands/init.md`
   - Explain to the user: "The codex:init command has been removed from your local cache. It will reappear next time the codex plugin updates, prompting you to install any new or changed rules."

8. **Summary** - Show what was installed, skipped, merged, and cleaned up. Then display an advisory message:

   > The following CLAUDE.md sections are now covered by codex rules and can be removed from your CLAUDE.md:
   > - Guiding Principles → `guiding-principles.md`
   > - Working with Claude / Agents & Skills / Auto Memory / Agent Teams → `working-with-claude.md`
   > - Plan Execution Discipline → `plan-execution.md`
   > - Session Hygiene / Commit Discipline → `session-hygiene.md`
   > - Communication / Bash Discipline → `communication.md`
   > - Constraints → `constraints.md`

   Remind the user to restart Claude Code for rules to take effect.

## Important

- The source rules live at `${CLAUDE_PLUGIN_ROOT}/rules/` - read from there
- The destination is `~/.claude/rules/` (flat, no subdirectories) - write there
- When comparing existing vs new, use file content diff, not just existence
- Always check for filename aliases to prevent silent duplicate rule loading
- The self-destruct targets the CACHE copy, not the source repo
- This command does NOT modify CLAUDE.md — the advisory is informational only
