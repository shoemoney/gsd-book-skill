---
phase: 01-repo-hardening-pr-material-audit
status: passed
verified_on: 2026-05-15
requirements_total: 9
requirements_passed: 9
requirements_partial: 0
requirements_failed: 0
---

# Phase 1 Verification — Repo Hardening & PR-Material Audit

**Result:** ✅ PASSED — all 9 requirements verified against acceptance criteria.

## Per-Requirement Verification

### POLISH-01 — requirements.txt + pyproject.toml ✅

- `requirements.txt` exists at repo root with `Pillow>=10.0` and `pypdf>=4.0` (pypdf confirmed imported by `skill/scripts/postprocess.py`).
- `pyproject.toml` exists, parses as valid TOML, has `project.name = "kdp-book-launch"`, `project.requires-python = ">=3.10"`, `project.urls.Homepage = "https://github.com/shoemoney/gsd-book-skill"`, no `[build-system]` block (intentional — not publishing to PyPI in this milestone).
- Commits: `dddf0e8` (requirements.txt), `a6eb5aa` (pyproject.toml).

### POLISH-02 — README Requirements section ✅

- README.md has a `## Requirements` section covering: OS compatibility, Python 3.10+ + pip install, pandoc, headless Chrome/Chromium, ImageMagick 7+, **epubcheck**, **Node.js + npx + kdp-book-generator**, `OPENROUTER_API_KEY`, and reference photos for image-gen likeness.
- Commits: `18c62d5` (initial section), `<fix-commit>` (restored epubcheck/Node.js/OS/reference-photos after subagent regression caught in inline review).

### POLISH-03 — examples/ skeleton ✅

- `examples/README.md` explains the input contract, references `skill/SKILL.md` as the canonical workflow doc, and explicitly states no fixture manuscript is shipped.
- `examples/book_config.json.example` and `examples/launch_config.json.example` exist and parse as valid JSON.
- Commit: `287cc30`.

### POLISH-04 — HTTP-Referer fix ✅

- All 3 OpenRouter callers (`generate_back_cover.py`, `generate_front_cover.py`, `generate_chapter_images.py`) now send `HTTP-Referer: https://github.com/shoemoney/gsd-book-skill`.
- `git grep -F "github.com/local/kdp-book-launch" skill/` returns zero hits.
- All 3 files pass `python -m py_compile`.
- Remaining hits for the placeholder exist only in `.planning/` documentation files that reference it by name — expected, out of scope.
- Commit: `1ec29f2`.

### POLISH-05 — __pycache__ cleanup ✅

- No `__pycache__` paths are tracked in git (`git ls-files | grep __pycache__` returns empty).
- `.gitignore` contains both `__pycache__/` and `*.pyc` patterns.
- Plan was a no-op — repo was already clean.

### AUDIT-01 — PR_BODY.md heading audit ✅

- First H2 is `## Enhancement PR` ✅
- All 8 required H2 headings present in canonical order: `Enhancement PR`, `Linked Issue`, `What this enhancement improves`, `Before / After`, `How it was implemented`, `Testing`, `Scope confirmation`, `Checklist` ✅
- `Closes #NNNN` placeholder present (3 occurrences) ✅
- No default-template scolding markers (`'Wrong template'`, `'Every PR must use a typed template'`, `'Select the template that matches your PR'`) ✅
- Additional headings (`Breaking changes`, `Acknowledgments`) appear AFTER the 8 required headings — does not affect policy match.
- Commit: `d4f4c29`.

### AUDIT-02 — PR_TITLE.txt verification ✅

- File content matches exactly: `docs(NNNN): list gsd-book-skill in Community table` (single line, `NNNN` as literal placeholder).
- No commit (file was already canonical).

### AUDIT-03 — Framing audit ✅

- Zero hits for expansion/marketing anti-patterns (`expand the community`, `powerful`, `complete solution`, `the only skill`) across STEP1_DISCUSSION_POST.md and PR_BODY.md.
- "zero maintenance" / "external project" / "community-authored" framing appears 2x in STEP1_DISCUSSION_POST.md.
- TÂCHES credit preserved 3x in each file.
- Commit: `b03a0c0`.

### AUDIT-04 — README_DIFF.md line-number verification ✅

- Hunk header is now `@@ -243,2 +243,3 @@` (matches current upstream `## Community` table position at line 243).
- Table structure unchanged from recon snapshot (2 columns, 2 existing rows).
- No structural changes that would expand scope.
- Commit: `215def5`.

## Deviations from Plan

1. **01-01 README regression (caught in review).** The 01-01 subagent followed its plan's "MERGE rather than duplicate: replace" directive literally and dropped 4 items from the pre-existing Requirements section: OS compatibility note, epubcheck, Node.js + npx + kdp-book-generator, reference-photos requirement. These were operationally load-bearing (epubcheck for EPUB validation, Node.js for the actual packaging CLI per ARCHITECTURE.md). Restored in a follow-up commit. Lesson for future plans: "merge" should mean "union of old + new content," not "replace with new."

## Goal Achievement Check

**Phase goal (from ROADMAP):** "A reviewer clicking through this repo from a future PR link sees a `requirements.txt`, a working `examples/` skeleton, real public-repo URLs (no `local/` placeholders), no stray `__pycache__/`, and the drafted PR materials in `.planning/gsd-pr/` already conform to upstream's `pr-template-policy.cjs` heading order — so nothing visible reads as 'abandoned' or 'first-time-contributor sloppy'."

✅ Achieved. Each clause maps to a passed requirement above. No remaining cosmetic or procedural risk surface that a click-through reviewer would catch.

## Next Phase

**Phase 2: File Enhancement Issue & Wait for Approval Label** — paused per user directive. Proceeds when user explicitly approves filing the upstream issue.
