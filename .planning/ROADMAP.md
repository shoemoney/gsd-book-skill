# ROADMAP — Upstream PR Milestone

**Milestone:** v1.0 — Land upstream community-table PR
**Total phases:** 4
**Requirements covered:** 26 of 26 (100%)

## Phase Overview

| # | Phase | Goal | Requirements | Success Criteria |
|---|-------|------|--------------|------------------|
| 1 | Repo Hardening & PR-Material Audit | A reviewer clicking through this repo finds a maintained-looking project, and the drafted PR materials pass the upstream template policy on first read. | POLISH-01, POLISH-02, POLISH-03, POLISH-04, POLISH-05, AUDIT-01, AUDIT-02, AUDIT-03, AUDIT-04 | 5 |
| 2 | File Enhancement Issue & Wait for Approval Label | The Enhancement issue is filed verbatim from `STEP1_DISCUSSION_POST.md`, captured in STATE.md, and carries the `approved-enhancement` label before any upstream fork branching happens. | ISSUE-01, ISSUE-02, ISSUE-03, ISSUE-04 | 4 |
| 3 | Open the Upstream PR | A non-draft PR exists on `gsd-build/get-shit-done` with a single-row README diff plus a `.changeset/` fragment, all 8 required H2 headings, and no auto-close labels. | PR-01, PR-02, PR-03, PR-04, PR-05, PR-06 | 6 |
| 4 | Iterate to Approving Review | A maintainer-tier approving review is visible on the PR, with its SHA captured in STATE.md as the milestone DoD signal. | ITER-01, ITER-02, ITER-03, ITER-04, ITER-05 | 4 |

## Phase Details

### Phase 1: Repo Hardening & PR-Material Audit
**Goal:** A reviewer clicking through this repo from a future PR link sees a `requirements.txt`, a working `examples/` fixture, real public-repo URLs (no `local/` placeholders), no stray `__pycache__/`, and the drafted PR materials in `.planning/gsd-pr/` already conform to upstream's `pr-template-policy.cjs` heading order — so nothing visible reads as "abandoned" or "first-time-contributor sloppy".
**Mode:** mvp
**Requirements:** POLISH-01, POLISH-02, POLISH-03, POLISH-04, POLISH-05, AUDIT-01, AUDIT-02, AUDIT-03, AUDIT-04
**Success Criteria:**
1. `pip install -r requirements.txt` works in a clean venv and pulls Pillow + pypdf at the documented `python>=3.10` floor.
2. README has a visible Requirements/Install section naming `pandoc`, headless Chrome/Chromium, and ImageMagick 7+ `magick`; `examples/` contains a public-domain text plus minimal `book_config.json` and `launch_config.json` that an outside reader can dry-run without any API keys.
3. `git grep -n "github.com/local/kdp-book-launch"` returns zero hits across all 3 OpenRouter callers, and `skill/scripts/__pycache__/` is absent from the working tree and covered by `.gitignore`.
4. `.planning/gsd-pr/PR_BODY.md` opens with `## Enhancement PR` as the first H2 and contains all 8 required headings in order (`Enhancement PR`, `Linked Issue`, `What this enhancement improves`, `Before / After`, `How it was implemented`, `Testing`, `Scope confirmation`, `Checklist`); `PR_TITLE.txt` reads `docs(NNNN): list gsd-book-skill in Community table` with `NNNN` clearly marked as a substitution placeholder.
5. `README_DIFF.md` applies cleanly as a single-row addition against current upstream `main` HEAD with no stale line numbers, and neither `STEP1_DISCUSSION_POST.md` nor `PR_BODY.md` contains any phrasing that reads as "expand the Community section" or category-expansion framing.
**Dependencies:** None

