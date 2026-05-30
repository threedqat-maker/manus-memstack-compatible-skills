# Manus MemStack Compatible Skills

This repository contains **71 Manus-compatible skills** converted from the public/free [`cwinvestments/memstack`](https://github.com/cwinvestments/memstack) skill set. The original project is MIT-licensed; the original license is preserved in [`LICENSE`](LICENSE).

The repository intentionally excludes Claude runtime infrastructure such as `.claude/`, hooks, local MCP loader configuration, dashboard files, non-public Pro skills, and MemStack-dependent skills that require unavailable runtime services.

## Structure

Each skill is stored as a self-contained Manus skill package:

```text
skills/<skill-name>/
├── SKILL.md
└── references/, scripts/, or templates/ when present in the original public source
```

When installing or copying a skill, keep the whole skill folder together. Some skills depend on bundled `references/` or `scripts/` resources, and copying only `SKILL.md` can remove important supporting material.

## Counts

| Metric | Count |
|---|---:|
| Manus-compatible skills retained | 71 |
| Manus-ready drafts | 71 |
| Skills marked needs review | 0 |
| MemStack/Claude runtime-dependent skills removed | 13 |
| Bundled resource groups/files copied | 4 |
| Skills with Manus-native `Scope Guard` | 71 |
| Skills with restored useful `Anti-patterns` | 20 |
| Claude-style `Activation` sections retained | 0 |

## Installation guidance

Install individual skills from `skills/<skill-name>/SKILL.md`. If a skill has a `references/`, `scripts/`, or `templates/` directory, keep those folders with the skill so Manus can access the supporting resources during use.

All skills retained in the main `skills/` directory are Manus-ready drafts. They should still be tested during real use, but the known MemStack/Claude runtime-dependent skills have been removed from the installable catalog.

## Manus-Compatible Skill Catalog

### Automation

This group contains **5** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`api-integration`](skills/api-integration/SKILL.md) | Manus-ready draft | Use for 'API integration', 'connect APIs', 'sync data', 'data mapping', 'rate limiting', or needs system-to-system connectors with authentication, rate limit handling, and error recovery. Generates API integration code with authentication (OAuth, API key, JWT), request/response mapping, rate limit handling, error recovery with circuit breakers, and sync monitoring. **Do not use for** visual n8n workflows or webhook receiving. |
| [`content-pipeline`](skills/content-pipeline/SKILL.md) | Manus-ready draft | Use for 'content pipeline', 'content automation', 'auto-publish', 'repurpose content', 'multi-platform publishing', or needs end-to-end content workflow from ideation through cross-platform formatting and publishing. **Do not use for** single social media posts or individual blog posts. |
| [`cron-scheduler`](skills/cron-scheduler/SKILL.md) | Manus-ready draft | Use for 'cron job', 'scheduled task', 'run every', 'cron expression', 'recurring job', or needs production-grade scheduled jobs with overlap prevention, monitoring, and structured logging. **Do not use for** n8n workflows or event-driven webhooks. |
| [`n8n-workflow-builder`](skills/n8n-workflow-builder/SKILL.md) | Manus-ready draft | Use for 'n8n workflow', 'build a workflow', 'automation workflow', 'connect services', or needs visual workflow design with node mapping, data transformations, and error handling for n8n. **Do not use for** standalone webhook endpoints or cron jobs. |
| [`webhook-designer`](skills/webhook-designer/SKILL.md) | Manus-ready draft | Use for 'webhook', 'webhook handler', 'webhook endpoint', 'receive events', 'HMAC verification', 'idempotency', or needs secure webhook handlers with signature verification, retry handling, and dead letter queues. **Do not use for** full n8n workflows or scheduled tasks. |

### Business

