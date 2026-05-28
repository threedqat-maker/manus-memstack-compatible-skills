---
name: project
description: "Use when the user asks for 'save project', 'handoff', or when context is running low and state must be preserved."
---

> **Conversion status: Needs manual review.** This skill was converted from a Claude/MemStack skill, but it contains runtime-specific assumptions that may not apply directly in Manus. Review before relying on it in production.
>
> **Review triggers:** $MEMSTACK_PATH, CC session, memstack-db.py.

> **Original source:** `cwinvestments/memstack/skills/project/SKILL.md`.

#  Project — Saving Project State...
*Save and restore project state between Manus sessions for seamless handoffs.*

## Protocol

### Saving (handoff):

1. **Capture current state:**
   - What was accomplished this session
   - What's currently in progress (uncommitted changes, partial work)
   - Open questions or decisions pending
   - Next steps in priority order
   - Key file paths that were modified
2. **Run git status** to capture uncommitted state
3. **Save project context to SQLite:**
   ```bash
   python "$MEMSTACK_PATH/db/memstack-db.py" set-context '{"project":"<name>","status":"active","current_branch":"<branch>","last_session_date":"<YYYY-MM-DD>","known_issues":"<issues>","backlog":"<next tasks>"}'
   ```
4. **Also save markdown handoff** to `memory/projects/{project}-{date}.md`
5. **Present the ready-to-paste prompt** for the next Manus session

### Loading (restore):

1. **Load project context from SQLite:**
   ```bash
   python "$MEMSTACK_PATH/db/memstack-db.py" get-context <project>
   ```
2. **Load recent sessions:**
   ```bash
   python "$MEMSTACK_PATH/db/memstack-db.py" get-sessions <project> --limit 3
   ```
3. **Load plan if exists:**
   ```bash
   python "$MEMSTACK_PATH/db/memstack-db.py" get-plan <project>
   ```
4. **Fallback:** Check `memory/projects/` for markdown handoffs
5. **Present combined state** so CC can continue immediately

## Inputs
- Project name
- Current session context (what was done, what's pending)

## Outputs
- Project context saved to SQLite database
- Markdown handoff in memory/projects/ (backup)
- Ready-to-paste prompt for next Manus session

## Example Usage

**User:** "context is running low — save project"

```
 Project — Saving project state...

Saved: memory/projects/adminstack-2026-02-18.md

--- PASTE INTO NEXT CC SESSION ---
Working directory: /path/to/your-project
Read $MEMSTACK_PATH/MEMSTACK.md

Resume AdminStack. Last session completed:
- Built CC Monitor page with auto-refresh
- Fixed API key validation

Next steps:
1. Build cc-reporter.js script
2. Add WebSocket updates
--- END HANDOFF ---
```
