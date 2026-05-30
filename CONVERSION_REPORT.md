# Conversion Report

This report covers the cleaned Manus-compatible skill catalog derived from the public/free MemStack skills.

| Metric | Count |
|---|---:|
| Manus-compatible skills retained | 71 |
| Marked needs review | 0 |
| Removed MemStack/Claude runtime-dependent skills | 13 |
| Resource groups/files copied | 4 |
| Skills with Manus-native Scope Guard | 71 |
| Skills with restored useful Anti-patterns | 20 |
| Claude-style Activation sections retained | 0 |

## Removed incompatible skills

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

## Manual Review Updates Retained

| Skill | Status | Change made |
|---|---|---|
| `ai-search-visibility` | Manus-ready draft | Reviewed on 2026-05-29: verified AI crawler guidance, local platform reference, Manus-native Scope Guard, and AI-search Anti-patterns restored. |
| `governor` | Manus-ready draft | Reviewed on 2026-05-29: replaced Claude-specific context source wording with Manus-neutral project-context wording. |
| `secrets-scanner` | Manus-ready draft | Reviewed on 2026-05-29: removed CLAUDE.md documentation scan and rewrote MemStack Pro hook coverage as optional generic pre-commit/pre-push secret scanning guidance. |
| `site-audit` | Manus-ready draft | Reviewed on 2026-05-29: AI platform reference updated, Manus-neutral platform wording applied, and Scope Guard plus Anti-patterns restored. |

## Full Retained Skill Inventory

