# REQUIREMENTS — Upstream PR Milestone

**Goal:** Get `gsd-book-skill` listed as a community-authored row in `gsd-build/get-shit-done` README.md `## Community` table, with an approving review from a maintainer.

**Scope discipline:** Single-row README diff + one changeset fragment. Everything else is in service of making that row land cleanly.

## v1 Requirements

### Repo Polish (PR-01 territory)

- [ ] **POLISH-01** Add `requirements.txt` listing the inferred runtime deps (Pillow, pypdf) plus the implicit `python>=3.10` floor.
- [ ] **POLISH-02** Document external CLI dependencies (`pandoc`, headless Chrome/Chromium, ImageMagick 7+ `magick` binary) in README under a clear "Requirements" or "Install" section.
- [ ] **POLISH-03** Populate `examples/` with a small, runnable fixture — public-domain text + a minimal `book_config.json` + `launch_config.json` that an outside reader can use to dry-run the pipeline. No real API calls required from the fixture path.
- [ ] **POLISH-04** Replace the placeholder `HTTP-Referer: github.com/local/kdp-book-launch` in all 3 OpenRouter callers (`skill/scripts/generate_*.py`) with the real public repo URL.
- [ ] **POLISH-05** Remove committed `skill/scripts/__pycache__/` and add it to `.gitignore` if not already covered.

### PR Material Audit (PR-01 territory — research findings)

- [ ] **AUDIT-01** Insert `## Enhancement PR` as the FIRST `##` heading in `.planning/gsd-pr/PR_BODY.md`. Verify all 8 required H2 headings exist in the order upstream's `pr-template-policy.cjs` expects: `Enhancement PR`, `Linked Issue`, `What this enhancement improves`, `Before / After`, `How it was implemented`, `Testing`, `Scope confirmation`, `Checklist`.
- [ ] **AUDIT-02** Confirm `PR_TITLE.txt` is `docs(NNNN): list gsd-book-skill in Community table` with `NNNN` clearly marked as a placeholder to be substituted at PR-open time.
- [ ] **AUDIT-03** Re-read `STEP1_DISCUSSION_POST.md` and `PR_BODY.md` for any phrasing that reads as "expand the Community section" or category expansion. Rephrase as "zero maintenance burden, first community-authored skill row" framing if found.
- [ ] **AUDIT-04** Verify `README_DIFF.md` produces a single-row diff against current upstream `main` (no stale line numbers).

### File Enhancement Issue (PR-02 territory)

- [ ] **ISSUE-01** File the Enhancement issue at `https://github.com/gsd-build/get-shit-done/issues/new?template=enhancement.yml` using `STEP1_DISCUSSION_POST.md` verbatim. Tick every pre-submission checkbox (unticked → auto-close).
- [ ] **ISSUE-02** Capture the issue number returned by GitHub; record in `.planning/STATE.md` for later substitution into commit message, PR title, PR body `Closes #N`, and changeset fragment.
- [ ] **ISSUE-03** Block on `approved-enhancement` label. Do NOT branch the upstream fork, do NOT edit upstream files, do NOT open a draft PR to stage work.
- [ ] **ISSUE-04** Set day-14, day-28 (stale label appears), and day-42 (auto-close) watchpoints. At day-14 send a polite Discord nudge if no activity. At day-42 trigger Path-4 fallback (already documented in `.planning/gsd-pr/SUBMISSION_RUNBOOK.md`).

### Open the PR (PR-03 territory)

- [ ] **PR-01** Fork `gsd-build/get-shit-done`. Use the same branch name as upstream's `auto-branch.yml` would create — `docs/<issue#>-list-gsd-book-skill-in-community-table` — for clean comparison.
- [ ] **PR-02** Apply `README_DIFF.md` exactly — one row added to the Community table, nothing else. No translated READMEs, no CI, no incidental formatting.
- [ ] **PR-03** Generate the changeset fragment: `node scripts/changeset/new.cjs --type Added --pr <PR#-or-TBD> --body "**Listed kdp-book-launch in Community table** — first community-authored skill built on GSD. Closes #<issue#>."` Commit alongside the README change.
- [ ] **PR-04** Single conventional commit: `docs(<issue#>): list gsd-book-skill in Community table`. Linked-issue line `Closes #<issue#>` in PR body.
- [ ] **PR-05** Open the PR via the URL parameter `?template=PULL_REQUEST_TEMPLATE/enhancement.md`. NOT draft. PR body must contain all 8 required H2 headings and must NOT contain default-template markers (`'Wrong template'`, `'Every PR must use a typed template'`, `'Select the template that matches your PR'`).
- [ ] **PR-06** If the PR number arrives different from the `--pr` value baked into the changeset, follow up with `chore: pin changeset pr field to #<PR#>` on the same branch.

### Iterate to Approval (PR-04 territory)

