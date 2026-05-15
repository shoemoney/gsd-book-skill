---
generated: 2026-05-15
focus: tech
---

# STACK

Lightweight Python skill that ships as a folder of markdown + scripts + templates. No build system, no compiled artifacts, no package registry.

## Languages

- **Python 3.10+** — all 12 scripts under `skill/scripts/` are `#!/usr/bin/env python3`. Uses 3.10+ syntax (`str | None` unions, `from __future__ import annotations`).
- **Markdown** — `skill/SKILL.md` (the runbook Claude loads), 8 methodology docs in `skill/references/`, repo-level docs.
- **HTML/CSS** — embedded as string literals in `skill/scripts/build_print_pdf.py` (print-CSS for paginated PDF: 6x9 trim, KDP margins, EB Garamond stack, `@page` rules).
- **JSON** — config schema lives in `skill/templates/book_config.json.template` and `skill/templates/launch_config.json.template`.

## Runtime / dependencies

There is **no `requirements.txt`, `pyproject.toml`, `setup.py`, or `package.json`** in the repo. Dependencies are inferred from imports and documented in `README.md`:

- **stdlib only** for most scripts: `argparse`, `pathlib`, `json`, `re`, `subprocess`, `urllib.request`, `urllib.error`, `base64`, `tempfile`, `zipfile`, `shutil`, `datetime`, `time`, `os`, `sys`.
- **Pillow (PIL)** — `Image, ImageDraw, ImageFont, ImageFilter` used in `compose_cover_wrap.py`, `make_bw_variants.py`, `build_visual_companion.py`, `build_social_pack.py`, `generate_chapter_images.py`.
- **pypdf** — `PdfReader, PdfWriter` used in `skill/scripts/postprocess.py` (PDF metadata stamping) and `skill/scripts/epub_embed_images.py`.

`README.md` line ~137: "Python 3.10+ with `pypdf` and `PIL` (Pillow) installed (PEP 668 — use `pipx` or system packages)".

`CONTRIBUTING.md` explicitly resists adding new external deps: *"PRs that introduce new external dependencies without prior discussion... the whole point of this is to be lightweight — Python stdlib + PIL + pypdf + a couple of brew tools."*

## External CLI tools (shelled out via `subprocess`)

- **pandoc** — markdown → HTML conversion in `skill/scripts/build_print_pdf.py` (line 162). Install: `brew install pandoc` or `apt-get install pandoc`.
- **Chrome / Chromium (headless)** — HTML → PDF in `build_print_pdf.py`. Searches `/Applications/Google Chrome.app/...`, `/usr/bin/google-chrome`, `/usr/bin/chromium`, or `--chrome` / `$CHROME_BIN` override.
- **kdp-book-generator** (Node CLI, separate npm package) — orchestrated, not bundled. `skill/scripts/build_book_md.py` assembles `_build_book.md` for it. Referenced in `skill/SKILL.md`.

## System fonts (filesystem paths, macOS-first)

Hardcoded in `skill/scripts/compose_cover_wrap.py`, `build_visual_companion.py`, `build_social_pack.py`:

- `/System/Library/Fonts/Supplemental/Georgia Bold.ttf` (+ regular / italic)
- Linux fallback `/Library/Fonts/Georgia*.ttf`
- Times New Roman as final fallback

Print CSS also references **EB Garamond** with Georgia / Times New Roman fallbacks.

## Configuration files

- `skill/scripts/_config.py` — shared launch-config loader (`find_config`, `load_config`, `project_root`, `add_config_arg`). All scripts import from it.
- `skill/templates/launch_config.json.template` — central per-book config (title, author, paths, protagonist refs, chapter manifest, BISAC, keywords). Copied to consumer project as `_launch_config.json`.
- `skill/templates/book_config.json.template` — companion config consumed by `kdp-book-generator`.
- `.gitignore` — excludes `dist/`, `images/chapters/`, `covers/*.jpg`, `social/`, `.keys`, `.env`, `__pycache__/`, `.venv/`, OpenRouter response dumps under `/tmp/`.

## Distribution / packaging

This is a **Claude Code Skill**, not a Python package. Install model (per `README.md`):

```bash
git clone https://github.com/shoemoney/gsd-book-skill ~/Projects/gsd-book-skill
ln -s ~/Projects/gsd-book-skill/skill ~/.claude/skills/kdp-book-launch
```

The `skill/` folder is the installable unit. `skill/SKILL.md` carries YAML frontmatter (`name: kdp-book-launch`, `description: ...`) that Claude reads to decide when to trigger the skill. No marketplace.json or plugin manifest exists in the repo.

## Tooling expected by contributors

`CONTRIBUTING.md` cites `pytest` and `ruff check .` (PEP 8, single-quote strings, 4-space indent). No `tests/` directory exists yet. `.github/workflows/` exists but is empty — CI is in the TASKS.md pending list.

## Repo-level

- `LICENSE` — MIT (per `README.md` references).
- `CHANGELOG.md` — Keep-a-Changelog format, SemVer; currently `[Unreleased]` pre-`0.1.0`.
- `.github/workflows/` — empty directory, CI pending.
