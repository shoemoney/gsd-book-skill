---
generated: 2026-05-15
focus: summary
---

# Research Summary — Upstream GSD PR Milestone

Synthesizes STACK.md (ecosystem), FEATURES.md (reviewer expectations), ARCHITECTURE.md (end-to-end PR flow), and PITFALLS.md (auto-close triggers) against `.planning/PROJECT.md` (PR-01 … PR-04), `gsd-pr-recon.md`, and `gsd-conformance-notes.md`.

## Top 5 Load-Bearing Findings

1. **`PR_BODY.md` is missing the `## Enhancement PR` H2 discriminator heading.** `scripts/pr-template-policy.cjs` resolves the template by matching that exact heading; without it, `matchingTemplate()` returns `null`, the first-time-contributor branch fires `action: 'close'`, and the PR dies in under a minute. Single highest-leverage catch in the entire research pass. — *PITFALLS.md "Specific to gsd-book-skill PR"*
2. **~70% of closed-unmerged PRs die on procedural violations, not technical merit.** Among the 25 most recent closures (May 7–15, 2026): missing typed template, no linked `approved-*` issue, draft state. The bar is procedural conformance, not code quality — which means our risk surface is paperwork, not the skill itself. — *FEATURES.md "Rejection-reason taxonomy"*
3. **`approved-enhancement` is a human-applied gate that strictly precedes the PR.** Maintainer must label the linked issue before any PR is opened; CI auto-closes otherwise. Sequencing is non-negotiable: PR-02 must complete (label visible) before PR-03 starts. — *ARCHITECTURE.md steps 4–5; PITFALLS.md "Hard auto-close triggers"*
4. **The `## Community` table was created by the maintainer in commit `7e9b8dec`, not by a community contributor; both existing rows (gsd-opencode, Discord) are maintainer-added.** Our PR will be the **first community-authored row** in this table. That raises the framing bar: pitch as "external project that uses GSD as documented, zero maintainer maintenance burden," never as category expansion. — *FEATURES.md "Patterns from recently merged PRs"*
5. **Changeset fragment format is exact: filename `<adjective>-<noun>-<noun>.md` (or `<issue#>-<slug>.md`), frontmatter `type: <Added|Changed|Deprecated|Removed|Fixed|Security>` + `pr: <#>`.** README is NOT in the enforced path list, so `no-changelog` is legal — but including a `type: Added` fragment is cheaper than chasing a maintainer-only label. Generator: `node scripts/changeset/new.cjs --type Added --pr <#> --body "..."`. — *STACK.md "Changeset conventions"; ARCHITECTURE.md "Changeset workflow"*

## Decisions These Findings Force

| Decision | Forced by | Effect on phases |
|---|---|---|
| Insert `## Enhancement PR` as the first H2 in `PR_BODY.md` before opening PR | Finding #1 | Mandatory gate in PR-03 prep; verify all 8 required headings present (`Enhancement PR`, `Linked Issue`, `What this enhancement improves`, `Before / After`, `How it was implemented`, `Testing`, `Scope confirmation`, `Checklist`) |
| Treat PR-02 → PR-03 as hard-serialized; do not branch upstream-fork until label is visible | Finding #3 | PR-03 cannot start until PR-02 closes. Build a check step. |
| Ship a `.changeset/<issue#>-community-kdp-book-launch.md` fragment with `type: Added`, not chase `no-changelog` label | Finding #5 | Adds one file to the PR-03 diff; eliminates one approval round-trip |
| Frame issue + PR body as "zero maintenance burden, first community-authored skill row" — never as "expand the Community section" | Finding #4 | Re-read `STEP1_DISCUSSION_POST.md` and `PR_BODY.md` for any phrasing that reads as category expansion or marketing |
| Pre-empt hardening (PR-01) must complete BEFORE the Enhancement issue is filed | Finding #2 (procedural rigor extends to repo click-through) | Locks PR-01 → PR-02 ordering even though they touch different repos |
| Substitute `NNNN` placeholders in `PR_TITLE.txt` and `PR_BODY.md` with the real issue number at PR-open time | Finding #1/#2 mechanical correctness | Add to PR-03 checklist |