This group contains **10** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`client-onboarding`](skills/client-onboarding/SKILL.md) | Manus-ready draft | Use for 'client onboarding', 'new client', 'onboard client', 'kickoff meeting', 'intake form', 'welcome email', or needs welcome sequences, questionnaires, and setup checklists for new clients. **Do not use for** contracts or invoicing. |
| [`contract-template`](skills/contract-template/SKILL.md) | Manus-ready draft | Use for 'contract', 'agreement', 'service agreement', 'NDA', 'freelance contract', 'consulting agreement', or needs service agreements with IP ownership, payment terms, and termination clauses. **Do not use for** invoicing or client onboarding. |
| [`financial-model`](skills/financial-model/SKILL.md) | Manus-ready draft | Use for 'financial model', 'projections', 'revenue forecast', 'unit economics', 'break-even', 'cash flow', or mentions MRR, churn, CAC, LTV, or runway. Builds monthly projections with scenario modeling. **Do not use for** pricing strategy or invoice generation. |
| [`freelancer-toolkit`](skills/freelancer-toolkit/SKILL.md) | Manus-ready draft | Use for 'track my time', 'freelancer invoice', 'billable hours', 'time tracking', 'freelance finances', 'client billing', 'project hours', or needs invoicing, time tracking, or analytics patterns for freelance work. **Do not use for** general invoice templates or proposal writing. |
| [`gdpr`](skills/gdpr/SKILL.md) | Manus-ready draft | Use for 'GDPR', 'data protection', 'privacy compliance', 'DPA', 'DSAR', 'data subject request', 'cookie consent', 'privacy audit', 'CCPA', or asks 'do I need GDPR for this repo'. Scans the repository to detect what personal data is collected, classifies sensitivity, determines whether GDPR applies and how critical it is, then reports required roles, obligations, and remediation. **Do not use for** general security audits (use owasp-top10) or contract drafting (use contract-template). |
| [`invoice-generator`](skills/invoice-generator/SKILL.md) | Manus-ready draft | Use for 'invoice', 'generate invoice', 'create invoice', 'bill client', 'line items', 'payment terms', or needs professional invoices with tax calculations and payment instructions. **Do not use for** contracts or financial projections. |
| [`licensing`](skills/licensing/SKILL.md) | Manus-ready draft | Use for 'licensing', 'license audit', 'can I use this commercially', 'OSS license check', 'license compatibility', 'GPL', 'MIT', 'AGPL', 'copyleft'. Scans the repository for every dependency and asset license, then produces a per-package verdict table: ready for commercial use, citation/attribution required, more information needed, or commercial use not allowed. **Do not use for** vulnerability scanning (use dependency-audit) or contract drafting (use contract-template). |
| [`proposal-writer`](skills/proposal-writer/SKILL.md) | Manus-ready draft | Use for 'write proposal', 'create proposal', 'proposal for', 'client proposal', 'project proposal', 'bid on project', 'pitch', or is preparing a project proposal for a client or freelance engagement. **Do not use for** contracts, invoices, or onboarding. |
| [`scope-of-work`](skills/scope-of-work/SKILL.md) | Manus-ready draft | Use for 'scope of work', 'SOW', 'define scope', 'project scope', 'write SOW', 'scope document', or is defining project boundaries, deliverables, and acceptance criteria for a formal engagement. **Do not use for** proposals, contracts, or invoicing. |
| [`sop-builder`](skills/sop-builder/SKILL.md) | Manus-ready draft | Use for 'create SOP', 'write SOP', 'standard operating procedure', 'document process', 'process documentation', 'runbook', 'playbook', or is creating step-by-step documentation for a repeatable process. **Do not use for** project proposals or scope documents. |

### Content

