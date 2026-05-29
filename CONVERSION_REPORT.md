# Conversion Report

This report covers the conversion of the 84 public/free MemStack skills into Manus-compatible skill packages.

| Metric | Count |
|---|---:|
| Converted skills | 84 |
| Marked needs review | 16 |
| Resource groups/files copied | 6 |

## Manual Review Updates

| Skill | Status | Change made |
|---|---|---|
| `secrets-scanner` | Manus-ready draft | Removed `CLAUDE.md` from the documentation scan command and rewrote MemStack Pro hook coverage as optional generic pre-commit/pre-push secret scanning guidance. |

## Skills Marked Needs Review

| Skill | Category | Main review triggers |
|---|---|---|
| `hosted-mcp-catalog` | automation | .mcp.json, Claude, Claude Code, Claude Desktop, MCP |
| `compress` | core | CC session, Claude, Claude Code |
| `diary` | core | $MEMSTACK_PATH, .claude, CC session, Claude, Claude Code, PostToolUse, PowerShell, PreToolUse, SessionStart, memstack-db.py |
| `echo` | core | $MEMSTACK_PATH, .claude, CC session, Claude, memstack-db.py |
| `familiar` | core | $MEMSTACK_PATH, CC session |
| `forge` | core | $MEMSTACK_PATH |
| `governor` | core | Claude |
| `grimoire` | core | Claude |
| `marketplace-submit` | marketing | CC session, Claude, Claude Code, MCP |
| `project` | core | $MEMSTACK_PATH, CC session, memstack-db.py |
| `quill` | core | $MEMSTACK_PATH, memstack-db.py |
| `ai-search-visibility` | seo-geo | Claude |
| `site-audit` | seo-geo | Claude |
| `state` | core | .claude, Claude |
| `token-optimization` | core | Claude, Claude Code, MCP, claude mcp |
| `work` | core | $MEMSTACK_PATH, .claude, Claude, memstack-db.py |

## Full Converted Skill Inventory

