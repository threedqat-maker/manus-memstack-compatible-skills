# Manus MemStack Skills

This repository contains a Manus-compatible conversion of some of the **84 public/free skills** from [`cwinvestments/memstack`](https://github.com/cwinvestments/memstack). The original project is MIT-licensed; the original license is preserved in `LICENSE`.

The conversion intentionally excludes Claude runtime infrastructure such as `.claude/`, hooks, local MCP loader configuration, dashboard files, and non-public Pro skills. Skills that still contain Claude/MemStack-specific assumptions are marked **Needs manual review** at the top of their `SKILL.md` file and listed in `CONVERSION_REPORT.md`.

## Structure

Each converted skill is stored as a self-contained Manus skill package:

```text
skills/<skill-name>/
├── SKILL.md
└── references/ or scripts/ when present in the original public source
```

## Counts

| Metric | Count |
|---|---:|
| Converted public/free skills | 84 |
| Skills marked needs review | 17 |
| Bundled resource groups/files copied | 6 |

## Practical note

This is a first-pass, credit-efficient conversion. The domain-specific workflows are largely preserved, but runtime-specific skills should be tested and revised before operational use in Manus.
