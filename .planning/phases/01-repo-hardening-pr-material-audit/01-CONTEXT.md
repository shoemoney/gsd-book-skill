# Phase 1: Repo Hardening & PR-Material Audit — Context

**Gathered:** 2026-05-15
**Status:** Ready for planning
**Mode:** Interactive (autonomous Phase 1 ceremony)

<domain>
## Phase Boundary

A reviewer clicking through this repo from a future PR link sees a `requirements.txt` + `pyproject.toml`, a working `examples/` skeleton, real public-repo URLs (no `local/` placeholders), no stray `__pycache__/`, and the drafted PR materials in `.planning/gsd-pr/` already conform to upstream's `pr-template-policy.cjs` heading order — so nothing visible reads as "abandoned" or "first-time-contributor sloppy".

Phase 1 covers 9 requirements (POLISH-01..05, AUDIT-01..04). It is **local-only work**: no upstream PRs, no GitHub issues, no public-facing changes outside this repo.

</domain>

<decisions>
## Implementation Decisions

### Public Repo URL — LOCKED
**Decision:** `https://github.com/shoemoney/gsd-book-skill`

**Applied to:**
- Git `origin` remote (set during context-gathering: `git remote add origin https://github.com/shoemoney/gsd-book-skill.git`)
- HTTP-Referer header in all 3 OpenRouter callers (POLISH-04)
- Future Community-table row target on `gsd-build/get-shit-done` README
- Any `<repo-url>` placeholders in PR_BODY.md / STEP1_DISCUSSION_POST.md / README

### Packaging — Both Files — LOCKED
**Decision:** Ship `requirements.txt` AND `pyproject.toml`.

**`requirements.txt`** (POLISH-01) — explicit pinned-or-minimum versions for the inferred deps:
```
Pillow>=10.0
pypdf>=4.0
```
With `python_requires>=3.10` floor documented in the README install section and reinforced in `pyproject.toml`.

**`pyproject.toml`** — minimal PEP 621 metadata: name, version, description, authors, license, requires-python, dependencies, project URLs (homepage = the public repo URL). NOT a build-system block (we're not publishing to PyPI as part of this milestone — that's a future hardening item). Keep it lean: ~25–35 lines.

**Rationale:** Reviewer doing a `pip install -r requirements.txt` smoke test sees the obvious file. A modern reviewer checking for `pyproject.toml` also finds it. Both are cheap; eliminating either creates an unforced "why didn't you ship X?" question in review.

### examples/ — Skeleton + README, No Fixture Text — LOCKED
**Decision:** `examples/` directory contains:
- `README.md` — explains "drop your manuscript and per-book launch config here" with the expected layout (`chapters/01-*.md`, `book_config.json`, `launch_config.json`) plus pointers to `skill/templates/*.template` for the config skeletons
- `book_config.json.example` — a minimal valid config that points to a placeholder manuscript path, with comments
- `launch_config.json.example` — same, for the launch config
- **NO** committed manuscript text (no Yellow Wallpaper, no lorem ipsum chapters)

**Rationale:** A reviewer doesn't need a runnable demo — they need to see the contract for how the skill consumes input. The README + example configs are sufficient signal that the project knows what its inputs look like. Avoids committing a 6k-word public-domain text that bloats the repo, and avoids implying we ship "Generate a Charlotte Perkins Gilman book" as a feature.

**Out-of-scope deliberately:** No fixture manuscript, no dry-run mode, no smoke-test CI.

### HTTP-Referer Fix Scope — LOCKED
**Decision:** Replace the placeholder `https://github.com/local/kdp-book-launch` in exactly these 3 files:
- `skill/scripts/generate_back_cover.py:108`
- `skill/scripts/generate_front_cover.py:108`
- `skill/scripts/generate_chapter_images.py:80`

With: `https://github.com/shoemoney/gsd-book-skill`

No other behavioral changes to the scripts. Same line number, same string, same `X-Title` header (which already says "kdp-book-launch" — leave it).