| Skill | Category | Status | Source | Resources |
|---|---|---|---|---|
| `api-integration` | automation | Manus-ready draft | `skills/automation/api-integration/SKILL.md` | None |
| `content-pipeline` | automation | Manus-ready draft | `skills/automation/content-pipeline/SKILL.md` | None |
| `cron-scheduler` | automation | Manus-ready draft | `skills/automation/cron-scheduler/SKILL.md` | None |
| `hosted-mcp-catalog` | automation | Needs review | `skills/automation/hosted-mcp-catalog/SKILL.md` | None |
| `n8n-workflow-builder` | automation | Manus-ready draft | `skills/automation/n8n-workflow-builder/SKILL.md` | None |
| `webhook-designer` | automation | Manus-ready draft | `skills/automation/webhook-designer/SKILL.md` | None |
| `client-onboarding` | business | Manus-ready draft | `skills/business/client-onboarding/SKILL.md` | None |
| `contract-template` | business | Manus-ready draft | `skills/business/contract-template/SKILL.md` | None |
| `financial-model` | business | Manus-ready draft | `skills/business/financial-model/SKILL.md` | None |
| `freelancer-toolkit` | business | Manus-ready draft | `skills/business/freelancer-toolkit/SKILL.md` | None |
| `gdpr` | business | Manus-ready draft | `skills/business/gdpr/SKILL.md` | None |
| `invoice-generator` | business | Manus-ready draft | `skills/business/invoice-generator/SKILL.md` | None |
| `licensing` | business | Manus-ready draft | `skills/business/licensing/SKILL.md` | None |
| `proposal-writer` | business | Manus-ready draft | `skills/business/proposal-writer/SKILL.md` | None |
| `scope-of-work` | business | Manus-ready draft | `skills/business/scope-of-work/SKILL.md` | None |
| `sop-builder` | business | Manus-ready draft | `skills/business/sop-builder/SKILL.md` | None |
| `compress` | core | Needs review | `skills/compress/SKILL.md` | None |
| `blog-post` | content | Manus-ready draft | `skills/content/blog-post/SKILL.md` | None |
| `email-sequence` | content | Manus-ready draft | `skills/content/email-sequence/SKILL.md` | None |
| `landing-page-copy` | content | Manus-ready draft | `skills/content/landing-page-copy/SKILL.md` | None |
| `newsletter` | content | Manus-ready draft | `skills/content/newsletter/SKILL.md` | None |
| `product-description` | content | Manus-ready draft | `skills/content/product-description/SKILL.md` | None |
| `tiktok-script` | content | Manus-ready draft | `skills/content/tiktok-script/SKILL.md` | None |
| `twitter-thread` | content | Manus-ready draft | `skills/content/twitter-thread/SKILL.md` | None |
| `youtube-script` | content | Manus-ready draft | `skills/content/youtube-script/SKILL.md` | None |
| `ci-cd-pipeline` | deployment | Manus-ready draft | `skills/deployment/ci-cd-pipeline/SKILL.md` | None |
| `docker-setup` | deployment | Manus-ready draft | `skills/deployment/docker-setup/SKILL.md` | None |
| `domain-ssl` | deployment | Manus-ready draft | `skills/deployment/domain-ssl/SKILL.md` | None |
| `hetzner-setup` | deployment | Manus-ready draft | `skills/deployment/hetzner-setup/SKILL.md` | None |
| `netlify-deploy` | deployment | Manus-ready draft | `skills/deployment/netlify-deploy/SKILL.md` | None |
| `railway-deploy` | deployment | Manus-ready draft | `skills/deployment/railway-deploy/SKILL.md` | None |
| `api-designer` | development | Manus-ready draft | `skills/development/api-designer/SKILL.md` | None |
| `changelog-generator` | development | Manus-ready draft | `skills/development/changelog-generator/SKILL.md` | None |
| `code-reviewer` | development | Manus-ready draft | `skills/development/code-reviewer/SKILL.md` | None |
| `mentor` | development | Manus-ready draft | `skills/development/mentor/SKILL.md` | None |
| `migration-planner` | development | Manus-ready draft | `skills/development/migration-planner/SKILL.md` | None |
| `performance-audit` | development | Manus-ready draft | `skills/development/performance-audit/SKILL.md` | None |
| `refactor-planner` | development | Manus-ready draft | `skills/development/refactor-planner/SKILL.md` | None |
| `test-writer` | development | Manus-ready draft | `skills/development/test-writer/SKILL.md` | None |
| `webapp-testing` | development | Manus-ready draft | `skills/development/webapp-testing/SKILL.md` | None |
| `diary` | core | Needs review | `skills/diary/SKILL.md` | None |
| `echo` | core | Needs review | `skills/echo/SKILL.md` | UPGRADE_PLAN.md, index-sessions.py, search.py |
| `familiar` | core | Needs review | `skills/familiar/SKILL.md` | None |
| `forge` | core | Needs review | `skills/forge/SKILL.md` | None |
| `governor` | core | Needs review | `skills/governor/SKILL.md` | None |
| `grimoire` | core | Needs review | `skills/grimoire/SKILL.md` | None |
| `humanize` | core | Manus-ready draft | `skills/humanize/SKILL.md` | None |
| `competitor-analysis` | marketing | Manus-ready draft | `skills/marketing/competitor-analysis/SKILL.md` | None |
| `facebook-ad` | marketing | Manus-ready draft | `skills/marketing/facebook-ad/SKILL.md` | None |
| `google-ad` | marketing | Manus-ready draft | `skills/marketing/google-ad/SKILL.md` | None |
| `launch-plan` | marketing | Manus-ready draft | `skills/marketing/launch-plan/SKILL.md` | None |
| `lead-magnet` | marketing | Manus-ready draft | `skills/marketing/lead-magnet/SKILL.md` | None |
| `marketplace-submit` | marketing | Needs review | `skills/marketing/marketplace-submit/SKILL.md` | None |
| `pricing-strategy` | marketing | Manus-ready draft | `skills/marketing/pricing-strategy/SKILL.md` | None |
| `sales-funnel` | marketing | Manus-ready draft | `skills/marketing/sales-funnel/SKILL.md` | None |
| `webinar-script` | marketing | Manus-ready draft | `skills/marketing/webinar-script/SKILL.md` | None |
| `feature-spec` | product | Manus-ready draft | `skills/product/feature-spec/SKILL.md` | None |
| `feedback-analyzer` | product | Manus-ready draft | `skills/product/feedback-analyzer/SKILL.md` | None |
| `mvp-scoper` | product | Manus-ready draft | `skills/product/mvp-scoper/SKILL.md` | None |
| `prd-writer` | product | Manus-ready draft | `skills/product/prd-writer/SKILL.md` | None |
| `roadmap-builder` | product | Manus-ready draft | `skills/product/roadmap-builder/SKILL.md` | None |
| `user-story-generator` | product | Manus-ready draft | `skills/product/user-story-generator/SKILL.md` | None |
| `project` | core | Needs review | `skills/project/SKILL.md` | None |
| `quill` | core | Needs review | `skills/quill/SKILL.md` | None |
| `scan` | core | Manus-ready draft | `skills/scan/SKILL.md` | None |
| `api-audit` | security | Manus-ready draft | `skills/security/api-audit/SKILL.md` | None |
| `csp-headers` | security | Manus-ready draft | `skills/security/csp-headers/SKILL.md` | None |
| `dependency-audit` | security | Manus-ready draft | `skills/security/dependency-audit/SKILL.md` | None |
| `owasp-top10` | security | Manus-ready draft | `skills/security/owasp-top10/SKILL.md` | None |
| `rls-checker` | security | Manus-ready draft | `skills/security/rls-checker/SKILL.md` | None |
| `rls-guardian` | security | Manus-ready draft | `skills/security/rls-guardian/SKILL.md` | None |
| `secrets-scanner` | security | Manus-ready draft | `skills/security/secrets-scanner/SKILL.md` | None |
| `ai-search-visibility` | seo-geo | Needs review | `skills/seo-geo/ai-search-visibility/SKILL.md` | None |
| `keyword-research` | seo-geo | Manus-ready draft | `skills/seo-geo/keyword-research/SKILL.md` | None |
| `local-seo` | seo-geo | Manus-ready draft | `skills/seo-geo/local-seo/SKILL.md` | None |
| `meta-tag-optimizer` | seo-geo | Manus-ready draft | `skills/seo-geo/meta-tag-optimizer/SKILL.md` | None |
| `schema-markup` | seo-geo | Manus-ready draft | `skills/seo-geo/schema-markup/SKILL.md` | references/ |
| `site-audit` | seo-geo | Needs review | `skills/seo-geo/site-audit/SKILL.md` | references/, scripts/ |
| `shard` | core | Manus-ready draft | `skills/shard/SKILL.md` | None |
| `sight` | core | Manus-ready draft | `skills/sight/SKILL.md` | None |
| `state` | core | Needs review | `skills/state/SKILL.md` | None |
| `token-optimization` | core | Needs review | `skills/token-optimization/SKILL.md` | None |
| `verify` | core | Manus-ready draft | `skills/verify/SKILL.md` | None |
| `work` | core | Needs review | `skills/work/SKILL.md` | None |

## Post-Processing for Manus Size Guidance

The following oversized `SKILL.md` files were shortened by moving detailed sections into `references/` files:

- api-designer: complete-route-handler-template.md
- docker-setup: advanced-docker-operations.md
- hetzner-setup: operations-hardening-and-backups.md
