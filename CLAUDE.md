# CLAUDE.md — Tax Document Renaming & Savedown Automation

This file gives Claude (and any developer) the context needed to work effectively on this project.

---

## Project Purpose

Automate the renaming and organised savedown of tax-related documents.

> Details to be expanded as the project takes shape.

---

## Key Files

| File | Purpose |
|------|---------|
| `Tax Document Renaming & Savedown Automation.py` | Main script — entry point |
| `config.xlsx` | Legend: maps name variants → canonical names + folder paths |
| `unmatched_log.csv` | Auto-generated: files the script couldn't resolve |
| `README.md` | Public-facing documentation |
| `CLAUDE.md` | This file — AI and developer context |

---

## Folder Structure (on work machine)

```
Save Location/
└── Savedowns/
    ├── By Client/
    │   └── <ClientFolder>/   e.g. "Smith, John"
    └── By Fund/
        └── <FundFolder>/     e.g. "Vanguard Total Market"
```

---

## Naming Conventions

- **Fund folder**:   `LastName, FullName - Year - FormType - FundName.pdf`
- **Client folder**: `FundName - ClientName - Year - FormType.pdf`

---

## Architecture & Design Decisions

- PDF parsing uses `pdfplumber` with region-based word extraction (top-left for names/fund, top-right for year, full page for form type)
- Filename parsing is a fallback if PDF extraction misses fields
- `rapidfuzz` fuzzy-matches extracted names against `config.xlsx` (threshold: 75)
- `config.xlsx` has two sheets: `clients` and `funds`, each with `canonical_name`, `folder_name`, `name_variants` (semicolon-separated)
- Script has three modes: normal, `--dry-run` (shows what would happen), `--debug` (prints raw PDF extractions)
- Unresolved files are logged to `unmatched_log.csv` rather than silently skipped

## Known Constraints

- Work machine libraries: os, shutil, pathlib, pdfplumber, rapidfuzz, pandas, regex
- PDFs are landscape format; 1099-DIVs have consistent layout; K-1s vary more
- Client names can be multi-line with TTEE, C/O, trust noise — script strips these
- LLCs and trusts without individual names use `config.xlsx` as legend

## Development Notes

- Language: Python 3
- Version control: Git + GitHub (https://github.com/Matheodabo/Tax-Document-Renaming-and-Saving-Automation)
- Started: 2026-04-16
- Iteration approach: run `--debug` on real PDFs and report back what was extracted so parsing can be tuned
