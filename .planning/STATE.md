# STATE — Upstream PR Milestone

**Project:** gsd-book-skill
**Milestone:** v1.0
**Active phase:** 2 (Discussion-first variant)
**Next action:** Wait for TÂCHES response on Discussion #3576 — do NOT file Enhancement issue until soft yes received
**Created:** 2026-05-15
**Last updated:** 2026-05-15 after opening Discussion #3576

## Progress
- Completed: 1
- Total: 4
- Percent: 25%

## Phase Status
| # | Phase | Status |
|---|-------|--------|
| 1 | Repo Hardening & PR-Material Audit | complete ✅ |
| 2 | File Enhancement Issue & Wait for Approval Label | pending (awaiting user approval to proceed) |
| 3 | Open the Upstream PR | pending |
| 4 | Iterate to Approving Review | pending |

## Notes

- **Strategy change (2026-05-15):** Phase 2 was originally "File Enhancement Issue" per ROADMAP. The prepared materials in `.planning/gsd-pr/STEP1_DISCUSSION_POST.md` were drafted for a Discussion-first path (open Discussion → wait for soft yes from TÂCHES → THEN file Enhancement issue → THEN PR). Adopted the Discussion-first path on user direction. Roadmap is now de facto: Phase 1 ✅ → Phase 2a Discussion (opened #3576) → Phase 2b file Enhancement issue if yes → Phase 3 PR → Phase 4 review iteration.
- **Discussion #3576 (opened 2026-05-15):**
  - URL: https://github.com/gsd-build/get-shit-done/discussions/3576
  - ID: `D_kwDOQojJX84AmbMG` (GraphQL node ID)
  - Category: General
  - Title: "Listing a third-party Claude Code skill in the Community table — welcome?"
  - Body: copy of `.planning/gsd-pr/STEP1_DISCUSSION_POST.md` with backslash-escapes stripped from the diff fence
  - Status: awaiting TÂCHES response
- **Hard serialization:** Phase 2 → 3 → 4 still non-negotiable post-Discussion. The Enhancement issue cannot be filed until a TÂCHES "yes" (or constructive feedback) appears on Discussion #3576. The PR cannot be opened until the Enhancement issue carries `approved-enhancement`. Branching upstream fork early, editing upstream files, or opening a draft PR triggers auto-close.
- **Discussion watchpoints (Phase 2a):**
  - day-5 (2026-05-20): minimum wait before any nudge — STEP1 doc says "Don't bump it for at least 5 business days"
  - day-10 (2026-05-25): polite Discord nudge OK if no response, per STEP1 P.S.
  - day-21+: re-evaluate — Discussions don't have a stale-bot auto-close, but extended silence = soft no
- **Issue number capture:** When the Enhancement issue is filed (Phase 2b after soft yes), record number here. All downstream `<issue#>` substitutions (commit message `docs(<issue#>): ...`, PR title, PR body `Closes #N`, changeset fragment filename and frontmatter) resolve from that single value.
  - `<issue#>` = _TBD (set when Enhancement issue is filed)_
- **PR number capture:** When the PR is opened in Phase 3, record number here. If it differs from the `--pr` value baked into the changeset at generation time, Phase 3 must land a follow-up `chore: pin changeset pr field to #<PR#>` commit.
  - `<PR#>` = _TBD (set in Phase 3)_
- **Approval SHA capture:** Phase 4 DoD = approving review from a maintainer-tier account. Record the approval comment SHA here as the milestone-close signal.
  - `<approval-sha>` = _TBD (set in Phase 4)_
- **DoD reminder:** Milestone closes on approving review of the PR, NOT on merge. Maintainer controls merge cadence; do not couple our timeline to theirs.
