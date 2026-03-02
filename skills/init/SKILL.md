---
name: init
description: Install codex rules to ~/.claude/rules/codex/. Self-destructs after running so it reappears when the plugin updates with new rules.
---

Install all codex rules to `$HOME/.claude/rules/codex/`. Installs everything by default — no scope selection needed.

## Steps

1. **Migrate** — Detect and move old flat files from the previous prefix scheme:

```bash
OLD_DEST="$HOME/.claude/rules"
NEW_DEST="$HOME/.claude/rules/codex"
mkdir -p "$NEW_DEST"

echo "=== MIGRATE ==="
for old in "$OLD_DEST"/codex-*.md; do
  [ -f "$old" ] || continue
  basename="${old##*/}"
  stripped="${basename#codex-}"
  if [ ! -f "$NEW_DEST/$stripped" ]; then
    mv "$old" "$NEW_DEST/$stripped"
    echo "MIGRATED $basename -> codex/$stripped"
  else
    rm "$old"
    echo "REMOVED $basename (already exists in codex/)"
  fi
done
```

2. **Hash compare** — Run this bash script to produce a manifest. Do NOT read any rule file contents yet.

```bash
DEST="$HOME/.claude/rules/codex"

echo "=== STATUS ==="
for src in "${CLAUDE_PLUGIN_ROOT}"/rules/*.md; do
  [ -f "$src" ] || continue
  name="$(basename "$src")"
  dest="$DEST/$name"
  if [ ! -f "$dest" ]; then
    echo "NEW $name"
  elif [ "$(md5 -q "$src")" = "$(md5 -q "$dest")" ]; then
    echo "UNCHANGED $name"
  else
    echo "UPDATED $name"
  fi
done
```

3. **Show summary** — Format the manifest as a markdown table:

| File | Status |
|------|--------|
| `communication.md` | NEW / UNCHANGED / UPDATED |

If everything is UNCHANGED, report "All codex rules are up to date." and skip to step 6.

4. **Install** — For each rule based on status:

- **NEW**: Copy the source file to the destination:
  ```bash
  cp "$src" "$DEST/$(basename "$src")"
  ```

- **UPDATED**: Read both the source (plugin) file and the destination (installed) file using the Read tool. Merge: incorporate plugin updates while preserving user customizations (added rules, modified wording, extra sections). Write the merged result to the destination using the Write tool.

- **UNCHANGED**: Skip.

5. **Self-destruct** — Delete this command from the plugin cache:

```bash
rm -f "$HOME"/.claude/plugins/cache/*/codex/*/commands/init.md
```

Tell the user: "The /codex:init command has been removed from cache. It will reappear when the codex plugin updates."

6. **Summary** — Report counts: installed, updated, unchanged, migrated. Then display:

> The following CLAUDE.md sections are now covered by codex rules and can be removed from your CLAUDE.md:
> - Guiding Principles -> `guiding-principles.md`
> - Working with Claude / Agents & Skills / Auto Memory / Agent Teams -> `working-with-claude.md`
> - Plan Execution Discipline -> `plan-execution.md`
> - Session Hygiene / Commit Discipline -> `session-hygiene.md`
> - Communication / Bash Discipline -> `communication.md`
> - Constraints -> `constraints.md`

Remind user to restart Claude Code.

## Important

- Source: `${CLAUDE_PLUGIN_ROOT}/rules/` — Destination: `~/.claude/rules/codex/`
- No prefix — files keep their original basename
- Files outside `codex/` are user-managed and never touched
- For UPDATED files, READ both source and destination, MERGE intelligently, then WRITE. Do NOT overwrite blindly
- Self-destruct targets the CACHE copy, not the source repo
- This command does NOT modify CLAUDE.md — the advisory is informational only
