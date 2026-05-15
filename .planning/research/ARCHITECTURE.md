---
generated: 2026-05-15
focus: architecture
---

# ARCHITECTURE — Upstream PR Flow

Target: `gsd-build/get-shit-done` (HEAD `a7f0af2c`, v1.42.1 era). Goal: land a one-row addition to the `## Community` table in `README.md` listing `kdp-book-launch` / `gsd-book-skill` as a community skill built on GSD.

## End-to-end happy path

1. **Contributor: pre-flight repo hygiene.** Make `gsd-book-skill` look maintained. Pre-empt the items in PR-01 (`requirements.txt`, populated `examples/`, real `HTTP-Referer` URL). Artifact: clean public repo at the URL the reviewer will click. Duration: hours. Failure: reviewer clicks through, sees `local/` placeholder URLs or empty examples, closes the Enhancement issue.
2. **Contributor: confirm classification.** Adding one row to an existing table is an Enhancement (not Fix, not Feature). Per `CONTRIBUTING.md:39` — "improves an existing feature… does **not** add new commands, new workflows, or new concepts." Artifact: decision recorded in `.planning/PROJECT.md` Key Decisions. Failure: misclassifying as Feature triggers `approved-feature` gate which is much harder.
3. **Contributor: open Enhancement Issue.** Use `enhancement.yml` template (issue gets auto-labels `enhancement`, `needs-review`). Fill every required textarea: what's being improved (the Community table), current/proposed behavior with the diff, reason+benefit, scope, breaking changes ("None"), alternatives, area = "Documentation". Artifact: GitHub Issue #NNNN. Source body: `.planning/gsd-pr/STEP1_DISCUSSION_POST.md`. Duration: minutes to file. Failure modes: pre-submission checkboxes unchecked → auto-close; vague reason/benefit → maintainer close with "incomplete proposal."
4. **Maintainer: triage & label.** A maintainer (TÂCHES or designated triager) reviews. If approved: applies `approved-enhancement` label and removes `needs-review`. If declined: closes with rationale. Expected duration: see "Maintainer activity patterns" below. Failure: declined (conflicts with project philosophy of keeping table focused on runtime ports + official channels); stale (>2 weeks no activity → polite nudge per Discord norms).
5. **Contributor: branch & code.** **Only after `approved-enhancement` label is visible.** Fork `gsd-build/get-shit-done`. Branch name: `docs/<issue#>-<short-slug>` (e.g., `docs/3540-community-table-kdp-book-launch`). Edit `README.md` lines 239–245 only — add one row to the existing Community table. Artifact: branch on contributor fork with single-file diff. Duration: minutes. Failure: scope creep (touching other files) → reviewer asks to remove or split.
6. **Contributor: add changeset fragment OR `no-changelog`.** README is **not** in the enforced changeset path list (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/` per `CONTRIBUTING.md:148`), so this PR qualifies for `no-changelog`. Decision per recon: include a `.changeset/<issue#>-community-kdp-book-launch.md` fragment of type `Changed` anyway — cheaper than arguing with a reviewer. Generate via `node scripts/changeset/new.cjs --type Changed --pr <future-pr#> --body "**Listed `kdp-book-launch` in Community table** — adds the first 'skill built on GSD' entry. Closes #NNNN."` Note: `--pr` is back-filled after PR opens (see commit `b40bc6bf` chore: "pin changeset pr field to #3515"). Failure: forgetting both fragment and label → `Changeset Required` CI fails; PR auto-closed.
7. **Contributor: commit.** Single conventional commit: `docs(NNNN): add kdp-book-launch to Community table`. Format from `CONTRIBUTING.md:581` + observed git log: `<type>(<issue#>): <subject>`. The PR number is appended by GitHub on merge per maintainer pattern (`(#<pr#>)`).
8. **Contributor: open PR.** Push branch; open PR against `gsd-build/get-shit-done:main`. **Must** use `?template=PULL_REQUEST_TEMPLATE/enhancement.md` (URL-param selection of typed template). **Must** include `Closes #NNNN` in body. **Must NOT** be draft. Artifact: PR #MMMM with body filled per `.planning/gsd-pr/PR_BODY.md`. Duration: minutes. Failure modes: (a) wrong template/default template → auto-close; (b) draft PR → auto-close; (c) missing `Closes #` → auto-close; (d) issue lacks `approved-enhancement` → auto-close.
9. **CI: automated gates run.** Jobs include `Changeset Required` linter (`scripts/changeset/lint.cjs`), `lint-tests`, and the full test matrix (Ubuntu × Node 22, 24; macOS × Node 24). For a README-only diff, only `Changeset Required` is materially in play; tests pass trivially. Duration: minutes. Failure: any red job → reviewer will not approve.
10. **Reviewer: human review.** Per `CONTRIBUTING.md:568-575`, reviewer builds locally, runs `npm test`, validates implementation matches the linked issue, confirms scope. For a one-row docs change this is fast. Possible asks: minor copy tweaks, anchor-link/badge cleanup, capitalization. Duration: days, sometimes single-cycle for trivial docs.
11. **Contributor: address feedback (if any).** Push additional commits on the same branch. Re-request review. No force-push needed for docs. Failure: scope creep in review responses → split into follow-up issue.
12. **Reviewer: approving review.** **This is our Definition of Done per `.planning/PROJECT.md` line 29** — bar is approval, not merge.
13. **Maintainer: merge.** Maintainer chooses merge timing/method (squash merge observed). Final commit lands with `(#MMMM)` suffix on master. PR closes; linked issue auto-closes via `Closes #NNNN` keyword. CHANGELOG fragment gets consolidated at the next release-notes pass.

## Required artifacts at each step

| Step | Artifact | Template path | Notes |
|---|---|---|---|
| 1 | Clean public repo | — | Reviewer will visit; no broken links, no `local/` placeholders. |
| 3 | Enhancement Issue | `.github/ISSUE_TEMPLATE/enhancement.yml` | Auto-labels `enhancement, needs-review`. All textarea fields required. |
| 4 | `approved-enhancement` label | — | Maintainer-applied; gate enforced by reviewers (and likely a check). |
| 5 | Fork + branch `docs/<issue#>-<slug>` | — | Matches observed pattern from `git log` merges (e.g., `docs/2935-…`, `docs/readme-ingest-docs`). |
| 5 | README.md diff (1 row added) | `.planning/gsd-pr/README_DIFF.md` | Targets existing table at lines 239–245. |
| 6 | `.changeset/<issue#>-…md` | `.changeset/README.md` format | Type `Changed`, body single sentence + `Closes #NNNN`. Or apply `no-changelog` label. |
| 7 | Conventional commit | `CONTRIBUTING.md` line 581 | `docs(<issue#>): subject` — issue # not PR # at commit time. |
| 8 | PR with enhancement template | `.github/PULL_REQUEST_TEMPLATE/enhancement.md` | Open via `?template=PULL_REQUEST_TEMPLATE/enhancement.md` URL param. |
| 8 | PR body | `.planning/gsd-pr/PR_BODY.md` | Fills Before/After, scope confirmation, platforms = N/A, runtimes = N/A. |

## Approval gates and bypasses

**Blocks:**
- Missing `approved-enhancement` label on linked issue → PR auto-closed (`CONTRIBUTING.md:41,125`).
- Draft PR → auto-closed.
- Wrong / default PR template → rejection reason.
- No `Closes #NNN` / `Fixes #NNN` / `Resolves #NNN` keyword → CI fails, PR auto-closed.
- `Changeset Required` workflow fails (no fragment AND no `no-changelog` label) on enforced paths. README is not in the enforced list, but the safe play is to include a fragment.

**Does not block (for our PR specifically):**
- CI test matrix — a README-only diff trivially passes.
- `lint-tests` — no test files touched.
- ADR/PRD numbering rules — N/A, no ADR/PRD created.

**Label authority:** Only maintainers (and trusted triagers) can apply `approved-enhancement`. The `enhancement` and `needs-review` labels are template-applied automatically. The `no-changelog` label is contributor-requestable but applied by a maintainer.

## Branch / commit conventions

**Branch pattern (verified against `git log` merges):**
- `<type>/<issue#>-<short-slug>` is canonical: `fix/3517-phase-complete-leaves-state-md-…`, `feat/2937-enhancement-statusline-…`.
- For docs the slug can omit the issue number when the change is house-keeping (`docs/readme-ingest-docs`, `docs/v1.42.1-release-update`), but those branches are on the maintainer org (`gsd-build/`). External contributors should keep the issue number for traceability.
- **Our branch:** `docs/<issue#>-community-kdp-book-launch`.

**Commit message pattern (`CONTRIBUTING.md:581` + observed):**
```
<type>(<issue#>): <subject>            ← at commit time
<type>(<issue#>): <subject> (#<pr#>)   ← after merge, maintainer-appended
```
Verified real examples from `git log`:
- `docs(3524): CJS↔SDK hard-seam ADR + phased PRD (#3529)`
- `docs(#2935): refresh README highlights for v1.39.0 across all languages (#2936)` ← **closest precedent: docs-only README PR**
- `fix(3537): route every phase-number ROADMAP regex through phaseMarkdownRegexSource (#3538)`

**Our commit (pre-merge form):** `docs(NNNN): add kdp-book-launch to Community table`

## Changeset workflow

Per `.changeset/README.md` and `CONTRIBUTING.md:135-150`:

1. Each PR with user-facing changes drops `.changeset/<random-or-named>.md`. Filename is unique → no merge conflicts when concurrent PRs both add CHANGELOG entries.
2. Frontmatter: `type: <Added|Changed|Deprecated|Removed|Fixed|Security>` (Keep a Changelog), `pr: <number>`. Body is a single sentence describing the user-visible change.
3. Generator: `node scripts/changeset/new.cjs --type <T> --pr <N> --body "..."`. The `--pr` field is often back-pinned in a follow-up `chore: pin changeset pr field to #NNNN` commit (e.g., `b40bc6bf`, `0f7b018d`).
4. `Changeset Required` CI (`scripts/changeset/lint.cjs`) enforces only against paths `bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/`. **README is not in this list.**
5. **For our PR:** include the fragment anyway (low cost, signals user-facing impact properly). Type `Changed`. If a reviewer says "skip," delete the fragment and apply `no-changelog` instead — pure label flip.

Example fragment from repo (`.changeset/2937-statusline-context-position.md`):
```md
---
type: Added
pr: 3515
---
**Context-window meter position is now configurable via `statusline.context_position`** — ... Closes #2937.
```

## Maintainer activity patterns

Based on real merge timestamps in this clone:

- **High velocity in v1.42.x cycle (May 2026).** Multiple merges per day (e.g., 2026-05-05: 14:17, 14:25, 14:46, 19:59 — four merges in one afternoon).
- **Docs-only README PR precedent (#2936 = closest analog to ours):** issue #2935 → commit `7e9477bb` 2026-04-30 23:21 with `(#2936)` suffix. Issue and PR resolved same day. Indicates docs-only enhancements can move fast once approved.
- **Docs README PR #2437 (`/gsd-ingest-docs` to Brownfield commands):** branch `docs/readme-ingest-docs`, merged 2026-04-19 15:39. Commit `4cbebfe7` `docs(readme): add /gsd-ingest-docs to Brownfield commands` — also same-day cadence for a maintainer-internal docs change.
- **External-contributor docs example:** PR #1369 (`noahrasheta/docs/fix-org-references`), PR #1586 (`marcfargas/fix/dev-install-docs`) — external forks land via the same flow.
- **Reasonable expectation for our PR:** if `approved-enhancement` is granted, merge can land within hours-to-a-week. The harder gate is *getting the label applied at all* — `.planning/gsd-pr-recon.md:90` notes TÂCHES is "solo-dev focused and intentionally lean" so a community-listing row may receive a thoughtful "no" rather than fast approval.
- **No-activity threshold:** `.planning/PROJECT.md:55` set 2 weeks before a polite nudge. Backed by Discord-community norm, not codified in `CONTRIBUTING.md`.

## Specific to gsd-book-skill PR

**Where we are right now (2026-05-15):**
- Step 1 complete-ish: pre-empt hardening is requirement PR-01 in `.planning/PROJECT.md` — Active, not Validated. **Must finish before Step 3.**
- Steps 2 (classification = Enhancement) and pre-drafted artifacts done. `STEP1_DISCUSSION_POST.md`, `PR_BODY.md`, `PR_TITLE.txt`, `README_DIFF.md` staged in `.planning/gsd-pr/`.
- Conformance audit complete (`.planning/gsd-conformance-notes.md`): `skill/SKILL.md` names the GSD loop commands.

**Next concrete action — finish Step 1, then execute Step 3:**
1. Complete PR-01 (hardening triplet: `requirements.txt`, populated `examples/` fixture, real `HTTP-Referer` URL in all three OpenRouter callers).
2. Push hardening to public repo so reviewer-clickthrough is clean.
3. File Enhancement issue using `STEP1_DISCUSSION_POST.md` body via `https://github.com/gsd-build/get-shit-done/issues/new?template=enhancement.yml`. Capture the assigned issue number (`NNNN`).
4. **STOP. Wait for `approved-enhancement` label.** Do not branch, do not edit. PR-02 explicitly gates here.
5. Once labeled: create branch `docs/<NNNN>-community-kdp-book-launch` on a fork of `gsd-build/get-shit-done`, apply `README_DIFF.md`, add `.changeset/<NNNN>-community-kdp-book-launch.md` (type `Changed`), commit as `docs(<NNNN>): add kdp-book-launch to Community table`.
6. Open PR via the enhancement template URL param; paste `PR_BODY.md`; ensure `Closes #<NNNN>` is in the body; not a draft.
7. Iterate on review until approval. Bar = approving review (PROJECT.md line 29), not merge.

**Risk register specific to us:**
- Maintainer declines the Enhancement (philosophy: keep table focused) → fall back to Path 4 (independent OSS).
- Maintainer asks for badge/anchor cleanup in `README_DIFF.md` → in-scope, address in PR.
- Maintainer asks for CI on our repo → out of scope per `.planning/PROJECT.md:26`; politely push back, ship minimal CI only if blocking.
- Stale >2 weeks → polite ping per Discord norms.
