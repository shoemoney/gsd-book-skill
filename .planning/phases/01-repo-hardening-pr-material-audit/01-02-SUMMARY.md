---
plan: 01-02
status: complete
---

# 01-02 Summary: Fix OpenRouter HTTP-Referer placeholder

## What changed
Replaced `https://github.com/local/kdp-book-launch` with `https://github.com/shoemoney/gsd-book-skill`
in the `HTTP-Referer` header of all 3 OpenRouter callers under `skill/scripts/`.

## Files modified
- `skill/scripts/generate_back_cover.py` (line 108)
- `skill/scripts/generate_front_cover.py` (line 108)
- `skill/scripts/generate_chapter_images.py` (line 80)

## Verification
- `python -m py_compile` passes for all 3 files.
- `git grep -F "github.com/local/kdp-book-launch" -- 'skill/'` returns zero hits.
- Remaining repo-wide hits are confined to `.planning/` docs that reference the
  placeholder by name (REQUIREMENTS, ROADMAP, CONCERNS, this plan, CONTEXT) —
  expected and out of scope.

## Requirements satisfied
- POLISH-04

## Commit
`1ec29f2` — fix(01-02): replace local/kdp-book-launch placeholder...
