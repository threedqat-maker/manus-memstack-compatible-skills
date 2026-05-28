---
name: keyword-research
description: "Use when the user asks for 'keyword research', 'find keywords', 'keyword strategy', 'search terms', 'keyword opportunities', or needs to identify target keywords with search volume, difficulty, and content mapping. Do not use for full site audits or ad keyword groups."
---

> **Conversion status: Manus-ready draft.** This skill was mechanically converted from the public MemStack MIT-licensed source and should be reviewed during first real use.

> **Original source:** `cwinvestments/memstack/skills/seo-geo/keyword-research/SKILL.md`.

#  Keyword Research — Identifying target keywords and content gaps...
*Analyzes the niche, existing content, and competitor landscape to produce a prioritized keyword map with search intent, difficulty estimates, and page assignments.*

## Protocol

### Step 1: Gather Niche Details

If the user hasn't provided details, ask:

> I need context for keyword research:
> 1. **Niche/industry** — what market are you in?
> 2. **Product or service** — what do you sell or offer?
> 3. **Target audience** — who are you trying to reach? (role, demographics, pain points)
> 4. **Competitors** — 2-3 competitor domains (or I'll identify them)
> 5. **Current site** — URL if live, or list of existing pages
> 6. **Goal** — more traffic, more signups, more sales, thought leadership?

### Step 2: Analyze Existing Content

Scan the project for current keyword coverage:

```bash
# Find all content pages
find app/ pages/ content/ posts/ blog/ -name "*.tsx" -o -name "*.jsx" -o -name "*.md" -o -name "*.mdx" 2>/dev/null | grep -v node_modules

# Extract titles and meta descriptions
grep -rn "title\|description\|<h1\|<H1" --include="*.tsx" --include="*.jsx" --include="*.md" . | grep -v node_modules | head -30

# Find existing keyword targeting
grep -rn "keywords\|tags\|category" --include="*.md" --include="*.mdx" --include="*.json" . | grep -v node_modules | head -20
```

**Document current coverage:**

| Page | Current Title | Primary Keyword (inferred) | Content Type |
|------|--------------|---------------------------|-------------|
| / | [homepage title] | [brand/product name] | Landing |
| /about | [about title] | [brand story] | Informational |
| /blog/[slug] | [post title] | [topic keyword] | Blog |
| /pricing | [pricing title] | [product pricing] | Commercial |

**Identify gaps:** Pages that exist but have weak or no keyword targeting.

### Step 3: Generate Primary Keywords

For each page or content opportunity, suggest a primary keyword:

**Keyword evaluation criteria:**

| Factor | Weight | How to Assess |
|--------|--------|--------------|
| Search intent match | High | Does the keyword match what the page offers? |
| Achievable difficulty | High | Can this site realistically rank? (domain authority, competition) |
| Search volume | Medium | Enough monthly searches to justify the effort |
| Business relevance | High | Does ranking for this drive revenue or signups? |
| Current ranking | Medium | Already ranking 10-50? Optimize. Not ranking at all? Harder path. |

**Primary keyword research process:**

1. **Seed keywords:** Start with 5-10 obvious terms the audience would search
2. **Expand:** For each seed, identify variations using these patterns:
   - How to [topic]
   - Best [product category]
   - [Topic] for [audience]
   - [Topic] vs [alternative]
   - [Topic] examples / templates / tools
   - What is [topic]
3. **Filter:** Remove keywords where the SERP is dominated by major brands (unless you are one)
4. **Prioritize:** Rank by intent match + achievable difficulty

### Step 4: Suggest Long-Tail Variations

For each primary keyword, generate 3-5 long-tail variations:

```markdown
### Primary: "project management software"
Long-tail variations:
- "project management software for small teams" (audience modifier)
- "free project management software 2026" (qualifier + year)
- "project management software with time tracking" (feature modifier)
- "project management software for remote teams" (use case modifier)
- "best project management software for startups" (intent + audience)
```

**Long-tail pattern formulas:**

| Pattern | Example | Intent |
|---------|---------|--------|
| [keyword] for [audience] | "CRM for freelancers" | Commercial |
| best [keyword] [year] | "best CRM 2026" | Commercial |
| [keyword] vs [alternative] | "Notion vs Airtable" | Commercial (comparison) |
| how to [action] with [keyword] | "how to manage leads with a CRM" | Informational |
| [keyword] [feature] | "CRM with email automation" | Commercial |
| free [keyword] | "free CRM software" | Transactional |
| [keyword] examples | "CRM workflow examples" | Informational |
| [keyword] template | "CRM spreadsheet template" | Transactional |

### Step 5: Identify Content Gaps

Compare your content against what competitors rank for:

```markdown
## Content Gap Analysis

### Topics competitors cover that you don't:
| Topic | Competitor Ranking | Your Coverage | Opportunity |
|-------|-------------------|--------------|-------------|
| [topic 1] | competitor.com ranks #3 | No page | Create blog post |
| [topic 2] | competitor.com ranks #7 | Mentioned briefly | Expand into dedicated page |
| [topic 3] | competitor.com ranks #1 | Not covered | Create comparison page |

### Topics you cover but could improve:
| Your Page | Current Ranking (est.) | Issue | Fix |
|-----------|----------------------|-------|-----|
| /blog/[slug] | ~page 3 | Thin content (500 words) | Expand to 1,500+ with more depth |
| /features | Not ranking | Missing target keyword in title | Optimize title and H1 |
```

**Gap identification methods:**
1. Review competitor sitemaps for content they produce
2. Search for primary keywords and note who ranks in top 10
3. Look for "People Also Ask" questions that no one answers well
4. Check competitor blog categories for topics you haven't covered

### Step 6: Map Keywords to Pages

Assign each keyword to a specific page (existing or to be created):

```markdown
## Keyword Map

| Primary Keyword | Volume Est. | Difficulty | Intent | Assigned Page | Status |
|----------------|-------------|-----------|--------|--------------|--------|
| project management tool | High | Hard | Commercial | / (homepage) | Optimize |
| best project management for startups | Medium | Medium | Commercial | /blog/best-pm-startups | Create |
| how to manage remote team projects | Medium | Easy | Informational | /blog/remote-pm-guide | Create |
| project management template free | Low-Med | Easy | Transactional | /templates | Create |
| [brand] vs [competitor] | Low | Easy | Commercial | /blog/brand-vs-competitor | Create |
| what is agile project management | Medium | Medium | Informational | /blog/agile-guide | Create |
```

**Mapping rules:**
- One primary keyword per page — no keyword cannibalization
- Group related keywords on the same page (primary + 3-5 secondary)
- Homepage targets the broadest commercial keyword
- Blog posts target informational and long-tail keywords
- Landing/product pages target transactional keywords
- Comparison pages target "[brand] vs [competitor]" keywords

### Step 7: Prioritize by Search Intent

Classify and prioritize every keyword:

| Intent | Description | Content Type | Priority for |
|--------|------------|-------------|-------------|
| **Transactional** | Ready to buy/sign up | Product page, pricing, CTA landing page | Revenue-focused sites |
| **Commercial** | Researching options | Comparison, review, "best of" | SaaS, e-commerce |
| **Informational** | Learning about a topic | Blog post, guide, tutorial | Traffic + authority building |
| **Navigational** | Looking for a specific site | Homepage, brand pages | Already established brands |

**Priority matrix:**

| | Low Difficulty | High Difficulty |
|---|---------------|----------------|
| **High Intent** |  Do first — quick wins with revenue impact | ⏳ Worth the investment — build authority content |
| **Low Intent** |  Easy traffic — good for brand awareness |  Skip — high effort, low return |

### Step 8: Output Keyword Map

```
 Keyword Research — Complete

Niche: [industry/market]
Site: [domain]
Pages analyzed: [count] existing
Keywords identified: [count] primary + [count] long-tail

Keyword map summary:
  Transactional: [count] keywords → [count] pages
  Commercial:    [count] keywords → [count] pages
  Informational: [count] keywords → [count] pages

Content to create: [count] new pages recommended
Content to optimize: [count] existing pages with improvements

Top 5 priority keywords:
  1. [keyword] — [volume] / [difficulty] — [action: create/optimize] [page]
  2. [keyword] — [volume] / [difficulty] — [action] [page]
  3. [keyword] — [volume] / [difficulty] — [action] [page]
  4. [keyword] — [volume] / [difficulty] — [action] [page]
  5. [keyword] — [volume] / [difficulty] — [action] [page]

Quick wins (low difficulty, high intent):
  - [keyword] → [page to create/optimize]
  - [keyword] → [page to create/optimize]

Next steps:
1. Create content for top 5 priority keywords (use blog-post skill)
2. Optimize existing pages with assigned keywords (use meta-tag-optimizer)
3. Build internal links between related keyword pages
4. Track rankings monthly in Google Search Console
5. Revisit keyword map quarterly
```
