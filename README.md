# Tax Document Renaming & Savedown Automation

A Python tool to automate the renaming and organised savedown of tax documents.

## Overview

Automates renaming and filing of tax documents (1099-DIVs, K-1s) downloaded in bulk.
Each PDF is copied to two destinations with the correct naming convention:

- **By Fund** → `LastName, FullName - Year - FormType - FundName.pdf`
- **By Client** → `FundName - ClientName - Year - FormType.pdf`

## Setup

```bash
# 1. Edit the two paths at the top of the script
SAVE_LOCATION = Path(r"C:\path\to\Save Location")
DROP_FOLDER   = Path(r"C:\path\to\drop folder")

# 2. Fill in config.xlsx (see below)
```

## config.xlsx

Two sheets — `clients` and `funds` — each with these columns:

| Column | Description |
|--------|-------------|
| `canonical_name` | The clean name used in filenames |
| `folder_name` | The exact folder name under By Client / By Fund |
| `name_variants` | Semicolon-separated aliases (how names appear in PDFs/filenames) |

## Usage

```bash
# Normal run — renames and copies files
python "Tax Document Renaming & Savedown Automation.py"

# Debug — prints what was extracted from each PDF, no files moved
python "Tax Document Renaming & Savedown Automation.py" --debug

# Dry run — shows what would be renamed/moved, no files moved
python "Tax Document Renaming & Savedown Automation.py" --dry-run
```

## Project Structure

```
.
├── Tax Document Renaming & Savedown Automation.py   # Main script
├── config.xlsx                                       # Client/fund legend (you maintain this)
├── unmatched_log.csv                                 # Auto-generated: files that couldn't be resolved
├── CLAUDE.md                                         # AI context & dev notes
├── README.md                                         # This file
└── .gitignore
```
