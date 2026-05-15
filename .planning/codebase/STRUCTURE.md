---
generated: 2026-05-15
focus: arch
---

# STRUCTURE

## Top-level layout

```
gsd-book-skill/
├── README.md            project overview + GSD/TÂCHES attribution
├── SKILL.md             (none at root — the manifest lives at skill/SKILL.md)
├── CHANGELOG.md         release notes
├── CONTRIBUTING.md      contributor guide
├── CODE_OF_CONDUCT.md   community standards
├── SECURITY.md          vulnerability reporting
├── LICENSE              MIT
├── TASKS.md             open-source kit task list
├── examples/            empty placeholder for worked examples
├── skill/               the actual Claude Code skill payload
└── .planning/           GSD planning artifacts (this doc lives here)
```

## skill/ — the plugin payload

`/Users/shoemoney/Projects/gsd-book-skill/skill/`

- `skill/SKILL.md` — entry point; Claude Code skill manifest + full runbook
- `skill/references/` — methodology docs (long-form how-to prose)
- `skill/scripts/` — Python 3 automation scripts
- `skill/templates/` — copy-and-fill prompt/config templates

## skill/scripts/ — naming convention

Verb-prefixed Python modules. Every script imports `_config.py` and reads `_launch_config.json` from the user project's CWD.

`/Users/shoemoney/Projects/gsd-book-skill/skill/scripts/`

| Prefix | Purpose | Files |
|---|---|---|
| `_` | shared / private helper | `_config.py` |
| `generate_*` | AI image generation (OpenRouter Gemini) | `generate_chapter_images.py`, `generate_front_cover.py`, `generate_back_cover.py` |
| `build_*` | compile artifact from inputs | `build_book_md.py`, `build_print_pdf.py`, `build_social_pack.py`, `build_visual_companion.py` |
| `compose_*` | layout / multi-asset composition | `compose_cover_wrap.py` |
| `make_*` | transformation pass | `make_bw_variants.py` |
| `*_embed_*` | post-hoc bundling | `epub_embed_images.py` |
| `postprocess` | final metadata pass | `postprocess.py` |

### Where to find each capability

- **Chapter image generation:** `skill/scripts/generate_chapter_images.py`
- **Front / back cover generation:** `skill/scripts/generate_front_cover.py`, `skill/scripts/generate_back_cover.py`
- **Color → B&W conversion:** `skill/scripts/make_bw_variants.py` (uses imagemagick)
- **EPUB-ready markdown build:** `skill/scripts/build_book_md.py` (pass `--print` for B&W variant)
- **EPUB image embedding:** `skill/scripts/epub_embed_images.py`
- **Paperback / hardcover PDF build:** `skill/scripts/build_print_pdf.py` (pandoc → headless Chrome)
- **Cover wrap PDFs (front+spine+back):** `skill/scripts/compose_cover_wrap.py`
- **Social media graphic pack:** `skill/scripts/build_social_pack.py`
- **Visual companion / lead-magnet PDF:** `skill/scripts/build_visual_companion.py`
- **PDF metadata:** `skill/scripts/postprocess.py`
- **Shared config loader:** `skill/scripts/_config.py`

## skill/templates/ — naming convention

Suffix `.template` flags a copy-and-fill artifact. Extension before `.template` is the destination format.

`/Users/shoemoney/Projects/gsd-book-skill/skill/templates/`

- `launch_config.json.template` — central config; copy to `_launch_config.json` in user project
- `book_config.json.template` — config for the external `kdp-book-generator` Node CLI
- `chapter-image-prompt.txt.template` — 6-block prompt scaffold (SUBJECT/SCENE/LIGHTING/COMPOSITION/NEGATIVE/STYLE)
- `front-cover-prompt.txt.template` — AI front cover art prompt (no text overlay)
- `back-cover-prompt.txt.template` — AI back cover art prompt
- `editorial-prompt.md.template` — subagent prompt for the editorial read in Phase 1
- `kdp-listing.md.template` — Amazon listing copy (description, About the Author, From the Author, keywords, BISAC)

## skill/references/ — methodology docs

Pure-prose how-to documents. Claude (or a spawned subagent) reads these for procedural knowledge. Never executed.

`/Users/shoemoney/Projects/gsd-book-skill/skill/references/`

- `phase-checklist.md` — the master 5-phase task checklist (Manuscript Lock → Chapter Illustrations → Cover Production → Build & Compile → KDP Launch Prep)
- `kdp-specifications.md` — KDP trim sizes, bleed, spine width formulas
- `prompt-structure-guide.md` — how to write a chapter image prompt (6-block structure)
- `aesthetic-libraries.md` — genre-by-genre STYLE block presets
- `cover-regen-troubleshooting.md` — failure-mode index for image generation and epubcheck
- `likeness-audit-methodology.md` — how to PASS/BORDERLINE/MISS each chapter
- `canon-audit-methodology.md` — religion/family/timeline canon audit
- `editorial-review-methodology.md` — how a subagent runs the editorial read

## Distinction — references/ vs templates/

- **`references/`** = procedural knowledge (Claude reads, follows). Markdown prose with checklists/rubrics.
- **`templates/`** = parameterized artifacts (user/Claude copies, fills placeholders, commits to the user project). Naming ends `.template`.

## User-project conventions (runtime, not in this repo)

When run against a manuscript, the skill expects/produces: `_launch_config.json`, `_book_config.json`, `ref_images/`, `prompts/chapters/{slug}.txt`, `prompts/Covers/*.txt`, `images/chapters/`, `images/chapters-bw/`, `covers/*.png`+`*.jpg`, `dist/book.epub`, `dist/book-paperback.pdf`, `dist/book-hardcover.pdf`, `dist/*-wrap.pdf`, `dist/visual-companion.pdf`, `.planning/notes/MANUSCRIPT-NOTES.md`, `.planning/notes/IMAGE-AUDIT.md`, `.planning/kdp-listing.md`, `.planning/launch/*.md`.