This group contains **8** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`blog-post`](skills/blog-post/SKILL.md) | Manus-ready draft | Use for 'write blog post', 'blog post about', 'write article', 'create blog', 'content for blog', 'write post', or is creating long-form written content for a blog or publication. **Do not use for** landing page copy, email sequences, or social media posts. |
| [`email-sequence`](skills/email-sequence/SKILL.md) | Manus-ready draft | Use for 'write email sequence', 'email sequence', 'drip campaign', 'email series', 'nurture sequence', 'onboarding emails', 'launch emails', or is creating a multi-email automated campaign. **Do not use for** newsletters or single marketing emails. |
| [`landing-page-copy`](skills/landing-page-copy/SKILL.md) | Manus-ready draft | Use for 'write landing page', 'landing page copy', 'sales page', 'hero section', 'conversion copy', or is creating persuasive short-form copy for a product or service landing page. **Do not use for** blog posts or email sequences. |
| [`newsletter`](skills/newsletter/SKILL.md) | Manus-ready draft | Use for 'newsletter', 'email newsletter', 'weekly digest', 'subscriber growth', 'open rates', or needs subject lines, content structure, sponsorship placement, and growth tactics for email newsletters. **Do not use for** lead magnets or content pipelines. |
| [`product-description`](skills/product-description/SKILL.md) | Manus-ready draft | Use for 'product description', 'product listing', 'product copy', 'Amazon listing', 'Shopify listing', 'e-commerce copy', or needs conversion-optimized product descriptions with benefit-driven headlines and platform-specific SEO. **Do not use for** pricing strategy or sales funnels. |
| [`tiktok-script`](skills/tiktok-script/SKILL.md) | Manus-ready draft | Use for 'TikTok script', 'TikTok video', 'Reels script', 'Shorts script', 'short-form video', or needs timestamped scripts with hooks, visual cues, and captions for 15-60 second videos. **Do not use for** Twitter threads or webinar scripts. |
| [`twitter-thread`](skills/twitter-thread/SKILL.md) | Manus-ready draft | Use for 'twitter thread', 'tweet thread', 'X thread', 'viral thread', or wants to create a multi-tweet narrative with hook tweets, data points, and CTAs. **Do not use for** TikTok scripts, newsletters, or LinkedIn posts. |
| [`youtube-script`](skills/youtube-script/SKILL.md) | Manus-ready draft | Use for 'YouTube script', 'video script', 'write script for YouTube', 'YouTube video outline', or is creating scripted content for a YouTube video with hooks, chapters, and CTAs. **Do not use for** TikTok/Reels short-form scripts or webinar presentations. |

### Core

This group contains **6** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`governor`](skills/governor/SKILL.md) | Manus-ready draft | Use for 'new project', 'project init', 'what tier', 'scope', or discusses project maturity, complexity budget, or what's appropriate to build. |
| [`humanize`](skills/humanize/SKILL.md) | Manus-ready draft | Use for 'humanize', 'clean up writing', 'make it sound natural', or wants text to not sound AI-generated. |
| [`scan`](skills/scan/SKILL.md) | Manus-ready draft | Use for 'scan project', 'estimate', 'how much to charge', or needs codebase complexity analysis. |
| [`shard`](skills/shard/SKILL.md) | Manus-ready draft | Use for 'shard this', 'split file', or when working with files over 1000 lines. |
| [`sight`](skills/sight/SKILL.md) | Manus-ready draft | Use for 'draw', 'diagram', 'visualize', 'architecture', or needs a visual overview of code structure. |
| [`verify`](skills/verify/SKILL.md) | Manus-ready draft | Use for 'verify', 'check this work', 'does it pass', or before committing completed work. |

### Deployment