### Phase 2: File Enhancement Issue & Wait for Approval Label
**Goal:** A real Enhancement issue exists on `gsd-build/get-shit-done` with every pre-submission checkbox ticked, its issue number is recorded in `STATE.md` for downstream substitution, and a maintainer has applied the `approved-enhancement` label — gating Phase 3 cleanly without any premature upstream-fork activity.
**Mode:** mvp
**Requirements:** ISSUE-01, ISSUE-02, ISSUE-03, ISSUE-04
**Success Criteria:**
1. An issue filed via `https://github.com/gsd-build/get-shit-done/issues/new?template=enhancement.yml` is live, pasted verbatim from `STEP1_DISCUSSION_POST.md`, with every pre-submission checkbox ticked (no `needs-approved-label`-style auto-close triggered).
2. The returned issue number is written into `.planning/STATE.md` so all downstream `<issue#>` substitutions (commit message, PR title, `Closes #N`, changeset fragment) resolve from a single source.
3. The `approved-enhancement` label is visible on the issue, and no upstream-fork branch, upstream-file edit, or draft PR has been created during the wait.
4. Day-14 (Discord nudge), day-28 (stale label expected), and day-42 (auto-close → Path-4 fallback) watchpoints are recorded as operational notes in STATE.md, with the Path-4 fallback path from `SUBMISSION_RUNBOOK.md` ready to trigger if day-42 hits.
**Dependencies:** Phase 1

### Phase 3: Open the Upstream PR
**Goal:** A non-draft PR exists on `gsd-build/get-shit-done` from a fork branch named the way `auto-branch.yml` would create it, carrying a single-row `README.md` diff plus one `.changeset/` fragment, a conventional commit linked to the approved issue, and a PR body that passes `pr-template-policy.cjs` on first evaluation — no auto-close labels.
**Mode:** mvp
**Requirements:** PR-01, PR-02, PR-03, PR-04, PR-05, PR-06
**Success Criteria:**
1. `gsd-build/get-shit-done` is forked under the contributor account with a branch named `docs/<issue#>-list-gsd-book-skill-in-community-table` (matching upstream's `auto-branch.yml` convention) and the PR diff is exactly `README.md` (1 row added to the Community table) + `.changeset/<slug>.md` (1 file added) — nothing else.
2. A single conventional commit `docs(<issue#>): list gsd-book-skill in Community table` is pushed, with the changeset fragment generated via `node scripts/changeset/new.cjs --type Added --pr <PR#-or-TBD> --body "..."` committed alongside the README change.
3. The PR is opened via `?template=PULL_REQUEST_TEMPLATE/enhancement.md`, is NOT draft, contains `Closes #<issue#>` in the body, contains all 8 required H2 headings starting with `## Enhancement PR`, and contains none of the default-template scolding markers (`'Wrong template'`, `'Every PR must use a typed template'`, `'Select the template that matches your PR'`).
4. No CI auto-close labels (`needs-approved-label`, `wrong-template`, `draft-not-allowed`) appear on the PR; if the assigned PR number differs from the `--pr` value baked into the changeset, a follow-up commit `chore: pin changeset pr field to #<PR#>` lands on the same branch to reconcile.
**Dependencies:** Phase 2

### Phase 4: Iterate to Approving Review
**Goal:** The PR carries an approving review from a maintainer-tier account (TÂCHES or designated triager), with the approval SHA captured in `STATE.md` as the milestone DoD signal — achieved while holding the single-row diff line against any scope-expansion asks.
**Mode:** mvp
**Requirements:** ITER-01, ITER-02, ITER-03, ITER-04, ITER-05
**Success Criteria:**
1. CodeRabbit pre-merge checks (out-of-scope, security, conventional-title) pass with all bot findings addressed before any human reviewer round; no bot-blocking comments remain open.
2. If a reviewer asks for translated-README sync, the response is a polite citation of L130 "one concern per PR" plus a pre-drafted follow-up issue body for translations — and the PR diff stays single-row.
3. If a reviewer asks for CI or any other scope expansion, the response acknowledges, scopes out, and offers a follow-up issue (post-merge hardening milestone for CI), and the PR diff stays single-row.
4. An approving review from a maintainer-tier account is visible on the PR and its SHA is recorded in `.planning/STATE.md` as the captured DoD signal; the milestone is ready to close.
**Dependencies:** Phase 3

## Coverage Validation

All 26 v1 requirements map to exactly one phase. No gaps, no overlaps.

- **Phase 1 (9):** POLISH-01, POLISH-02, POLISH-03, POLISH-04, POLISH-05, AUDIT-01, AUDIT-02, AUDIT-03, AUDIT-04
- **Phase 2 (4):** ISSUE-01, ISSUE-02, ISSUE-03, ISSUE-04
- **Phase 3 (6):** PR-01, PR-02, PR-03, PR-04, PR-05, PR-06
- **Phase 4 (5):** ITER-01, ITER-02, ITER-03, ITER-04, ITER-05

Total: 9 + 4 + 6 + 5 = **26 of 26 (100%)**.
