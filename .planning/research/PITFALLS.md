---
generated: 2026-05-15
focus: pitfalls
---

# PITFALLS — Things That Sink Community PRs

Primary-source citations are to the upstream clone at `~/Projects/gsd-upstream/get-shit-done/` (HEAD `a7f0af2c`, v1.42.1 era). Workflow files are the auto-close logic — not maintainer judgment calls.

## Hard auto-close triggers (instant death)

- **Missing `Closes|Fixes|Resolves #NNN` in PR body** — `.github/workflows/require-issue-link.yml` greps the body with `grep -qiE '(closes|fixes|resolves)\s+#[0-9]+'`. No match → bot comments, calls `pulls.update({ state: 'closed' })`, sets job to failed. Auto-reopens on body edit + manual reopen. Prevention: PR body MUST contain `Closes #<approved-issue-number>`. Phase: **PR-03** (issue number must be substituted into `PR_BODY.md` before opening).

- **Linked issue lacks `approved-enhancement` label** — `CONTRIBUTING.md` L41, L45: *"a PR for an enhancement will be closed without review if the linked issue does not carry the `approved-enhancement` label."* Enforced by maintainer, not CI, but reviewers act fast. Prevention: do not open PR until label is visible on the issue. Phase: **PR-02** (gates PR-03).

- **PR body does not match a typed template** — `scripts/pr-template-policy.cjs` parses headings (case-insensitive, decoration-stripped) and requires every entry in `requiredHeadings` for the matched template. For Enhancement, all 8 must be present: `Enhancement PR`, `Linked Issue`, `What this enhancement improves`, `Before / After`, `How it was implemented`, `Testing`, `Scope confirmation`, `Checklist`. Non-trusted authors (first-timers — see `TRUSTED_AUTHOR_ASSOCIATIONS` set) get `action: 'close'`. Prevention: heading-by-heading verification of `PR_BODY.md`. Phase: **PR-01/PR-03**.

- **Draft PR** — `.github/workflows/close-draft-prs.yml` checks `pull_request.draft === true` on open/reopen/converted_to_draft and immediately closes. Prevention: never mark draft; complete work on local branch first. Phase: **PR-03**.

- **Using the default `pull_request_template.md`** — same `pr-template-policy.cjs` check; `DEFAULT_TEMPLATE_MARKERS` array catches the scolding-template phrases (`'Wrong template'`, `'Every PR must use a typed template'`, `'Select the template that matches your PR'`). If any appear in body → close. Prevention: load the PR with `?template=enhancement.md` query string or replace body wholesale. Phase: **PR-03**.

- **Empty PR body** — `pr-template-policy.cjs` line ~140: `if (!normalizedBody) { valid = false; ... }` → close. Prevention: don't open PR before pasting body. Phase: **PR-03**.

- **Wrong author email pattern (local guard, optional)** — `.githooks/pre-push` example in CONTRIBUTING.md L502–533 blocks commits whose author email matches `$GSD_BLOCKED_AUTHOR_REGEX`. Only fires on machines that have configured the hook — not enforced server-side. Not a realistic risk for an external contributor.

## Soft pitfalls (slow approval / extra review rounds)

- **Missing `.changeset/*.md` fragment OR `no-changelog` label** — `.github/workflows/changeset-required.yml` runs `scripts/changeset/lint.cjs` on every PR. README isn't in the enforced paths per `gsd-pr-recon.md` L80 (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/`), so a README-only PR likely passes — but the `no-changelog` label is the explicit opt-out and the safer move. Mention it in a PR comment if you can't apply labels.

- **PR size `size/L` or `size/XL`** — `.github/workflows/pr-gate.yml` counts additions+deletions and stamps a `size/` label; >500 lines logs `core.warning("Large PR")`. A one-row README change is trivially `size/S`. Prevention is already locked: single-row diff discipline (PROJECT.md "Constraints").

- **Non-conventional commit subject / wrong issue prefix** — commit log convention `<type>(<issue#>): subject (#<pr#>)`. Not workflow-enforced but is a visible reviewer expectation; the merging maintainer may squash with their own formatted message. Branch-naming workflow (`branch-naming.yml`) accepts `docs/`, `feat/`, `fix/`, `chore/`, etc. as warning-only — not auto-close. `docs/<issue#>-<slug>` is fine.

- **Stale-bot timing** — `.github/workflows/stale.yml`: `days-before-stale: 28`, `days-before-close: 14`. Issues AND PRs idle 42 days are auto-closed. Discussions can drag past this; if the enhancement issue sits with no `approved-enhancement` label for >28 days, the `stale` label appears and the clock starts.

- **CodeRabbit out-of-scope / security / title scans** — `.coderabbit.yaml` keeps defaults for `out-of-scope`, `security`, and `title` pre-merge checks; only `docstrings` and `eslint` are disabled. A README-only PR with a conventional title and an approved-enhancement scope should pass cleanly. Title must be conventional (`docs(NNNN): ...`).

- **No localized-README updates** — `README.md`, `README.pt-BR.md`, `README.zh-CN.md`, `README.ja-JP.md`, `README.ko-KR.md` all exist upstream. Reviewer may ask to sync the new row across all five OR may invoke "one concern per PR" (CONTRIBUTING.md L130) and ask for a follow-up. Either path adds a round trip.

- **Drive-by formatting / scope creep** — CONTRIBUTING.md L131 *"No drive-by formatting"* and L130 *"One concern per PR"*. Tempting if a tidy-up itch shows up while in the codebase. Don't.

## GSD-specific pitfalls (not generic OSS advice)

