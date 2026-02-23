---
name: init
description: Install codex rules to ~/.claude/rules/. Self-destructs after running so it reappears when the plugin updates with new rules.
---

Install interaction and workflow rules from the codex plugin to `~/.claude/rules/` using prefixed filenames (`codex-*.md`).

## Steps

1. **Hash compare** — Run this bash script to produce a manifest. Do NOT read any rule file contents.

```bash
DEST="$HOME/.claude/rules"
mkdir -p "$DEST"

echo "=== STATUS ==="
for src in "${CLAUDE_PLUGIN_ROOT}"/rules/*.md; do
  [ -f "$src" ] || continue
  name="codex-$(basename "$src")"
  dest="$DEST/$name"
  if [ ! -f "$dest" ]; then
    echo "NEW $name"
  elif [ "$(md5 -q "$src")" = "$(md5 -q "$dest")" ]; then
    echo "UNCHANGED $name"
  else
    echo "UPDATED $name"
  fi
done

echo "=== MIGRATE ==="
for src in "${CLAUDE_PLUGIN_ROOT}"/rules/*.md; do
  [ -f "$src" ] || continue
  old="$DEST/$(basename "$src")"
  [ -f "$old" ] && echo "OLD $(basename "$src")"
done
```

2. **Show summary** — Format the manifest as a markdown table:

| File | Status |
|------|--------|
| `codex-communication.md` | NEW / UNCHANGED / UPDATED |

If any `OLD` entries exist, add a migration note: "Found unprefixed files that will be removed: ..."

3. **Ask** — Single AskUserQuestion:
   - "Install all (Recommended)" — copy all NEW + UPDATED files
   - "Install new + updated only" — same behavior, just an explicit label
   - "Skip" — do nothing

If everything is UNCHANGED and no OLD files exist, skip the question and report "All codex rules are up to date."

4. **Install** — If not skipped, run:

```bash
DEST="$HOME/.claude/rules"
for src in "${CLAUDE_PLUGIN_ROOT}"/rules/*.md; do
  [ -f "$src" ] || continue
  cp "$src" "$DEST/codex-$(basename "$src")"
done
```

5. **Remove old unprefixed** — If OLD files were detected, run:

```bash
DEST="$HOME/.claude/rules"
for src in "${CLAUDE_PLUGIN_ROOT}"/rules/*.md; do
  [ -f "$src" ] || continue
  old="$DEST/$(basename "$src")"
  [ -f "$old" ] && rm "$old" && echo "Removed: $(basename "$src")"
done
```

6. **Self-destruct** — Delete this command from the plugin cache:

```bash
rm -f "$HOME"/.claude/plugins/cache/*/codex/commands/init.md
```

Tell the user: "The /codex:init command has been removed from cache. It will reappear when the codex plugin updates."

7. **Summary** — Report counts: installed, updated, unchanged, migrated. Then display:

> The following CLAUDE.md sections are now covered by codex rules and can be removed from your CLAUDE.md:
> - Guiding Principles -> `codex-guiding-principles.md`
> - Working with Claude / Agents & Skills / Auto Memory / Agent Teams -> `codex-working-with-claude.md`
> - Plan Execution Discipline -> `codex-plan-execution.md`
> - Session Hygiene / Commit Discipline -> `codex-session-hygiene.md`
> - Communication / Bash Discipline -> `codex-communication.md`
> - Constraints -> `codex-constraints.md`

Remind user to restart Claude Code.

## Important

- Do NOT read rule file contents — the hash comparison handles everything
- Source: `${CLAUDE_PLUGIN_ROOT}/rules/` — Destination: `~/.claude/rules/`
- Prefix: every installed file gets `codex-` prepended to its basename
- Files in `~/.claude/rules/` NOT matching `codex-*` are user-managed and never touched
- Self-destruct targets the CACHE copy, not the source repo
- This command does NOT modify CLAUDE.md — the advisory is informational only