### __pycache__ Cleanup — LOCKED
**Decision:**
- `git rm -r --cached skill/scripts/__pycache__/`
- Add `__pycache__/` and `*.pyc` to `.gitignore` (verify existing patterns don't already cover it)

### PR_BODY.md Heading Audit Scope (AUDIT-01) — LOCKED
**Decision:** Insert `## Enhancement PR` as the FIRST H2. Verify the 8 required headings exist in this exact order (from PITFALLS research, `scripts/pr-template-policy.cjs`):

1. `## Enhancement PR`
2. `## Linked Issue`
3. `## What this enhancement improves`
4. `## Before / After`
5. `## How it was implemented`
6. `## Testing`
7. `## Scope confirmation`
8. `## Checklist`

If any heading is missing or out of order, restructure to match — preserving all existing content under reasonable section boundaries. Do not add new content beyond reorganization; AUDIT-03 (framing) handles wording.

### PR_TITLE.txt Verification (AUDIT-02) — LOCKED
**Decision:** Confirm `PR_TITLE.txt` reads exactly:
```
docs(NNNN): list gsd-book-skill in Community table
```
With `NNNN` as the literal placeholder string (substitution happens at PR-open time in Phase 3). If it's currently anything else (e.g., real number, different verb, different phrasing), rewrite to this exact form.

### Framing Audit (AUDIT-03) — LOCKED
**Decision:** Scan `STEP1_DISCUSSION_POST.md` and `PR_BODY.md` for phrases that read as "expand the Community section" / category expansion / marketing. Specifically flag and rewrite:
- "expand the Community table" → "add one row to the existing Community table"
- "first skill" / "a new category" → keep neutral; pitch as "external project that uses GSD as documented, zero maintenance burden"
- "Built on GSD" promotional language → keep TÂCHES credit but frame as factual (the skill uses GSD's command surface), not promotional

Reviewer's mental model: this is a docs-only enhancement that incurs zero maintenance burden on TÂCHES. Anything that reads like a feature pitch hurts approval odds.

### README Diff Verification (AUDIT-04) — LOCKED
**Decision:** Fetch the current upstream `README.md` at `gsd-build/get-shit-done@main` and compare against `.planning/gsd-pr/README_DIFF.md`. Confirm:
- The line numbers in the diff still point at the `## Community` table (currently 239–245 per recon, but main may have advanced)
- The diff applies cleanly (no merge conflicts)
- The diff adds exactly 1 row, no other changes

If line numbers drift, regenerate the diff with current line numbers; do not modify the addition itself.

</decisions>

<code_context>
## Existing Code Insights

Pulled forward from the codebase map and PR-prep notes:

- **Python conventions:** `skill/scripts/*.py` follow a strict pattern — shebang → module docstring → argparse → `add_config_arg(ap)` → `load_config(args.config)` → `main()`. The HTTP-Referer fix should not perturb this structure; just swap the string literal.
- **Config loading is centralized** in `skill/scripts/_config.py`. No script reads OpenRouter URL directly — but each OpenRouter caller has its own hardcoded HTTP-Referer string. Three independent fixes; not a centralization opportunity in this phase.
- **`.gitignore` already exists** at the repo root (per codebase map). Inspect it before adding `__pycache__/` to avoid duplicate patterns.
- **`.planning/gsd-pr/` files are already drafted** with placeholder `NNNN` for the issue number. They live under `.planning/`, which `commit_docs: true` covers, so edits will commit with the rest of phase 1.
- **No tests, no CI** (TESTING.md). AUDIT verifications run as one-shot shell commands in this phase — no test fixtures to add.
- **examples/ directory currently doesn't exist** at all (it was listed as empty in the earlier survey but `ls examples/` shows nothing). POLISH-03 must create the directory before writing files into it.

</code_context>

<specifics>
## Specific Ideas

- **`pyproject.toml` should declare `project.urls.Homepage = "https://github.com/shoemoney/gsd-book-skill"`** so reviewers using uv/pip-audit/etc tooling see the public repo URL.
- **`requirements.txt` should NOT include `pypdf>=4.0` unless it's actually imported.** Per the codebase map, the manifest says "Pillow + pypdf", but if pypdf appears only transitively, drop it. Verify with `git grep -l "import pypdf\|from pypdf"` before locking in.
- **The `examples/README.md` should reference `skill/SKILL.md`** as the canonical workflow doc rather than re-explaining the pipeline.
- **AUDIT-04 should use `gh api repos/gsd-build/get-shit-done/contents/README.md`** (with `GITHUB_TOKEN` unset to use keyring auth) to fetch current upstream README. Stash the response under `.planning/phases/01-*/upstream-README-snapshot.md` for auditability — but don't commit it (add to `.gitignore` if needed; or write under `/tmp`).
- **Atomic commits during execute:** one commit per requirement is fine. The execute-phase workflow handles this naturally if plans are split per-requirement. Plan should consider grouping POLISH-04 (3-file string fix) as one plan, AUDIT-01..04 as one plan ("PR-material polish"), POLISH-03 as its own plan, etc.

</specifics>

<deferred>
## Deferred Ideas

These came up during context-gathering but are NOT in Phase 1 scope. Capture so we don't lose them:

- **PyPI-publishable pyproject.toml** with `[build-system]`, version-bumping automation, GitHub Releases. Future hardening milestone.
- **Dry-run / smoke-test CI** for the book-build pipeline. Future hardening milestone.
- **A committed example manuscript** for hands-on demo. The decision here was "no fixture text" — but if a reviewer asks during Phase 4, this is the obvious response.
- **Centralizing the OpenRouter HTTP-Referer header** through `_config.py`. Cleanup pass for a future hardening milestone; not worth the diff in this PR-prep.
- **A repo-level CHANGELOG.md entry** for this milestone's changes. CHANGELOG.md already exists at repo root (per codebase map); should we add a "v0.x" entry on merge? Defer until Phase 3 — only relevant if the milestone is "released" rather than just merged.
- **Cross-platform font handling** (Linux/Windows fallbacks for hardcoded macOS Georgia paths). Tracked in REQUIREMENTS.md v2.
- **Programmatic image-gen guardrails** (consent / likeness / NSFW). Tracked in REQUIREMENTS.md v2.

</deferred>

---

**Next:** `/gsd-plan-phase 1` — break Phase 1 into atomic plans, one per requirement cluster.
