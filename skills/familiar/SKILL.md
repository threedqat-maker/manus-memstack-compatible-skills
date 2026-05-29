---
name: familiar
description: "Use when the user asks for 'dispatch', 'send familiar', 'split task', or needs work split across parallel CC sessions."
---

> **Conversion status: Needs manual review.** This skill was converted from a Claude/MemStack skill, but it contains runtime-specific assumptions that may not apply directly in Manus. Review before relying on it in production.
>
> **Review triggers:** $MEMSTACK_PATH, CC session.

> **Original source:** `cwinvestments/memstack/skills/familiar/SKILL.md`.

#  Familiar — Dispatching Sub-Agents...
*Break large tasks into coordinated Manus session prompts for parallel execution.*

## Scope Guard

Use this skill when the task matches the skill description and the user needs this specific workflow or deliverable.

| Use this skill for | Do not use this skill for |
|---|---|
| Use when the user asks for 'dispatch', 'send familiar', 'split task', or needs work split across parallel CC sessions. | Tasks outside this skill’s stated domain, or tasks better handled by a more specific installed skill. |

## Protocol

1. **Analyze the task** — identify independent sub-tasks that can run in parallel
2. **Determine session count** — split into 2-6 sessions based on complexity
3. **For each sub-task, generate a complete CC prompt** that includes:
   - Working directory path
   - Full task description with acceptance criteria
   - Any shared context (database schema, API contracts, types)
   - MemStack activation line: `Read $MEMSTACK_PATH/MEMSTACK.md`
   - CC Monitor reporting snippet (if configured in config.json)
4. **Add coordination notes** — specify what each session should NOT touch to avoid conflicts
5. **Define merge order** — which session's work should be committed first

## Inputs
- The large task description
- Project directory from config.json
- Number of available Manus sessions (default: 3)

## Outputs
- Numbered list of sub-task prompts, each ready to paste into a new Manus session
- Coordination notes explaining dependencies and merge order

## Example Usage

**User:** "dispatch — build the analytics dashboard, API routes, and database migration"

**Familiar activates:**

```
 Familiar — Dispatching sub-agents...

Session 1 — Database & Types
Working directory: C:\Projects\AdminStack
Task: Create migration + TypeScript types for analytics...

Session 2 — API Routes
Working directory: C:\Projects\AdminStack
Task: Build /api/analytics endpoints (types from Session 1)...

Session 3 — Frontend Page
Working directory: C:\Projects\AdminStack
Task: Build /analytics dashboard page...

Merge order: Session 1 → Session 2 → Session 3
```
