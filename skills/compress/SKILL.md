---
name: compress
description: "Use when the user asks for 'headroom', 'compression', 'token savings', 'proxy status', or asks about context window usage."
---

> **Conversion status: Needs manual review.** This skill was converted from a Claude/MemStack skill, but it contains runtime-specific assumptions that may not apply directly in Manus. Review before relying on it in production.
>
> **Review triggers:** CC session, Claude, Claude Code.

> **Original source:** `cwinvestments/memstack/skills/compress/SKILL.md`.

# ️ Compress — Headroom Proxy Manager
*Monitor and manage Headroom context compression for Manus sessions.*

## Context Guard

| Context | Status |
|---------|--------|
| **User says "headroom", "compression stats", "check proxy"** | ACTIVE — run status check |
| **User asks about token savings or context window** | ACTIVE — run session report |
| **Proxy errors or API connection failures appear** | ACTIVE — run health diagnostics |
| **General discussion about CC features** | DORMANT — do not activate |
| **User is actively coding (no proxy issues)** | DORMANT — do not activate |

## What It Does

Headroom is a transparent proxy between Manus and the Anthropic API that compresses tool outputs by removing redundant boilerplate. It extends effective context window by 30–40%.

This skill checks proxy health, reports compression stats, and troubleshoots connection issues.

## Prerequisites

- **Headroom installed:** `pip install headroom-ai[code]`
  - The `[code]` extra installs tree-sitter for AST-based code compression. Without it, Code-Aware compression is disabled and Manus sessions get 0% compression.
- **Proxy running:** `headroom proxy --llmlingua-device cpu` (defaults to `localhost:8787`)
- **CC configured:** `ANTHROPIC_BASE_URL=http://127.0.0.1:8787`

### Recommended Startup

```bash
headroom proxy --llmlingua-device cpu
```

- `--llmlingua-device cpu` — Forces LLMLingua to use CPU (avoids silent CUDA failures on machines without GPU)
- Default port is 8787, no other flags needed
- Code-Aware and LLMLingua both load lazily when relevant content is detected

## Workflow

### 1. Status Check

Run:
```bash
curl -s http://127.0.0.1:8787/stats | python -m json.tool
```

Report: proxy up/down, requests processed, compression ratio, tokens saved, estimated cost savings.

### 2. Health Diagnostics

If proxy is unreachable:

1. Check if process is running:
   ```bash
   # Windows
   tasklist | findstr headroom
   # Linux/macOS
   ps aux | grep headroom
   ```
2. Check port binding:
   ```bash
   netstat -ano | findstr 8787
   ```
3. Verify `ANTHROPIC_BASE_URL` is set:
   ```bash
   echo $ANTHROPIC_BASE_URL
   ```
4. Restart: `headroom proxy` in a separate terminal

### 3. Session Report

When triggered at session end or on request, report:

- Requests this session
- Tokens before/after compression
- Compression ratio (target: 30–40%)
- Estimated dollar savings (at $15/MTok input, $75/MTok output for Opus)

### 4. Configuration Reference

| Setting | Value | Notes |
|---------|-------|-------|
| Proxy URL | `http://127.0.0.1:8787` | Default port |
| Dashboard | AdminStack Infrastructure tab | Headroom monitoring panel |
| Repo | `github.com/chopratejas/headroom` | Apache 2.0 |
| Python | 3.14 compatible | Tested Feb 2026 |

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| **0% compression / 0.00x ratio** | `headroom-ai[code]` is not installed. Run: `pip install headroom-ai[code]`. Restart proxy. |
| **"Code-Aware: NOT INSTALLED" in startup banner** | Same fix — install the `[code]` extra and restart. |
| **Cost figures don't match Anthropic Console** | Headroom estimates costs at list token prices without accounting for Anthropic's server-side prompt caching discounts. For actual costs, check console.anthropic.com. |

## Output Format

```
️ Headroom Status
├── Proxy:  Running on :8787
├── Requests: 47 processed
├── Compression: 46.2% reduction
├── Tokens saved: ~18,500 tokens
└── Cost savings: ~$0.28 this session
```

## Integration

- **AdminStack:** Infrastructure page has Headroom tab with live dashboard
- **CC Sessions:** Auto-routed when `ANTHROPIC_BASE_URL` is set
- **Monitoring:** Stats endpoint polled every 30s with visibility-aware polling
