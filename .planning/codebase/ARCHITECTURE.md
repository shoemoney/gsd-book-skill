---
generated: 2026-05-15
focus: arch
---

# ARCHITECTURE

## Pattern

A **Claude Code Skill** (single-skill plugin) that ORCHESTRATES a parameterized Python toolkit plus prompt templates plus methodology references. The skill itself does not implement EPUB/PDF generation — it shells out to the external `kdp-book-generator` Node CLI for that. The skill is GSD-aware: it plugs into the GSD `/gsd-*` slash command loop (`discuss-phase → plan-phase → execute-phase → verify-work`) and the `.planning/` artifact convention.

## Entry point

`skill/SKILL.md` — Claude Code skill manifest with YAML frontmatter (`name`, `description`) and the full runbook. When trigger phrases match (e.g. "publish on Amazon", "build the EPUB", "make chapter art"), Claude loads SKILL.md and follows its 5-phase procedure verbatim. Every other file in the skill is referenced from SKILL.md by relative path.

## Layered structure

Three logical layers, each in its own directory under `skill/`:

1. **Prompts layer — `skill/templates/`** — Parameterized text/JSON templates a user copies into their project and fills in. Drives both AI image generation (cover/chapter prompts) and human-facing collateral (KDP listing, editorial subagent prompt).
2. **Methodology layer — `skill/references/`** — Long-form how-to docs that Claude (or a subagent) reads for procedural knowledge. Not executable; pure prose with checklists and rubrics. Includes the master `phase-checklist.md`.
3. **Automation layer — `skill/scripts/`** — Python 3 scripts that consume one shared config (`_launch_config.json`) and emit build artifacts (images, PDFs, JPGs). All scripts import `_config.py` for unified config loading.

The single source of truth tying the layers together is `_launch_config.json` (user-side, derived from `templates/launch_config.json.template`). Every script reads from it; no script takes per-script config files.

## Data flow — manuscript to launch

```
manuscript.md  +  ref_images/  +  _launch_config.json
         │
PHASE 1  ├── editorial subagent (templates/editorial-prompt.md.template)
         │       └── .planning/notes/MANUSCRIPT-NOTES.md  [GATE]
         │   canon audit (references/canon-audit-methodology.md)
         │       └── manuscript fixes committed as MSS-NN
         ▼
PHASE 2  generate_chapter_images.py  (OpenRouter Gemini, ~$0.14/img)
         │       └── images/chapters/*.png  [GATE: likeness audit]
         │   make_bw_variants.py (imagemagick)
         │       └── images/chapters-bw/*.png
         ▼
PHASE 3  generate_front_cover.py, generate_back_cover.py
         │       └── covers/*.png  [GATE: author pick]
         │   (manual typography overlay → covers/*.jpg)
         │   compose_cover_wrap.py --page-count N
         │       └── dist/*-paperback-wrap.pdf, *-hardcover-wrap.pdf
         ▼
PHASE 4  build_book_md.py        → _build_book.md (color, EPUB)
         │   kdp-book-generator (external Node CLI)
         │       → dist/book.epub
         │   epub_embed_images.py → embeds chapter art
         │   epubcheck            → validates
         │   build_book_md.py --print → _build_book.md (B&W)
         │   build_print_pdf.py (pandoc → headless Chrome)
         │       → dist/book-paperback.pdf, dist/book-hardcover.pdf
         │   postprocess.py       → sets PDF metadata
         ▼
PHASE 5  build_visual_companion.py → dist/visual-companion.pdf
         build_social_pack.py      → 30-50 social JPGs across 6 platforms
         kdp-listing.md.template   → .planning/kdp-listing.md (manual fill)
         launch collateral drafts  → .planning/launch/*.md
         → upload to KDP
```

## Key abstractions

- **`_config.py`** at `skill/scripts/_config.py` — shared config loader; every other script imports it. Single point of contract with `_launch_config.json`.
- **Slug-keyed chapters** — `chapters[].slug` in the config maps 1:1 to `prompts/chapters/{slug}.txt` and `images/chapters/{slug}.png`. This is how generation, audit, regen, and build stay synchronized.
- **Idempotent generators** — `generate_chapter_images.py` skips existing files unless `--force` is passed; supports `--regen-log` for audit trail.
- **External dependency boundary** — the skill draws a hard line at EPUB compilation: it generates the input markdown and embeds images post-hoc, but the actual EPUB packaging is delegated to `kdp-book-generator` (Node, installed separately).

## GSD integration

The 5 phases map 1:1 to GSD roadmap phases. Each phase has explicit `[GATE]` points that align with `/gsd-verify-work` checkpoints. `/gsd-autonomous` can drive the full pipeline with atomic per-task commits.