| Skill | Category | Source | Resources |
|---|---|---|---|
| `api-integration` | automation | `skills/automation/api-integration/SKILL.md` | None |
| `content-pipeline` | automation | `skills/automation/content-pipeline/SKILL.md` | None |
| `cron-scheduler` | automation | `skills/automation/cron-scheduler/SKILL.md` | None |
| `n8n-workflow-builder` | automation | `skills/automation/n8n-workflow-builder/SKILL.md` | None |
| `webhook-designer` | automation | `skills/automation/webhook-designer/SKILL.md` | None |
| `client-onboarding` | business | `skills/business/client-onboarding/SKILL.md` | None |
| `contract-template` | business | `skills/business/contract-template/SKILL.md` | None |
| `financial-model` | business | `skills/business/financial-model/SKILL.md` | None |
| `freelancer-toolkit` | business | `skills/business/freelancer-toolkit/SKILL.md` | None |
| `gdpr` | business | `skills/business/gdpr/SKILL.md` | None |
| `invoice-generator` | business | `skills/business/invoice-generator/SKILL.md` | None |
| `licensing` | business | `skills/business/licensing/SKILL.md` | None |
| `proposal-writer` | business | `skills/business/proposal-writer/SKILL.md` | None |
| `scope-of-work` | business | `skills/business/scope-of-work/SKILL.md` | None |
| `sop-builder` | business | `skills/business/sop-builder/SKILL.md` | None |
| `blog-post` | content | `skills/content/blog-post/SKILL.md` | None |
| `email-sequence` | content | `skills/content/email-sequence/SKILL.md` | None |
| `landing-page-copy` | content | `skills/content/landing-page-copy/SKILL.md` | None |
| `newsletter` | content | `skills/content/newsletter/SKILL.md` | None |
| `product-description` | content | `skills/content/product-description/SKILL.md` | None |
| `tiktok-script` | content | `skills/content/tiktok-script/SKILL.md` | None |
| `twitter-thread` | content | `skills/content/twitter-thread/SKILL.md` | None |
| `youtube-script` | content | `skills/content/youtube-script/SKILL.md` | None |
| `governor` | core | `skills/governor/SKILL.md` | None |
| `humanize` | core | `skills/humanize/SKILL.md` | None |
| `scan` | core | `skills/scan/SKILL.md` | None |
| `shard` | core | `skills/shard/SKILL.md` | None |
| `sight` | core | `skills/sight/SKILL.md` | None |
| `verify` | core | `skills/verify/SKILL.md` | None |
| `ci-cd-pipeline` | deployment | `skills/deployment/ci-cd-pipeline/SKILL.md` | None |
| `docker-setup` | deployment | `skills/deployment/docker-setup/SKILL.md` | None |
| `domain-ssl` | deployment | `skills/deployment/domain-ssl/SKILL.md` | None |
| `hetzner-setup` | deployment | `skills/deployment/hetzner-setup/SKILL.md` | None |
| `netlify-deploy` | deployment | `skills/deployment/netlify-deploy/SKILL.md` | None |
| `railway-deploy` | deployment | `skills/deployment/railway-deploy/SKILL.md` | None |
| `api-designer` | development | `skills/development/api-designer/SKILL.md` | None |
| `changelog-generator` | development | `skills/development/changelog-generator/SKILL.md` | None |
| `code-reviewer` | development | `skills/development/code-reviewer/SKILL.md` | None |
| `mentor` | development | `skills/development/mentor/SKILL.md` | None |
| `migration-planner` | development | `skills/development/migration-planner/SKILL.md` | None |
| `performance-audit` | development | `skills/development/performance-audit/SKILL.md` | None |
| `refactor-planner` | development | `skills/development/refactor-planner/SKILL.md` | None |
| `test-writer` | development | `skills/development/test-writer/SKILL.md` | None |
| `webapp-testing` | development | `skills/development/webapp-testing/SKILL.md` | None |
| `competitor-analysis` | marketing | `skills/marketing/competitor-analysis/SKILL.md` | None |
| `facebook-ad` | marketing | `skills/marketing/facebook-ad/SKILL.md` | None |
| `google-ad` | marketing | `skills/marketing/google-ad/SKILL.md` | None |
| `launch-plan` | marketing | `skills/marketing/launch-plan/SKILL.md` | None |
| `lead-magnet` | marketing | `skills/marketing/lead-magnet/SKILL.md` | None |
| `pricing-strategy` | marketing | `skills/marketing/pricing-strategy/SKILL.md` | None |
| `sales-funnel` | marketing | `skills/marketing/sales-funnel/SKILL.md` | None |
| `webinar-script` | marketing | `skills/marketing/webinar-script/SKILL.md` | None |
| `feature-spec` | product | `skills/product/feature-spec/SKILL.md` | None |
| `feedback-analyzer` | product | `skills/product/feedback-analyzer/SKILL.md` | None |
| `mvp-scoper` | product | `skills/product/mvp-scoper/SKILL.md` | None |
| `prd-writer` | product | `skills/product/prd-writer/SKILL.md` | None |
| `roadmap-builder` | product | `skills/product/roadmap-builder/SKILL.md` | None |
| `user-story-generator` | product | `skills/product/user-story-generator/SKILL.md` | None |
| `api-audit` | security | `skills/security/api-audit/SKILL.md` | None |
| `csp-headers` | security | `skills/security/csp-headers/SKILL.md` | None |
| `dependency-audit` | security | `skills/security/dependency-audit/SKILL.md` | None |
| `owasp-top10` | security | `skills/security/owasp-top10/SKILL.md` | None |
| `rls-checker` | security | `skills/security/rls-checker/SKILL.md` | None |
| `rls-guardian` | security | `skills/security/rls-guardian/SKILL.md` | None |
| `secrets-scanner` | security | `skills/security/secrets-scanner/SKILL.md` | None |
| `ai-search-visibility` | seo-geo | `skills/seo-geo/ai-search-visibility/SKILL.md` | references/ |
| `keyword-research` | seo-geo | `skills/seo-geo/keyword-research/SKILL.md` | None |
| `local-seo` | seo-geo | `skills/seo-geo/local-seo/SKILL.md` | None |
| `meta-tag-optimizer` | seo-geo | `skills/seo-geo/meta-tag-optimizer/SKILL.md` | None |
| `schema-markup` | seo-geo | `skills/seo-geo/schema-markup/SKILL.md` | references/ |
| `site-audit` | seo-geo | `skills/seo-geo/site-audit/SKILL.md` | references/, scripts/ |