This group contains **6** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`ci-cd-pipeline`](skills/ci-cd-pipeline/SKILL.md) | Manus-ready draft | Use for 'CI/CD', 'GitHub Actions', 'pipeline', 'continuous integration', 'continuous deployment', 'ci-cd-pipeline', 'automate deploys', or needs to set up automated build, test, and deployment pipelines. **Do not use for** one-time manual deployments. |
| [`docker-setup`](skills/docker-setup/SKILL.md) | Manus-ready draft | Use for 'Docker', 'Dockerfile', 'docker-compose', 'containerize', 'docker-setup', or needs to containerize an application with optimized Docker images and compose configurations. **Do not use for** serverless or static site deployments. |
| [`domain-ssl`](skills/domain-ssl/SKILL.md) | Manus-ready draft | Use for 'setup domain', 'configure DNS', 'SSL certificate', 'domain-ssl', 'custom domain', 'HTTPS setup', or needs to configure DNS records, SSL certificates, and custom domains for any hosting provider. **Do not use for** full deployment workflows. |
| [`hetzner-setup`](skills/hetzner-setup/SKILL.md) | Manus-ready draft | Use for 'Hetzner', 'VPS setup', 'server provisioning', 'deploy to VPS', 'hetzner-setup', 'cloud server', or needs to provision, harden, and deploy applications to a Hetzner Cloud server with Docker, Nginx/Caddy, SSL, and monitoring. **Do not use for** managed platform deployments like Railway or Netlify. |
| [`netlify-deploy`](skills/netlify-deploy/SKILL.md) | Manus-ready draft | Use for 'deploy to Netlify', 'Netlify setup', 'netlify-deploy', or needs to deploy a static site or serverless functions to Netlify with build configuration and custom domains. **Do not use for** Railway, Vercel, or VPS deployments. |
| [`railway-deploy`](skills/railway-deploy/SKILL.md) | Manus-ready draft | Use for 'deploy to Railway', 'Railway setup', 'railway-deploy', or needs to deploy a Node.js, Python, or Docker application to Railway with environment variables, custom domains, and monitoring. **Do not use for** Netlify, Vercel, or Hetzner deployments. |

### Development

This group contains **9** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`api-designer`](skills/api-designer/SKILL.md) | Manus-ready draft | Use for 'design API', 'API endpoints', 'REST API', 'API designer', 'route structure', 'API architecture', or is designing RESTful API routes, request/response schemas, and endpoint organization. **Do not use for** API security audits or database design. |
| [`changelog-generator`](skills/changelog-generator/SKILL.md) | Manus-ready draft | Use for 'generate changelog', 'update changelog', 'what changed', 'release notes', 'write changelog', or needs a formatted CHANGELOG.md from git commit history. **Do not use for** diary entries, git log viewing, or commit message writing. |
| [`code-reviewer`](skills/code-reviewer/SKILL.md) | Manus-ready draft | Use for 'review code', 'code review', 'check my code', 'audit this', 'review PR', 'review changes', 'what\\'s wrong with this', or is requesting a structured review of code quality, security, performance, or maintainability. **Do not use for** refactoring plans or test generation. |
| [`mentor`](skills/mentor/SKILL.md) | Manus-ready draft | Use for 'teach me', 'explain as you go', 'mentor mode', 'walk me through', 'help me learn', 'explain why', 'learning mode', or wants real-time plain language narration of decisions and tradeoffs while building. **Do not use for** code review or debugging. |
| [`migration-planner`](skills/migration-planner/SKILL.md) | Manus-ready draft | Use for 'migration', 'schema change', 'database migration', 'alter table', 'add column', 'change type', 'rollback plan', or needs safe database schema evolution with zero-downtime strategies. **Do not use for** initial database design (use database-architect) or code refactoring. |
| [`performance-audit`](skills/performance-audit/SKILL.md) | Manus-ready draft | Use for 'performance audit', 'why is it slow', 'optimize performance', 'page speed', 'Core Web Vitals', 'lighthouse', 'load time', or needs to diagnose and fix frontend or backend performance issues. **Do not use for** code reviews or security audits. |
| [`refactor-planner`](skills/refactor-planner/SKILL.md) | Manus-ready draft | Use for 'refactor', 'refactoring plan', 'code cleanup', 'reduce duplication', 'simplify code', 'tech debt', 'god class', 'tight coupling', or needs to systematically improve existing code. Identifies targets, assesses risk, and builds incremental execution plans. **Do not use for** writing new features or database migrations. |
| [`test-writer`](skills/test-writer/SKILL.md) | Manus-ready draft | Use for 'write tests', 'add tests', 'test coverage', 'unit tests', 'integration tests', 'component tests', 'mocking', 'edge cases', or needs to generate tests with proper mocking and edge case coverage. **Do not use for** refactoring plans or database migrations. |
| [`webapp-testing`](skills/webapp-testing/SKILL.md) | Manus-ready draft | Use for 'write browser tests', 'test this page', 'playwright test', 'e2e test', 'end to end test', 'browser test', 'test the UI', or needs Playwright-based browser testing for a web application. **Do not use for** unit tests, API tests, or non-browser testing. |

