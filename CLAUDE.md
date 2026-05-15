<!-- GSD:project-start source:PROJECT.md -->
## Project

**gsd-book-skill — Upstream PR Milestone**

`gsd-book-skill` (installed as `kdp-book-launch`) is a Claude Code skill that drives an end-to-end Amazon KDP book launch — editorial review, AI cover/chapter image generation, EPUB + paperback/hardcover PDF builds, and KDP listing prep — through GSD's `.planning/` workflow. This project is the **PR-prep milestone**: get the skill listed as a community plugin in the upstream `gsd-build/get-shit-done` README.

**Core Value:** **A reviewer on `gsd-build/get-shit-done` looks at this repo, finds nothing embarrassing, and approves the PR adding the Community row.** Everything else (hardening, examples, future features) is in service of that single decision.

### Constraints

- **Process:** Enhancement type, not Fix or Feature. Must follow CONTRIBUTING.md sequence verbatim — gates exist and reviewers enforce them.
- **Diff size:** Single-row README addition + a `.changeset/` fragment. Anything outside that scope (CI, deep refactors) widens the PR and slows approval. Strict discipline on Out of Scope.
- **Commit format:** Conventional commits with linked issue number (e.g., `docs(NNNN): add kdp-book-launch to Community table (#NNNN)`).
- **Repository hygiene:** Repo URL in any text the reviewer sees (HTTP-Referer, README, SKILL.md) must point at the real public URL of this repo — no `local/` placeholders.
- **Timing:** No hard deadline, but a stale Enhancement issue with no maintainer activity > 2 weeks may need a polite nudge per Discord-community norms.
<!-- GSD:project-end -->

<!-- GSD:stack-start source:codebase/STACK.md -->
## Technology Stack

## Languages
- **Python 3.10+** — all 12 scripts under `skill/scripts/` are `#!/usr/bin/env python3`. Uses 3.10+ syntax (`str | None` unions, `from __future__ import annotations`).
- **Markdown** — `skill/SKILL.md` (the runbook Claude loads), 8 methodology docs in `skill/references/`, repo-level docs.
- **HTML/CSS** — embedded as string literals in `skill/scripts/build_print_pdf.py` (print-CSS for paginated PDF: 6x9 trim, KDP margins, EB Garamond stack, `@page` rules).
- **JSON** — config schema lives in `skill/templates/book_config.json.template` and `skill/templates/launch_config.json.template`.
## Runtime / dependencies
- **stdlib only** for most scripts: `argparse`, `pathlib`, `json`, `re`, `subprocess`, `urllib.request`, `urllib.error`, `base64`, `tempfile`, `zipfile`, `shutil`, `datetime`, `time`, `os`, `sys`.
- **Pillow (PIL)** — `Image, ImageDraw, ImageFont, ImageFilter` used in `compose_cover_wrap.py`, `make_bw_variants.py`, `build_visual_companion.py`, `build_social_pack.py`, `generate_chapter_images.py`.
- **pypdf** — `PdfReader, PdfWriter` used in `skill/scripts/postprocess.py` (PDF metadata stamping) and `skill/scripts/epub_embed_images.py`.
## External CLI tools (shelled out via `subprocess`)
- **pandoc** — markdown → HTML conversion in `skill/scripts/build_print_pdf.py` (line 162). Install: `brew install pandoc` or `apt-get install pandoc`.
- **Chrome / Chromium (headless)** — HTML → PDF in `build_print_pdf.py`. Searches `/Applications/Google Chrome.app/...`, `/usr/bin/google-chrome`, `/usr/bin/chromium`, or `--chrome` / `$CHROME_BIN` override.
- **kdp-book-generator** (Node CLI, separate npm package) — orchestrated, not bundled. `skill/scripts/build_book_md.py` assembles `_build_book.md` for it. Referenced in `skill/SKILL.md`.
## System fonts (filesystem paths, macOS-first)
- `/System/Library/Fonts/Supplemental/Georgia Bold.ttf` (+ regular / italic)
- Linux fallback `/Library/Fonts/Georgia*.ttf`
- Times New Roman as final fallback
## Configuration files
- `skill/scripts/_config.py` — shared launch-config loader (`find_config`, `load_config`, `project_root`, `add_config_arg`). All scripts import from it.
- `skill/templates/launch_config.json.template` — central per-book config (title, author, paths, protagonist refs, chapter manifest, BISAC, keywords). Copied to consumer project as `_launch_config.json`.
- `skill/templates/book_config.json.template` — companion config consumed by `kdp-book-generator`.
- `.gitignore` — excludes `dist/`, `images/chapters/`, `covers/*.jpg`, `social/`, `.keys`, `.env`, `__pycache__/`, `.venv/`, OpenRouter response dumps under `/tmp/`.
## Distribution / packaging
## Tooling expected by contributors
## Repo-level
- `LICENSE` — MIT (per `README.md` references).
- `CHANGELOG.md` — Keep-a-Changelog format, SemVer; currently `[Unreleased]` pre-`0.1.0`.
- `.github/workflows/` — empty directory, CI pending.
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

