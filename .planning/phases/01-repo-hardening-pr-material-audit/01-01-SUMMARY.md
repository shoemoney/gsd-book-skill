---
plan: 01-01
status: complete
---

# Plan 01-01 Summary — Requirements + Python packaging

All four tasks completed successfully with no blockers.

## Tasks

- **Task 1 — pypdf import check (read-only):** Ran `git grep -lE "^(import pypdf|from pypdf)" skill/` and confirmed pypdf is imported by `skill/scripts/postprocess.py`. Decision: include `pypdf>=4.0` in both `requirements.txt` and `pyproject.toml`. No commit (read-only).
- **Task 2 — `requirements.txt`:** Created at repo root with `Pillow>=10.0` and `pypdf>=4.0`, plus header comments documenting Python 3.10+ and external CLIs (pandoc, headless Chrome, ImageMagick 7+). Commit: `dddf0e8`.
- **Task 3 — `pyproject.toml`:** Created with PEP 621 metadata (name `kdp-book-launch`, version `0.1.0`, MIT license matching `LICENSE`, `requires-python = ">=3.10"`, project.urls including GSD upstream credit). No `[build-system]` block (intentional — not publishing to PyPI). Validated with `tomllib`. Commit: `a6eb5aa`.
- **Task 4 — README Requirements section:** README already had a `## Requirements` section; per plan instruction ("MERGE rather than duplicate: replace the existing section with the canonical version"), the existing section was replaced with the canonical version naming Python 3.10+, `pip install -r requirements.txt`, pandoc, headless Chrome/Chromium, ImageMagick 7+, and `OPENROUTER_API_KEY`. No other README sections modified. Commit: `18c62d5`.

## Deviations

- The README already contained a `## Requirements` section; the plan anticipated this case and instructed merge-by-replace. Followed that path. The old section listed `epubcheck`, `Node.js + npx`, and OS-support notes that are not in the canonical version; these were dropped per plan's explicit "replace the existing section with the canonical version" directive.

## Commits

- `dddf0e8` feat(01-01): add requirements.txt
- `a6eb5aa` feat(01-01): add pyproject.toml
- `18c62d5` docs(01-01): add Requirements section to README