### Marketing

This group contains **8** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`competitor-analysis`](skills/competitor-analysis/SKILL.md) | Manus-ready draft | Use for 'competitor analysis', 'competitive analysis', 'compare products', 'market positioning', 'competitive gaps', or needs pricing, feature, and messaging comparisons against competitors. **Do not use for** setting your own pricing strategy. |
| [`facebook-ad`](skills/facebook-ad/SKILL.md) | Manus-ready draft | Use for 'facebook ad', 'FB ad', 'Meta ad', 'Instagram ad', or needs social media ad copy with targeting, creative direction, and A/B test plans for Meta Ads Manager. **Do not use for** Google search ads or organic social content. |
| [`google-ad`](skills/google-ad/SKILL.md) | Manus-ready draft | Use for 'google ad', 'search ad', 'PPC', 'Google Ads', 'responsive search ad', 'ad extensions', or needs keyword groups, headlines, descriptions, and Quality Score optimization for Google Ads. **Do not use for** Facebook/Meta ads or SEO. |
| [`launch-plan`](skills/launch-plan/SKILL.md) | Manus-ready draft | Use for 'launch plan', 'product launch', 'go-to-market', 'launch calendar', or needs a day-by-day launch timeline with pre-launch, launch week, and post-launch task checklists. **Do not use for** ongoing funnel design or ad copy alone. |
| [`lead-magnet`](skills/lead-magnet/SKILL.md) | Manus-ready draft | Use for 'lead magnet', 'opt-in', 'freebie', 'list building', 'email list growth', or needs a lead capture asset with landing page copy, delivery emails, and nurture sequence. **Do not use for** full funnel design or paid ad copy. |
| [`pricing-strategy`](skills/pricing-strategy/SKILL.md) | Manus-ready draft | Use for 'pricing strategy', 'how to price', 'pricing model', 'tier structure', 'pricing psychology', or needs to design pricing tiers, apply pricing psychology, and plan A/B price tests. **Do not use for** competitor pricing comparison alone. |
| [`sales-funnel`](skills/sales-funnel/SKILL.md) | Manus-ready draft | Use for 'sales funnel', 'funnel', 'conversion funnel', 'customer journey', or wants to map the complete customer journey from stranger to repeat buyer with copy hooks and conversion targets. **Do not use for** ad copy creation or time-bound launch plans. |
| [`webinar-script`](skills/webinar-script/SKILL.md) | Manus-ready draft | Use for 'webinar script', 'webinar', 'live presentation', 'teach-to-sell', or needs a timestamped presentation script with slide notes, presenter cues, and replay email sequence. **Do not use for** launch plans or static sales page copy. |

### Product

This group contains **6** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`feature-spec`](skills/feature-spec/SKILL.md) | Manus-ready draft | Use for 'feature spec', 'spec this feature', 'write a spec', 'functional requirements', or needs a detailed specification for one feature with user flows, edge cases, API definitions, and acceptance criteria. **Do not use for** full PRDs or user story generation. |
| [`feedback-analyzer`](skills/feedback-analyzer/SKILL.md) | Manus-ready draft | Use for 'analyze feedback', 'feedback analysis', 'what are customers asking for', or has support tickets, reviews, or survey data to categorize, score, and prioritize into actionable reports. **Do not use for** competitor analysis or market research. |
| [`mvp-scoper`](skills/mvp-scoper/SKILL.md) | Manus-ready draft | Use for 'MVP', 'minimum viable product', 'scope the MVP', 'what should I build first', 'strip to core', or needs to define the smallest build that validates a product hypothesis. **Do not use for** full PRDs or roadmap planning. |
| [`prd-writer`](skills/prd-writer/SKILL.md) | Manus-ready draft | Use for 'PRD', 'product requirements', 'requirements document', or needs a complete engineering-ready PRD with problem statement, personas, MoSCoW features, and success metrics. **Do not use for** single feature specs or user story backlogs. |
| [`roadmap-builder`](skills/roadmap-builder/SKILL.md) | Manus-ready draft | Use for 'roadmap', 'product roadmap', 'quarterly plan', 'now/next/later', 'OKRs', or needs strategic planning with themes, milestones, resource allocation, and stakeholder-ready views. **Do not use for** MVP scoping or sprint-level planning. |
| [`user-story-generator`](skills/user-story-generator/SKILL.md) | Manus-ready draft | Use for 'user stories', 'write stories', 'backlog', 'sprint planning', 'acceptance criteria', or needs prioritized stories with Given/When/Then criteria and story point estimates. **Do not use for** full PRDs or detailed feature specs. |