## Python style
- **Target:** Python 3.10+ (uses PEP 604 unions like `str | None`, `from __future__ import annotations`). Explicit reminder in `skill/scripts/postprocess.py` header: "Python 3.10+ ... pipx or venv (PEP 668)."
- **Shebang + docstring:** Every executable script opens with `#!/usr/bin/env python3` followed by a triple-quoted module docstring that documents purpose, pipeline steps, required external tools, and `Usage:` examples. See `skill/scripts/build_print_pdf.py` lines 1-25.
- **Imports:** `from __future__ import annotations` first, then stdlib, then third-party (`pypdf` in `postprocess.py`), then the local `_config` import. Local sibling import uses an explicit `sys.path` insert so scripts work when run as `python3 path/to/script.py`:
- **Paths:** Always `pathlib.Path`, never `os.path.join`. Resolve early with `.expanduser().resolve()`.
- **Type hints:** Used on public functions (`resolve_image_path(slug: str, root: Path, ...) -> str | None`), often omitted on small internal helpers.
## Naming
- **Files:** `snake_case.py`. Shared/private modules prefixed with underscore: `_config.py`.
- **Functions:** `snake_case`, verbs (`find_config`, `load_config`, `make_front_matter`, `insert_image`).
- **Constants:** `UPPER_SNAKE_CASE` at module level (`DEFAULT_CONFIG_NAME`, `DEFAULT_CHROME_PATHS`, `CSS`, `HTML_WRAPPER` in `build_print_pdf.py`).
- **Private helpers:** Leading underscore (`_strip_comments`).
## CLI / argparse pattern
## Configuration loading
## Error handling
- **Fail fast with `sys.exit(message)`.** No custom exception classes, no traceback gymnastics. Examples in `_config.py`: `sys.exit(f"config not found: {p}")`, `sys.exit(f"no config found. Looked for ...")`.
- **Warnings, not errors, for recoverable cases:** `print(f"  WARN: no image for {slug}", file=sys.stderr)` in `build_book_md.py` — script continues, user sees it on stderr.
- **External-tool failures** rely on `subprocess.run(..., check=True)` to raise `CalledProcessError`. Headless Chrome stdout/stderr are silenced (`DEVNULL`) in `build_print_pdf.py`.
- **Cleanup with `try/finally`** for temp files (the temp HTML in `build_print_pdf.py` is unlinked even on Chrome failure).
## Comment / docstring practices
- Module docstrings are long-form and tutorial-style: explain *why*, list pipeline steps, name external dependencies, include `Usage:` block. They double as `--help` text via `description=__doc__.split("\n\n")[0]`.
- Inline comments are sparse and reserved for non-obvious regex intent ("Pattern A: header + optional blank + ### location block + trailing blank" in `build_book_md.py`).
- Function docstrings are short or omitted when the name + signature carry the meaning.
## Markdown conventions (`skill/SKILL.md`, `skill/references/*.md`)
- **YAML frontmatter only on `SKILL.md`** (`name:` + `description:` — Anthropic skill manifest format). References are plain markdown, no frontmatter.
- **ATX headings** (`#`, `##`, `###`). H1 reserved for document title.
- **Horizontal rules (`---`) as section separators** between major chunks of a reference doc.
- **Hard-wrapped prose** around 65-72 columns in references (`canon-audit-methodology.md`, `likeness-audit-methodology.md`); SKILL.md uses longer lines.
- **Bullet style:** `-` for lists. Bold-prefixed labels: `**HIGH** — Must be fixed before launch.`
- **Code fences are language-tagged** (` ```python `, ` ```json `).
## Template conventions (`skill/templates/*.template`)
- **Placeholder syntax:** `{ALL_CAPS_PLACEHOLDER}` — not Jinja, not Mustache, not `${}`. Filled by humans (or by Claude editing the file in place), never by Python `str.format()`. Examples: `{BOOK TITLE}`, `{PROTAGONIST_NAME}`, `{PRONOUN_POSSESSIVE}` in `chapter-image-prompt.txt.template`; `{TITLE}`, `{AUTHOR}` in `kdp-listing.md.template`.
- **JSON templates** (`launch_config.json.template`, `book_config.json.template`) carry a top-level `"_comment"` documenting copy/edit instructions, which `_config._strip_comments` removes at load time. This lets the template be a working JSON file that doubles as inline docs.
- **`.template` suffix** signals "copy this, then edit" — copies are gitignored at the project (consumer) level, not here.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

## Pattern
## Entry point
## Layered structure
## Data flow — manuscript to launch
```
```
## Key abstractions
- **`_config.py`** at `skill/scripts/_config.py` — shared config loader; every other script imports it. Single point of contract with `_launch_config.json`.
- **Slug-keyed chapters** — `chapters[].slug` in the config maps 1:1 to `prompts/chapters/{slug}.txt` and `images/chapters/{slug}.png`. This is how generation, audit, regen, and build stay synchronized.
- **Idempotent generators** — `generate_chapter_images.py` skips existing files unless `--force` is passed; supports `--regen-log` for audit trail.
- **External dependency boundary** — the skill draws a hard line at EPUB compilation: it generates the input markdown and embeds images post-hoc, but the actual EPUB packaging is delegated to `kdp-book-generator` (Node, installed separately).
## GSD integration
<!-- GSD:architecture-end -->

<!-- GSD:skills-start source:skills/ -->
## Project Skills

No project skills found. Add skills to any of: `.claude/skills/`, `.agents/skills/`, `.cursor/skills/`, `.github/skills/`, or `.codex/skills/` with a `SKILL.md` index file.
<!-- GSD:skills-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->