## Confirmed: Things We Were Already Right About

- **Enhancement classification (not Fix, not Feature).** Adding a row to an existing table is improving an existing surface. CONTRIBUTING.md taxonomy matches PROJECT.md Key Decisions. — *recon §2; ARCHITECTURE.md step 2*
- **Single-row diff discipline; no new section, no Description column, no badge swap.** Restructuring the table is feature-level. — *FEATURES.md "Anti-features"; PROJECT.md Constraints*
- **Skill name `kdp-book-launch` conforms to `^[a-z0-9]+(-[a-z0-9]+)*$` and does NOT use the `gsd-` prefix.** Avoids impersonating a core command. — *STACK.md SKILL.md frontmatter; conformance audit row 6*
- **CI deferral is defensible for a docs-only README enhancement.** Empty `.github/workflows/` is visible but not blocking. — *STACK.md "CI workflows: NICE"; PROJECT.md Out of Scope*
- **DoD = reviewer approval, not merge.** Maintainer controls cadence; decoupling our timeline is correct. — *ARCHITECTURE.md step 12; PROJECT.md Key Decisions*
- **TÂCHES attribution + Acknowledgments section in both README and SKILL.md** matches the gsd-opencode precedent. — *STACK.md "Community precedent"; conformance audit row 5*
- **The 6 conformance-audit checks (`.planning/`, GSD loop commands, no command collisions, clean install, TÂCHES credit, non-impersonation) all PASS post-patch.** — *gsd-conformance-notes.md*

## Surprises: Things Research Surfaced That We Hadn't Documented