### Security

This group contains **7** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`api-audit`](skills/api-audit/SKILL.md) | Manus-ready draft | Use for 'audit API', 'check API security', 'API routes security', 'endpoint audit', 'check my routes', or needs to verify API route protection. Reviews API endpoints for authentication, authorization, and input validation gaps. **Do not use for** frontend security headers or dependency scanning. |
| [`csp-headers`](skills/csp-headers/SKILL.md) | Manus-ready draft | Use for 'CSP', 'Content-Security-Policy', 'security headers', 'HSTS', 'X-Frame-Options', 'clickjacking', 'unsafe-inline', 'unsafe-eval', or needs to audit, generate, or fix HTTP security headers for a web application. **Do not use for** API route audits or dependency scanning. |
| [`dependency-audit`](skills/dependency-audit/SKILL.md) | Manus-ready draft | Use for 'dependency audit', 'npm audit', 'pip audit', 'cargo audit', 'security vulnerabilities', 'outdated packages', 'supply chain', or needs to scan project dependencies for vulnerabilities, abandoned packages, and upgrade risks. **Do not use for** application-level security or secrets scanning. |
| [`owasp-top10`](skills/owasp-top10/SKILL.md) | Manus-ready draft | Use for 'OWASP audit', 'OWASP top 10', 'security audit', 'vulnerability assessment', 'full security check', or needs a comprehensive web application security review against OWASP Top 10 categories. **Do not use for** dependency audits or secret scanning alone. |
| [`rls-checker`](skills/rls-checker/SKILL.md) | Manus-ready draft | Use for 'check RLS', 'audit RLS', 'RLS policies', 'row level security', 'Supabase security audit', or needs to verify table-level access control. Audits Supabase Row Level Security policies across all tables. **Do not use for** non-Supabase projects or writing RLS policies from scratch. |
| [`rls-guardian`](skills/rls-guardian/SKILL.md) | Manus-ready draft | Use when creating or altering database tables in Supabase or PostgreSQL projects. Triggers include: CREATE TABLE, ALTER TABLE, migration files, 'RLS', 'row level security', 'new table', 'database schema'. Enforces Row Level Security policies on every table to prevent unauthorized data access. **Do not use for** general SQL queries or non-schema database tasks. |
| [`secrets-scanner`](skills/secrets-scanner/SKILL.md) | Manus-ready draft | Use for 'scan for secrets', 'check for leaked keys', 'secrets scanner', 'hardcoded credentials', 'API key leak', or needs to detect exposed secrets in source code. **Do not use for** dependency vulnerabilities or RLS auditing. |

### SEO/GEO

This group contains **6** Manus-compatible skill(s).

