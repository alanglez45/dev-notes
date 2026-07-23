# AGENTS.md — dev-notes

This is a personal study-notes repo. It contains **only Markdown files** — no code, no tests, no build system, no runnable commands.

## Navigation

- `index.md` at the root lists all categories and links to each one's index
- All categories live inside `notes/`: `notes/frontend/`, `notes/backend/`, `notes/infra/`, `notes/genai/`, `notes/opencode/`
- Each category has its own `index.md` entry point
- Topic directories each have their own `index.md` inside their category folder

## Content conventions

- Some sections are in Spanish (NextJS, Docker, some BackendNotes) — agents should read and write in whichever language fits the existing file
- Has a `package.json` with dependencies for PDF generation (marked, shiki, puppeteer)
- Scripts in `scripts/` for converting notes to PDF

## Constraints

- Do not add build/test tooling or convert to a code project
- Do not create new category directories without a clear purpose
- Prefer linking to existing notes over duplicating content
