---
name: forge
description: "Use when the user asks for 'forge this', 'new skill', 'create enchantment', or wants to create a MemStack skill."
---

> **Conversion status: Needs manual review.** This skill was converted from a Claude/MemStack skill, but it contains runtime-specific assumptions that may not apply directly in Manus. Review before relying on it in production.
>
> **Review triggers:** $MEMSTACK_PATH.

> **Original source:** `cwinvestments/memstack/skills/forge/SKILL.md`.

#  Forge — Creating New Enchantment...
*Create new MemStack skills or improve existing ones.*

## Scope Guard

Use this skill when the task matches the skill description and the user needs this specific workflow or deliverable.

| Use this skill for | Do not use this skill for |
|---|---|
| Use when the user asks for 'forge this', 'new skill', 'create enchantment', or wants to create a MemStack skill. | Tasks outside this skill’s stated domain, or tasks better handled by a more specific installed skill. |

## Protocol

### Creating a new skill:

1. **Ask the user:**
   - What should the skill do?
   - What trigger keywords should activate it?
   - What inputs does it need? What should it output?

2. **Generate the skill file** with v2.1 format:
   - YAML frontmatter with name and description ("MUST use when..." format)
   - Activation message (pick an appropriate emoji)
   - Context guard (if the skill could have false positives)
   - Protocol steps
   - Inputs/Outputs
   - Example usage
   - Level history starting at Lv.1

3. **Write the file** to `$MEMSTACK_PATH/skills/{name}.md`

4. **Update MEMSTACK.md** — add a new row to the Skill Index table

5. **Confirm creation** — show the skill summary

### Improving an existing skill:

1. **Read the current skill file**
2. **Apply improvements** based on user feedback
3. **Increment the level** in Level History
4. **Update the file**

## Inputs
- Skill concept description, trigger keywords, desired behavior

## Outputs
- New skill .md file in skills/
- Updated MEMSTACK.md index

## Example Usage

**User:** "forge a new skill called Beacon for health check pinging"

```
 Forge — Creating new enchantment...

Creating: Beacon
Emoji:  | Type: Passive | Triggers: "health check", "ping", "uptime"

Writing: skills/beacon.md 
Updating: MEMSTACK.md — added row #N 

Beacon is ready. Triggers: "health check", "ping", "uptime"
```
