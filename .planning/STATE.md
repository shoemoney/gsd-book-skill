# STATE — Upstream PR Milestone

**Project:** gsd-book-skill
**Milestone:** v1.0
**Active phase:** 1
**Next action:** /gsd-discuss-phase 1
**Created:** 2026-05-15

## Progress
- Completed: 0
- Total: 4
- Percent: 0%

## Phase Status
| # | Phase | Status |
|---|-------|--------|
| 1 | Repo Hardening & PR-Material Audit | pending |
| 2 | File Enhancement Issue & Wait for Approval Label | pending |
| 3 | Open the Upstream PR | pending |
| 4 | Iterate to Approving Review | pending |

## Notes

- **Hard serialization:** Phase 1 → 2 → 3 → 4 is non-negotiable. Phase 2 cannot start until POLISH/AUDIT items land (reviewer will inspect repo state before approving the issue). Phase 3 cannot start until the `approved-enhancement` label is visible on the filed issue — branching the upstream fork early, editing upstream files, or opening a draft PR will trigger auto-close.
- **Issue number capture:** Phase 2 must write the returned GitHub issue number here before Phase 3 begins. All downstream `<issue#>` substitutions (commit message `docs(<issue#>): ...`, PR title, PR body `Closes #N`, changeset fragment filename and frontmatter) resolve from that single value.
  - `<issue#>` = _TBD (set in Phase 2)_
- **PR number capture:** When the PR is opened in Phase 3, record its number here. If it differs from the `--pr` value baked into the changeset at generation time, Phase 3 must land a follow-up `chore: pin changeset pr field to #<PR#>` commit.
  - `<PR#>` = _TBD (set in Phase 3)_
- **Approval SHA capture:** Phase 4 DoD = approving review from a maintainer-tier account. Record the approval comment SHA here as the milestone-close signal.
  - `<approval-sha>` = _TBD (set in Phase 4)_
- **Stale-bot watchpoints (Phase 2):** day-14 (Discord nudge if no activity), day-28 (stale label expected to appear), day-42 (auto-close — trigger Path-4 fallback per `.planning/gsd-pr/SUBMISSION_RUNBOOK.md`). Set calendar reminders when the issue is filed.
- **DoD reminder:** Milestone closes on approving review, NOT on merge. Maintainer controls merge cadence; do not couple our timeline to theirs.
