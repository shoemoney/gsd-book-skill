# gsd-book-skill — Upstream PR Milestone

## What This Is

`gsd-book-skill` (installed as `kdp-book-launch`) is a Claude Code skill that drives an end-to-end Amazon KDP book launch — editorial review, AI cover/chapter image generation, EPUB + paperback/hardcover PDF builds, and KDP listing prep — through GSD's `.planning/` workflow. This project is the **PR-prep milestone**: get the skill listed as a community plugin in the upstream `gsd-build/get-shit-done` README.

## Core Value

**A reviewer on `gsd-build/get-shit-done` looks at this repo, finds nothing embarrassing, and approves the PR adding the Community row.** Everything else (hardening, examples, future features) is in service of that single decision.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] **PR-01** Pre-empt hardening — `requirements.txt`/`pyproject.toml` so `pip install -r` works, populated `examples/` fixture, real `HTTP-Referer` URL in all 3 OpenRouter callers
- [ ] **PR-02** File Enhancement issue on `gsd-build/get-shit-done` using the drafted `STEP1_DISCUSSION_POST.md`; monitor until `approved-enhancement` label appears
- [ ] **PR-03** Open the PR using the prepared `PR_BODY.md` + `PR_TITLE.txt` + `README_DIFF.md`, with the required `.changeset/` fragment or `no-changelog` label, against current upstream HEAD
- [ ] **PR-04** Iterate on reviewer feedback until the PR carries an approving review

### Out of Scope

- **Adding CI** to this repo — drafted but deliberately deferred; only add if a reviewer asks. Empty `.github/workflows/` is conspicuous but not a blocker for a docs-only README enhancement.
- **Code rewrites of `skill/scripts/*.py`** — every CONCERNS finding except the 3 selected pre-empts is parked. Don't widen the diff a reviewer has to read.
- **Audiobook prep / Apple Books / Series mgmt / A+ Content gen** — future milestones. Not in v1.
- **Merging the PR** — the maintainer controls merge timing. Bar for this milestone is *approving review*, not green merge.
- **Cross-platform font handling, Windows Chrome path, dependency lock for external CLIs** — known concerns; deferred to a hardening milestone post-merge.

## Context

- **Upstream repo:** `gsd-build/get-shit-done` (v1.42.x era; HEAD `a7f0af2c` per recon)
- **Insertion target:** `README.md` line 239–245 — extend the existing `## Community` table with one row, not a new section
- **Existing prior art:** `gsd-opencode` row (a runtime port). This skill is the first "skill built on GSD" entry in that table.
- **Strict governance** (from `CONTRIBUTING.md`):
  - No code before approval. Enhancement issue → `approved-enhancement` label → PR
  - PRs without a typed template or linked approved issue are auto-closed
  - No draft PRs
  - Conventional commit format: `<type>(<issue#>): subject (#<pr#>)`
  - Every PR needs a `.changeset/` fragment OR the `no-changelog` label
- **Conformance audit complete:** `.planning/gsd-conformance-notes.md` — `skill/SKILL.md` already patched to name the GSD loop commands explicitly (`/gsd-discuss-phase`, `/gsd-plan-phase`, `/gsd-execute-phase`, `/gsd-verify-work`, `/gsd-autonomous`)
- **Drafted materials in `.planning/gsd-pr/`:**
  - `STEP1_DISCUSSION_POST.md` — Enhancement-issue body, copy-paste ready
  - `PR_BODY.md`, `PR_TITLE.txt`, `README_DIFF.md` — for the PR step
  - `SUBMISSION_RUNBOOK.md` — operator-level checklist

## Constraints

- **Process:** Enhancement type, not Fix or Feature. Must follow CONTRIBUTING.md sequence verbatim — gates exist and reviewers enforce them.
- **Diff size:** Single-row README addition + a `.changeset/` fragment. Anything outside that scope (CI, deep refactors) widens the PR and slows approval. Strict discipline on Out of Scope.
- **Commit format:** Conventional commits with linked issue number (e.g., `docs(NNNN): add kdp-book-launch to Community table (#NNNN)`).
- **Repository hygiene:** Repo URL in any text the reviewer sees (HTTP-Referer, README, SKILL.md) must point at the real public URL of this repo — no `local/` placeholders.
- **Timing:** No hard deadline, but a stale Enhancement issue with no maintainer activity > 2 weeks may need a polite nudge per Discord-community norms.

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Submit as Enhancement (not Fix or Feature) | Adding a row to an existing table is improving an existing surface, not fixing a bug or shipping a new system. Matches CONTRIBUTING.md taxonomy. | — Pending |
| Pre-empt 3 of 4 hardening items before filing the issue | `requirements.txt`, `examples/` fixture, and real HTTP-Referer URL are the visible polish that makes the repo look maintained when a reviewer clicks through. CI deferred because docs-only PRs don't strictly require it. | — Pending |
| Skip adding CI in this milestone | Empty `.github/workflows/` is a visible flaw but not a blocker; adding CI now widens scope and timeline. Reintroduce only if a reviewer flags it. | — Pending |
| Bar = reviewer approval, not merge | Maintainer controls merge cadence; tying our DoD to merge would couple our timeline to theirs. | — Pending |
| Hardening + issue filing happen in the same project | Reviewer will inspect repo state before approving the issue, so pre-empts must land before the issue is filed. | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-15 after initialization*
