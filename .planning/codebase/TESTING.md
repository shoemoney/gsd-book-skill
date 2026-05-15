---
generated: 2026-05-15
focus: quality
---

# Testing

## Automated tests

**None.** This repo has no automated test suite.

Confirmed gaps:

- No `tests/` directory anywhere in the repo.
- No `test_*.py` or `*_test.py` files in `skill/scripts/` or elsewhere.
- No `pyproject.toml`, `pytest.ini`, `setup.cfg`, or `tox.ini` — there is no Python package metadata at all, so no test framework, linter, or formatter is pinned.
- No `requirements-dev.txt` / `requirements.txt`; runtime dependencies (`pypdf`, `pandoc`, Chrome) are documented inline in script docstrings (e.g., `skill/scripts/postprocess.py` header, `skill/scripts/build_print_pdf.py` "Required tools" block).
- No pre-commit config (`.pre-commit-config.yaml` absent).

## CI

**None active.** `.github/workflows/` exists as an empty directory — no YAML workflows. No GitHub Actions, no other CI provider config (`.circleci/`, `.gitlab-ci.yml`, etc.) in the tree.

`.github/` contains only the empty `workflows/` directory. There are no issue templates or PR templates either.

## Verification model

Verification is **manual and artifact-driven**, by design. The skill produces visual deliverables (chapter images, cover wraps, PDFs, EPUBs, social pack PNGs) whose correctness can only be assessed by human review. The methodology docs encode that review:

- **`skill/references/likeness-audit-methodology.md`** — manual audit run after the first full chapter-image generation pass. For each generated image the reviewer writes three lines: "Should show protagonist?", "Verdict (PASS / BORDERLINE / MISS)", and a one-line reason. The doc explicitly times the audit ("AFTER the first complete generation pass ... BEFORE you build the PDF / EPUB") and tells the user to fix all misses in one batch rather than chasing individual chapters.
- **`skill/references/canon-audit-methodology.md`** — manual cross-check that the manuscript prose doesn't contradict its own canon (Dramatis Personae, appendix, worldbuilding doc). The process is: extract every factual claim into a list, then grep / re-read the prose for contradictions on ages, dates, denominations, name collisions, etc.
- **`skill/references/editorial-review-methodology.md`** and **`skill/templates/editorial-prompt.md.template`** — scripted editorial pass with HIGH / MEDIUM / LOW finding classification, written to `.planning/notes/MANUSCRIPT-NOTES.md`.
- **`skill/references/phase-checklist.md`** — author-approval gates between each of the 5 phases described in `skill/SKILL.md`. The hard-cost gate is Phase 2 (chapter image API spend); the visual-correctness gates are Phase 2 (likeness audit) and Phase 3 (cover review).
- **`skill/references/cover-regen-troubleshooting.md`** — diagnostic playbook for the most common visual failure modes on cover regeneration.

## Per-script self-checks

Several scripts emit lightweight post-run sanity output to stdout/stderr that the operator is expected to eyeball:

- `skill/scripts/build_book_md.py` prints `H1 count: N` after writing `_build_book.md` — operator checks the count matches expected chapter count + front matter (title + copyright pages). It also emits `WARN: no image for {slug}` and `WARN: header pattern not matched for {slug}` to stderr when chapter-image insertion fails, which is the closest thing to a test assertion in the codebase.
- `skill/scripts/build_print_pdf.py` prints the output PDF path + byte size on success; operator opens the PDF and visually inspects page breaks, image placement, and page numbering.
- `skill/scripts/postprocess.py` patches PDF metadata in place; the user verifies via Preview / Acrobat "Document Properties".

## Example launches

The top-level `examples/` directory exists but is **empty**. The README and `skill/SKILL.md` reference the "Jeremy Christ" production launch as the reference implementation rather than checking in an example project. There is no fixture project a contributor can run end-to-end against to validate changes.

## Gaps worth flagging

- No regression coverage on the regex-heavy `build_book_md.py` (the two-pattern chapter-image insertion logic is the most likely place to silently break on manuscript-format drift).
- No schema validation on `_launch_config.json` — missing keys surface as `KeyError` tracebacks rather than friendly `sys.exit` messages.
- No smoke-test workflow in CI to even confirm `python3 -c "import skill.scripts.<x>"` parses after a change.
- An empty `examples/` directory means contributors can't reproduce the happy path without bringing their own manuscript + reference photos + OpenRouter API key.
