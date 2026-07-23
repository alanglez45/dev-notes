# PDF Export Guide

Export markdown notes to PDF using Puppeteer.

## Commands

| Command | Description |
|---|---|
| `npm run pdf` | Export all topics as individual PDFs |
| `npm run pdf:folder -- notes/frontend/React` | Export all `.md` from a specific folder |

## Manual Usage

```bash
# All topics as individual PDFs
node scripts/md-to-pdf.mjs --all

# Specific folder
node scripts/md-to-pdf.mjs --folder notes/frontend/React

# Single file
node scripts/md-to-pdf.mjs notes/frontend/React/01-fundamentals.md

# Multiple files
node scripts/md-to-pdf.mjs file1.md file2.md
```

## Output

PDFs are saved to `output/` directory.

| Flag | Output |
|---|---|
| `--all` | `output/<Topic>.pdf` per topic folder |
| `--folder <path>` | `output/<folder-name>.pdf` |
| Single file | `output/<file-name>.pdf` |

## Notes

- Uses `index.md` for file ordering when available
- Includes table of contents and page numbers
- Page breaks between sections
