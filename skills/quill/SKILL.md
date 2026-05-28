---
name: quill
description: "Use when the user asks for 'create quotation', 'generate quote', 'proposal', or needs a client-facing price document."
---

> **Conversion status: Needs manual review.** This skill was converted from a Claude/MemStack skill, but it contains runtime-specific assumptions that may not apply directly in Manus. Review before relying on it in production.
>
> **Review triggers:** $MEMSTACK_PATH, memstack-db.py.

> **Original source:** `cwinvestments/memstack/skills/quill/SKILL.md`.

# ️ Quill — Drafting Quotation...
*Generate professional client quotations and proposals.*

## Protocol

1. **Gather requirements** — ask the user for:
   - Client name and company
   - Project description (or "use Scan results" if just scanned)
   - Timeline expectations
   - Any specific requirements or constraints

2. **If project was already scanned** — use the Scan results for scope and pricing
3. **If not scanned** — run a quick Scan first to get baseline metrics

4. **Generate the quotation** using `templates/client-quote.md`:
   - Professional header with CW Affiliate Investments info
   - Project scope breakdown with line items
   - Three pricing tiers (if applicable)
   - Timeline with milestones
   - Terms and conditions
   - Valid-until date (30 days from now)

5. **Save to SQLite** (primary):
   ```bash
   python "$MEMSTACK_PATH/db/memstack-db.py" set-context '{"project":"<client>","last_quote_date":"<date>","quote_summary":"<scope>"}'
   ```
6. **Also save markdown copy** to `memory/projects/{client}-quote-{date}.md` (human-readable backup)
7. **Present formatted** for copy-paste into email or PDF export

## Inputs
- Client name and company
- Project requirements or Scan results

## Outputs
- Professional quotation document
- Quote context saved to SQLite database
- Markdown backup in memory/projects/

## Example Usage

**User:** "generate a quote for John at GreenTech for an admin dashboard"

```
️ Quill — Drafting quotation...

CW AFFILIATE INVESTMENTS
Quotation — GreenTech Corp
Date: 2026-02-18 | Valid Until: 2026-03-20

Scope: Full-stack admin dashboard with auth, CRUD, payments
Standard: $22,000 | Timeline: 7 weeks

Saved: memory/projects/greentech-quote-2026-02-18.md
```
