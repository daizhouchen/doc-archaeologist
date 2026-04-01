# doc-archaeologist

> Dig up stale docs, broken references, and misleading comments before they mislead your team.

An [OpenClaw](https://openclawskill.ai) skill that scans project documentation and code comments, finds outdated, inconsistent, or misleading content, and generates an archaeology report with a health grade (A-F) and fix suggestions.

## Features

- **4-Phase Archaeology Pipeline**
  1. **Artifact Discovery** — Finds all docs (README, CHANGELOG, docs/, .md) and code comments (docstrings, JSDoc, TODO/FIXME/HACK/DEPRECATED)
  2. **Carbon Dating** — Compares doc last-modified dates with referenced code changes
  3. **Cross-Verification** — Checks commands, file paths, env vars, and API references against actual code
  4. **Report Generation** — Health grade + findings + fix suggestions

- **Finding Types**

  | Type | Severity | Example |
  |------|----------|---------|
  | Stale docs | Warning | README unchanged 6 months, code changes weekly |
  | Broken file refs | Critical | `See docs/setup.md` but file doesn't exist |
  | Invalid commands | Critical | `npm run deploy` but no such script |
  | Env var drift | Warning | Doc mentions `DB_HOST` but code uses `DATABASE_URL` |
  | Code markers | Info | TODO/FIXME/DEPRECATED in source |

- **Health Grade** — A (excellent) through F (critical issues), computed from severity counts
- **Auto-Generated Fix Suggestions** — Individual fix recommendation files per finding

## Installation

```bash
npx @anthropic-ai/claw@latest skill add daizhouchen/doc-archaeologist
```

## How It Works

1. **Scan** — `scripts/scan_docs.py` inventories all documentation and code comments
2. **Analyze** — `scripts/analyze_freshness.py` checks staleness, references, and consistency
3. **Report** — `scripts/report.py` generates a Markdown report + fix suggestions directory

## Manual Usage

```bash
# Scan a project
python3 scripts/scan_docs.py /path/to/project

# Analyze freshness and consistency
python3 scripts/analyze_freshness.py

# Generate the report
python3 scripts/report.py
# Output: doc-archaeology-report.md + fix-suggestions/
```

## Trigger Phrases

- "文档过期" / "README 需要更新"
- "文档审查" / "文档质量"
- "这个项目的文档是不是有点旧了"

## Project Structure

```
doc-archaeologist/
├── SKILL.md                      # Skill definition and workflow
├── scripts/
│   ├── scan_docs.py              # Documentation inventory scanner
│   ├── analyze_freshness.py      # Staleness and consistency checker
│   └── report.py                 # Report and fix suggestion generator
└── README.md
```

## Report Output

```
doc-archaeology-report.md         # Full report with health grade
fix-suggestions/
├── fix_001_broken_ref.md         # Individual fix recommendations
├── fix_002_stale_readme.md
└── ...
```

## Requirements

- Python 3.8+ (no external packages)
- Git (optional, for staleness analysis via commit history)

## Limitations

- Focuses on Markdown, docstrings, JSDoc comment formats
- Command validation checks for binary existence, not full execution

## License

MIT