- [ ] **ITER-01** Monitor PR for reviewer comments. CodeRabbit pre-merge checks (out-of-scope, security, conventional-title) run automatically; address bot findings first, then human reviewer.
- [ ] **ITER-02** If reviewer asks for translated-README sync, politely cite L130 "one concern per PR" and offer a pre-drafted follow-up issue body for translations.
- [ ] **ITER-03** If reviewer asks for CI, acknowledge, scope-out, defer to a post-merge hardening milestone.
- [ ] **ITER-04** If reviewer asks for any other scope expansion, scope-out + follow-up issue. Hold the single-row diff line.
- [ ] **ITER-05** Capture the approving-review SHA in `.planning/STATE.md`. That's the milestone DoD signal.

## v2 Requirements (deferred)

- Add minimal CI to this repo (lint + `python -m compileall`) — only if a reviewer asks during PR review, or as a follow-up milestone post-merge.
- Cross-platform font handling (Linux/Windows fallbacks beyond hardcoded macOS Georgia paths in 3 scripts).
- Windows Chrome path support in `skill/scripts/build_print_pdf.py`.
- Programmatic consent / likeness / NSFW / copyright guardrails on image generation (currently post-hoc human audit only).
- Cover trim-size validation in `compose_cover_wrap.py`.
- Document the ImageMagick 7+ `magick` binary requirement (was Low-severity in CONCERNS).

## Out of Scope

- **Merging the PR.** Maintainer controls merge cadence; DoD stops at approving review.
- **Code rewrites of `skill/scripts/*.py`** beyond POLISH-04 (HTTP-Referer fix). Every CONCERNS finding except the 4 selected is parked for a separate hardening milestone — don't widen the PR diff.
- **Audiobook prep, Apple Books / Kobo / Smashwords pipelines, Series management, A+ Content generator.** These are future product milestones, not PR-prep.
- **Restructuring the upstream `## Community` table** (3rd column, badges, sub-sections, alphabetical sort). That's a feature-level ask; this PR is enhancement-level.
- **Adding our skill to upstream's translated READMEs** (pt-BR, zh-CN, ja-JP, ko-KR). Cite "one concern per PR"; offer follow-up issue if asked.
- **Force-push history rewrites** during PR review iteration. Use additional commits; docs PRs don't need a tidy history.

## Definition of Done

A maintainer (TÂCHES or designated triager) leaves an **approving review** on the PR. Capture the approval comment SHA in `STATE.md` and close the milestone. Merge timing is the maintainer's decision and out of scope.

## Acceptance Criteria

- Issue filed via the `enhancement.yml` template carries the `approved-enhancement` label.
- PR exists on `gsd-build/get-shit-done`, NOT draft, with diff = `README.md` (1 row added) + `.changeset/<slug>.md` (1 file added) — no other files touched.
- PR body's first H2 is `## Enhancement PR`. All 8 required headings present in order.
- PR title format: `docs(<issue#>): list gsd-book-skill in Community table`.
- PR body contains `Closes #<issue#>`.
- No CI auto-close labels (`needs-approved-label`, `wrong-template`, `draft-not-allowed`) on the PR.
- An approving review from a maintainer-tier account is visible on the PR.

## User Stories

- **As a TÂCHES maintainer reviewing the Community table,** I want to see one new row that says "kdp-book-launch — Claude Code skill for KDP book launches" with a link to a real, maintained public repo, so I can approve the addition in under five minutes without auditing this contributor's code.
- **As a future GSD user landing on the README,** I want to see at least one example of a skill built on GSD (not just a runtime port), so I can imagine building my own.
- **As the gsd-book-skill maintainer,** I want my skill discoverable from the canonical GSD README, so users searching for "Claude Code skills built on GSD" find it through the obvious route.

## Risks

- **Procedural auto-close** (≥70% of closed-unmerged PRs die this way). Mitigated by AUDIT-01..AUDIT-04 + PR-05 explicit checks.
- **First-contributor amplifier** — any template violation triggers close, not warn. Mitigated by exact heading verification.
- **Maintainer responsiveness window** (28-day stale → 42-day close). Mitigated by day-14 Discord nudge + day-42 Path-4 fallback.
- **Reviewer scope-expansion asks** (translations, CI, refactors). Mitigated by pre-drafted follow-up issue bodies + "one concern per PR" reference.
- **Stale upstream HEAD** between recon date and PR-open date. Mitigated by AUDIT-04 (re-verify README diff against current main).

## Dependencies

- Upstream `gsd-build/get-shit-done` repo on GitHub (read + fork access).
- GitHub account capable of opening issues and PRs (current: `shoemoney`, OAuth token in keyring — flagged in earlier session but functional).
- `gh` CLI authed against keyring (verified earlier; works when `GITHUB_TOKEN` env var is unset).
- This repo (`gsd-book-skill`) hosted at a real public URL for HTTP-Referer + Community-row link target. **Confirm before PR-01 starts** whether the repo is already public; if not, that's a prerequisite.

## Traceability

(Populated by `gsd-roadmapper` when ROADMAP.md is generated.)

---
*Last updated: 2026-05-15 after research synthesis*
