---
name: grimoire
description: "Use when the user asks for 'update context', 'update claude', 'save library', or after significant project changes."
---

> **Conversion status: Needs manual review.** This skill was converted from a Claude/MemStack skill, but it contains runtime-specific assumptions that may not apply directly in Manus. Review before relying on it in production.
>
> **Review triggers:** Claude.

> **Original source:** `cwinvestments/memstack/skills/grimoire/SKILL.md`.

#  Grimoire — Updating the Knowledge Library
*Manage and update CLAUDE.md files across all projects.*

## Scope Guard

Use this skill when the task matches the skill description and the user needs this specific workflow or deliverable.

| Use this skill for | Do not use this skill for |
|---|---|
| Use when the user asks for 'update context', 'update claude', 'save library', or after significant project changes. | Tasks outside this skill’s stated domain, or tasks better handled by a more specific installed skill. |

## Protocol

1. **Identify the target project** — use config.json to find the CLAUDE.md path
2. **Read the current CLAUDE.md** if it exists
3. **Determine what to update** based on the session's work:
   - New API endpoints built
   - New database tables/migrations
   - New pages or components added
   - Architecture decisions made
   - Environment variables added
   - Dependencies installed
4. **Update the CLAUDE.md:**
   - Keep existing content intact
   - Add new entries under the right headings
   - Don't duplicate existing entries
   - Use consistent formatting
5. **If no CLAUDE.md exists** — create one with standard sections:
   - Project Overview, Tech Stack, Directory Structure, Key Files
   - API Endpoints, Database Schema, Environment Variables, Dev Commands

## Inputs
- Project name (maps to config.json entry)
- What was built/changed this session

## Outputs
- Updated CLAUDE.md file
- Summary of what was added

## Example Usage

**User:** "update claude.md with the CC Monitor stuff"

```
 Grimoire — Updating the knowledge library.

Reading: C:\Projects\AdminStack\CLAUDE.md

Adding to API Endpoints:
  - POST/GET/PATCH/DELETE /api/cc-sessions
  - POST /api/cc-sessions/report

Adding to Database Schema:
  - cc_sessions (020_cc_sessions.sql)

Adding to Environment Variables:
  - CC_MONITOR_API_KEY

CLAUDE.md updated 
```