| Skill | Status | Purpose |
|---|---|---|
| [`ai-search-visibility`](skills/ai-search-visibility/SKILL.md) | Manus-ready draft | Use for 'AI search', 'AI visibility', 'ChatGPT ranking', 'Perplexity optimization', 'GEO', 'generative engine optimization', or needs to optimize content for AI-powered search engines and LLM citations. **Do not use for** traditional SEO audits or Google Ads. |
| [`keyword-research`](skills/keyword-research/SKILL.md) | Manus-ready draft | Use for 'keyword research', 'find keywords', 'keyword strategy', 'search terms', 'keyword opportunities', or needs to identify target keywords with search volume, difficulty, and content mapping. **Do not use for** full site audits or ad keyword groups. |
| [`local-seo`](skills/local-seo/SKILL.md) | Manus-ready draft | Use for 'local SEO', 'Google Business Profile', 'local search', 'NAP consistency', 'local listings', 'Google Maps', 'local pack', or is optimizing a business for local search results and map visibility. **Do not use for** general SEO audits or national keyword research. |
| [`meta-tag-optimizer`](skills/meta-tag-optimizer/SKILL.md) | Manus-ready draft | Use for 'meta tags', 'title tag', 'meta description', 'optimize meta', 'SERP preview', or needs to write or optimize HTML meta tags for better search visibility and click-through rates. **Do not use for** schema markup or full site audits. |
| [`schema-markup`](skills/schema-markup/SKILL.md) | Manus-ready draft | Use for 'add schema', 'schema markup', 'JSON-LD', 'structured data', 'rich results', 'rich snippets', or is adding or fixing schema.org structured data for better search result appearance. **Do not use for** meta tag optimization or full SEO audits. |
| [`site-audit`](skills/site-audit/SKILL.md) | Manus-ready draft | Use for 'SEO audit', 'site audit', 'check SEO', 'audit my site', 'SEO check', 'technical SEO', or is evaluating a website's search engine optimization health, meta tags, performance, or structured data. **Do not use for** keyword research or schema markup generation alone. |

## Removed MemStack/Claude Runtime-Dependent Skills

The following skills were removed from the main installable catalog because they require MemStack or Claude Code runtime infrastructure that is not available in Manus. Git history preserves the previous converted versions if they are ever needed for a rewrite.

| Removed skill | Former category | Reason |
|---|---|---|
| `compress` | core | Depends on Claude Code session/context compression and MemStack/TokenStack behavior. |
| `diary` | core | Depends on $MEMSTACK_PATH, memstack-db.py, SQLite session storage, .claude, hooks, and Claude Code session logging. |
| `echo` | core | Depends on MemStack SQLite/semantic session memory, diary search, $MEMSTACK_PATH, and .claude. |
| `familiar` | core | Depends on MemStack paths and Claude Code multi-session workflow assumptions. |
| `forge` | core | Creates MemStack skills via $MEMSTACK_PATH; not a Manus-native skill creation workflow. |
| `grimoire` | core | Manages CLAUDE.md files and includes Claude/Windows path assumptions. |
| `hosted-mcp-catalog` | automation | Current version is Claude/MCP-client specific and should be rewritten before Manus use. |
| `marketplace-submit` | marketing | Depends on Claude Code session/MCP marketplace workflow assumptions and MemStack branding. |
| `project` | core | Depends on $MEMSTACK_PATH, memstack-db.py, SQLite project snapshot storage, and Claude Code handoff conventions. |
| `quill` | core | Depends on MemStack SQLite context and memstack-db.py; overlaps with Manus-compatible proposal/SOW/invoice skills. |
| `state` | core | Depends on .claude/STATE.md and MemStack-style state management. |
| `token-optimization` | core | Depends on Claude Code, MCP setup, hooks, RTK/Serena/TokenStack, and Claude command injection assumptions. |
| `work` | core | Depends on $MEMSTACK_PATH, .claude, CLAUDE.md, STATE.md, memstack-db.py, SQLite, diary, and MemStack planning state. |

## Conversion Policy

Claude-style `Activation` sections were intentionally omitted because Manus uses skill metadata for routing and does not need fixed activation banners. Each retained skill includes a Manus-native `Scope Guard` section. Useful `Anti-patterns` sections from the original skills were restored where present because they provide task-specific reasoning guardrails.

## Practical note

This is a first-pass, credit-efficient conversion. The domain-specific workflows are largely preserved, but each skill should still be tested and refined during real Manus usage. Review [`CONVERSION_REPORT.md`](CONVERSION_REPORT.md) for cleanup details and manual updates already completed.