- **`approved-enhancement` is a human gate, not a bot one.** The label is applied manually by TÂCHES/maintainers. Auto-branch workflow (`.github/workflows/auto-branch.yml`) reacts to the `enhancement` label by creating a `feat/<issue#>-<slug>` branch in upstream — that's a different label and shouldn't be confused with `approved-enhancement`. For `area: docs` label, the auto-branch is `docs/<issue#>-<slug>`. If the maintainer applies `area: docs` first, the canonical branch already exists upstream; if you push to your fork's `docs/<issue#>-<slug>` the comparison still works fine.

- **Default `pull_request_template.md` is a trap.** Per `gsd-pr-recon.md` L39: it explicitly says *"Using this default template is a reason for rejection."* The "doc-only fixes with no linked issue" carve-out at the bottom is a credibility test; a Community-table addition is too close to promotional content to safely invoke it. Treat as Enhancement, not doc-only.

- **CONTRIBUTING.md L94 "No code before approval" applies even to one-line README changes.** The Issue-First Rule is exception-less. Forking + branching + committing locally is fine; *opening the PR* is the gated step. PROJECT.md sequencing already respects this (Active items PR-02 → PR-03).

- **CONTEXT.md / ADR vocabulary expectations (CONTRIBUTING.md L117–123).** Doesn't apply to our PR — we're not touching code or naming new modules — but if the reviewer asks for a one-line description change in the platform column, stay inside the existing vocabulary ("skill", "workflow", "loop") and avoid coining new terms.

- **Solo-maintainer maintenance-burden filter.** CONTRIBUTING.md L55 frames every Feature decision through "permanent maintenance burden to a solo-developer tool." Our pitch (PR_BODY.md, STEP1_DISCUSSION_POST.md) already emphasizes zero ongoing maintenance and one row — that framing is doing real work.

## First-contributor amplifiers

- **`TRUSTED_AUTHOR_ASSOCIATIONS` excludes `FIRST_TIME_CONTRIBUTOR`.** `scripts/pr-template-policy.cjs` line 3: only `CONTRIBUTOR`, `COLLABORATOR`, `MEMBER`, `OWNER` get `action: 'warn'` on a malformed body. Everyone else gets `action: 'close'`. A repeat contributor with a typo in headings gets a comment asking to fix; a first-timer gets the PR auto-closed.

- **`dismiss-unauthorized-pr-approvals.yml`** runs every 15 minutes and dismisses approvals from non-collaborators. Doesn't affect a first-timer's *own* PR, but a friendly "looks good!" review from another community member won't count.

- **No prior commits in `git log`** → reviewers default-skeptical. Mitigated by the Discussion-first step (STEP1) — you arrive with context and credibility before any code.

- **Stale-bot exempt labels** (`fix-pending`, `priority: critical`, `pinned`, `confirmed-bug`, `confirmed`, `awaiting-retest`, `needs-reproduction`, `DO NOT MERGE`) are all maintainer-applied. A first-time contributor has no path to keep a slow-moving thread alive other than a polite comment before day 28.

## Specific to gsd-book-skill PR

**Already mitigated by PROJECT.md decisions:**
- Cold-PR auto-close → `SUBMISSION_RUNBOOK.md` Step 1 (Discussion-first) and PR-02 (issue → label → PR) eliminate it.
- Embarrassing repo on click-through → PR-01 pre-empts the 3 visible CONCERNS items (`requirements.txt`, real `HTTP-Referer`, `examples/` fixture).
- Conventional commit + branch naming → recon documents the exact pattern; runbook Step 3.6 bakes it in.
- Size/L+ label → single-row diff discipline (Constraints + Out of Scope).
- Changeset enforcement → README path not in the enforced list; `no-changelog` label is the runbook Step 4 fallback.
- Scope creep / drive-by formatting → Out of Scope is explicit on this.
- CI deferred → `.github/workflows/` is empty, visible but PROJECT.md Decisions parks it as deliberate; reviewer may flag, runbook anticipates.

**Open / not-yet-verified for OUR PR:**
- **HIGHEST LEVERAGE: `PR_BODY.md` is missing the `Enhancement PR` H2 heading at the top.** `pr-template-policy.cjs` requires `Enhancement PR` as the discriminator heading that identifies which template matched. Current `PR_BODY.md` headings start with `Linked Issue` (line 13). Without `## Enhancement PR` first, `matchingTemplate()` returns `template: null` → `reason: 'PR body does not match the fix, enhancement, or feature template.'` → close for first-time contributor. Fix before PR-03.
- **`PR_TITLE.txt` placeholder `NNNN` must be substituted** with the real approved-enhancement issue number at PR-open time. Currently `docs(NNNN): list gsd-book-skill in Community table`. Substitution is mechanical but easy to forget.
- **Localized-README question is unresolved.** Runbook Step 5 acknowledges it but doesn't pre-commit. If the reviewer asks for the sync inline, doing it widens the PR; doing it as a follow-up cites L130 "one concern per PR." Decide in advance which branch to take.
- **`size/S` label expected** (single-row addition is ~1–2 additions). If `PR_BODY.md` is very long, the body itself doesn't count toward the diff size — only file additions/deletions do (`pulls.listFiles`).
- **Stale window for the Enhancement issue.** If TÂCHES doesn't label within 28 days, the stale-bot adds `stale`. Per runbook Step 1 expectations: politely ping Discord at the 2-week mark; have a Path 4 plan ready before day 42.

Bottom line: of the seven instant-death triggers above, six are addressed by the runbook. The one remaining sharp edge is the missing `## Enhancement PR` heading in `PR_BODY.md` — fix that before PR-03 or the PR closes itself in under a minute.