1. **`TRUSTED_AUTHOR_ASSOCIATIONS` excludes `FIRST_TIME_CONTRIBUTOR`** — first-time contributors get `action: 'close'` for any template-policy violation, while repeat contributors get `action: 'warn'`. SEVERITY: HIGH. Mitigation: heading-by-heading verification of PR_BODY.md, no "close enough" tolerance.
2. **Stale-bot timing: `days-before-stale: 28`, `days-before-close: 14` (42 days total).** Our PROJECT.md only documents a "2-week nudge" Discord norm. SEVERITY: MEDIUM. Add explicit day-28 (stale label appears) and day-42 (auto-close) checkpoints to the runbook; Path-4 fallback must be ready by day 42.
3. **CodeRabbit out-of-scope/security/title pre-merge checks remain ON** (only `docstrings` + `eslint` disabled). Title must be conventional. SEVERITY: LOW. Our planned `docs(NNNN): list gsd-book-skill in Community table` is conventional and in-scope.
4. **`auto-branch.yml` reacts to the `enhancement` label (NOT `approved-enhancement`) by creating `feat/<issue#>-<slug>` upstream**; for `area: docs` it creates `docs/<issue#>-<slug>`. So an upstream branch may exist before we fork. SEVERITY: LOW. Use the same branch name on our fork; comparison still resolves.
5. **`dismiss-unauthorized-pr-approvals.yml` runs every 15 minutes** and dismisses non-collaborator approvals. SEVERITY: LOW. A community "looks good!" reaction doesn't count; only TÂCHES/triagers can approve. Don't solicit community drive-by approvals.
6. **Five localized READMEs exist** (`README.md` + pt-BR / zh-CN / ja-JP / ko-KR). A reviewer may either ask to sync all five or invoke "one concern per PR." SEVERITY: MEDIUM. Pre-decide: default response is "I'll file a follow-up issue for translations" — cite L130.
7. **gsd-opencode does NOT use a plugin manifest** (no `marketplace.json` / `plugin.json` / `SKILL.md` at repo root — it's a runtime port). Reinforces that no manifest file is required. SEVERITY: INFO. Validates our minimal SKILL.md-only approach.

## Pre-Empt Adjustments

**Move from "Out of Scope" to Active — none recommended.** CI remains correctly deferred; reviewer may still flag it but the cost of pre-emptively adding it (broader diff, more maintenance-signal questions) outweighs the benefit for a docs-only README PR.

**Re-scope of Active items:**
- **PR-01** should add a 4th deliverable: **verify all 8 required H2 headings exist in `PR_BODY.md`** with `## Enhancement PR` as the first. This is the single highest-leverage catch from research and belongs in pre-empt hardening, not deferred to PR-03 execution.
- **PR-03** should explicitly include the `.changeset/<issue#>-community-kdp-book-launch.md` fragment as a deliverable artifact (currently phrased as "fragment OR label"). Choose the fragment.
- **PR-04** should add an explicit day-28 / day-42 stale-bot watchpoint, not just the 2-week Discord nudge.

**New Active candidate (optional):** Pre-draft a "translations follow-up issue body" so that if a reviewer asks during PR-03/PR-04 review, the response is a one-comment paste rather than a round-trip. Low cost, hedges the localized-README risk surfaced in surprise #6.

## Phase Implications

**PR-01 — Pre-empt hardening (repo polish + PR_BODY heading audit):**
- Ship `requirements.txt`/`pyproject.toml`, populated `examples/` fixture, real `HTTP-Referer` URL in all 3 OpenRouter callers (already scoped).
- NEW: Insert `## Enhancement PR` as the first H2 in `.planning/gsd-pr/PR_BODY.md` and verify all 8 required headings present in order.
- NEW: Confirm `PR_TITLE.txt` placeholder `NNNN` substitution is a documented step (not a typo waiting to happen).

**PR-02 — File Enhancement issue and wait for label:**
- Use `enhancement.yml` template via `https://github.com/gsd-build/get-shit-done/issues/new?template=enhancement.yml`; paste `STEP1_DISCUSSION_POST.md` verbatim.
- Verify pre-submission checkboxes are all ticked (unticked → auto-close).
- HARD STOP until `approved-enhancement` label is visible. Do NOT branch, do NOT edit upstream fork, do NOT open a draft PR to "stage" the work — drafts auto-close.
- Set day-14 (Discord nudge), day-28 (stale label), day-42 (auto-close → Path-4 fallback) reminders.

**PR-03 — Open the PR:**
- Open via `?template=PULL_REQUEST_TEMPLATE/enhancement.md` URL parameter; do NOT use the default scolding template.
- Single conventional commit: `docs(<issue#>): list gsd-book-skill in Community table`.
- Diff = 1 row in `README.md` + 1 file in `.changeset/`. No translated READMEs, no CI, no drive-by formatting.
- PR body must include `Closes #<issue#>`, must contain all 8 enhancement headings starting with `## Enhancement PR`, must not be draft, must not contain default-template markers (`'Wrong template'`, `'Every PR must use a typed template'`, `'Select the template that matches your PR'`).
- Changeset fragment: `node scripts/changeset/new.cjs --type Added --pr <future-pr#> --body "**Listed kdp-book-launch in Community table** — first community-authored skill built on GSD. Closes #<issue#>."` Back-pin the `--pr` field via follow-up `chore: pin changeset pr field to #<#>` commit if needed.

**PR-04 — Iterate to approving review:**
- Address any reviewer asks in additional commits on the same branch; no force-push needed for docs.
- If reviewer asks for localized-README sync: politely cite L130 "one concern per PR" and offer a follow-up issue (pre-drafted per Pre-Empt Adjustments).
- If reviewer asks for CI: acknowledge, scope out, defer to post-merge hardening milestone.
- DoD = approving review on the PR (not merge). Capture the approval comment SHA in `STATE.md` and close the milestone.
