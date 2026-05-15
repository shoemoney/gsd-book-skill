---
generated: 2026-05-15
focus: features
---

# FEATURES — Reviewer Expectations for Community Plugin PRs

Context: this is research for the `gsd-book-skill` PR-prep milestone, which aims to add a single row to the upstream `gsd-build/get-shit-done` README Community table. All evidence pulled from the upstream clone at `~/Projects/gsd-upstream/get-shit-done` (HEAD `a7f0af2c`) and the live GitHub PR list (606 closed-unmerged PRs on file).

## Table stakes (without these, the PR gets closed before a human reads it)

- **Linked, pre-approved issue with the right label.** GSD's CI bot enforces `approved-enhancement` / `approved-feature` / `confirmed-bug` and auto-closes PRs missing the label. Source: `CONTRIBUTING.md` lines 36, 50; PR #3492 closed by bot with literal message "This PR does not follow one of the required pull request templates."
- **Typed PR template — not the default one.** PR #3401 (Spanish README translation) was closed with the one-line comment "follow contribution guidelines" because the body was the default template. The default template at `.github/pull_request_template.md` literally says "Using this default template is a reason for rejection."
- **Closing-keyword link to the approved issue.** `Closes #NNN` / `Fixes #NNN` / `Resolves #NNN` in the PR body. CI grep is mechanical — missing keyword = auto-close. Source: `CONTRIBUTING.md` PR Guidelines section.
- **Conventional-commit subject with issue number.** Pattern is `<type>(<issue#>): subject (#<pr#>)`. Every merged commit on `main` follows it (e.g., `fix(3537): route every phase-number ROADMAP regex ...`, `docs(3524): CJS↔SDK hard-seam ADR ...`).
- **No draft PRs.** `CONTRIBUTING.md` line 110: "draft PRs are automatically closed." PR #3405 (a maintainer's own draft) got closed under this rule.
- **Changeset fragment OR `no-changelog` label.** README-only changes are outside the enforced paths (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/`) so `no-changelog` is legitimate, but the lint job (`scripts/changeset/lint.cjs`) is unambiguous: one or the other must be present.
- **Single concern per PR.** "One concern per PR — bug fixes, enhancements, and features must be separate PRs" — CONTRIBUTING.md. A README row + an unrelated fix bundled together gets bounced.

## Differentiators (raise approval odds significantly)

- **Tiny diff, single file.** Smallest visible merged enhancement PRs are 1–10 lines. The whole maintainer commit that introduced the Community section (`7e9b8dec` "docs: add community ports section to README") was 9 added lines. A one-row diff signals low maintenance burden.
- **Discussion-first signal in the issue body.** CONTRIBUTING.md feature path explicitly says "Discuss first — check Discussions to see if the idea has been raised." Even for an enhancement, a paragraph saying "I checked Discussions and didn't find prior art, here's why this fits scope" is the difference between "approved-enhancement" within a week and silence.
- **Repo the reviewer clicks through to looks maintained.** When a maintainer reads the proposed README row, they will click the link. Real `HTTP-Referer` URLs, an `examples/` fixture, a populated README, and a `requirements.txt` are the table stakes for that 30-second drive-by check. (This is exactly what the project's PR-01 pre-empts.)
- **GSD-vocabulary fluency.** CONTRIBUTING.md line 80–95: "Read CONTEXT.md before naming or refactoring … Use CONTEXT.md vocabulary consistently in … PR text." A PR body that names GSD primitives correctly (`.planning/`, `/gsd-discuss-phase`, `/gsd-plan-phase`, `/gsd-execute-phase`, `/gsd-verify-work`, `/gsd-autonomous`) reads as "this person knows the project." The conformance audit already locks this in for the skill itself.
- **Credit to the maintainer / project.** Both existing rows (gsd-opencode, Discord) are positioned as community amplification of GSD, not as standalone marketing. The skill's "Built on Get Shit Done (GSD) by TÂCHES" callout in SKILL.md and the dedicated Acknowledgments section in README directly mirrors that posture.
- **Show, don't market.** A PR body that demonstrates one concrete user value ("here's the GSD `.planning/` artifacts this skill produces") beats adjective-heavy ones.

## Anti-features (actively hurt — do NOT include)

- **A new section header** (e.g., `## Community Skills` or `## Built on GSD`). The recon doc nailed this: a new section is a feature request, not an enhancement. The maintainer (not contributors) added the existing section in `7e9b8dec`. Asking for structural changes to README invites a "no, just use the existing table" reply at best, "won't merge" at worst.
- **CI additions, lint configs, repo-wide doc updates** bundled into the same PR. Violates "one concern per PR." Will trigger "please split this." Confirmed by closures of PRs that touched more than the approved scope.
- **Adjective-heavy marketing prose** ("the most powerful KDP launch toolkit"). PR #3401 wasn't closed for marketing per se, but reviewers on this repo are visibly allergic to any text that doesn't justify itself.
- **Edits to translated READMEs** (README.ja-JP.md, README.zh-CN.md, etc.). PR #3518 (Russian translation) was closed for template violation; even if compliant, translating without coordination with the active translation owners is scope creep. Don't touch translated files — let the existing translation maintainers carry the row over.
- **Self-modified CHANGELOG.md.** CONTRIBUTING.md is explicit: "Do not edit CHANGELOG.md directly." Two PRs editing it conflict on merge.
- **Comments shaped by AI tooling in the PR body** (CodeRabbit-style auto-comments, "I asked Claude to summarize", emoji-stuffed dividers). PRs #3217, #3205, #3341 are bot-authored CodeRabbit auto-fix PRs — all closed unmerged. The signal is "human-curated, terse, GSD vocabulary."
- **Asking the maintainer to "review when you have a chance."** No nudge in the PR body. Nudges, if needed, go in the issue thread after 2 weeks of silence (per Discord-community norms documented in PROJECT.md).

## Patterns from recently merged PRs

External-author merges to `main` are vanishingly rare — the 25 most-recent closed-unmerged PRs span May 7–15, 2026 and include external contributors `umee-man`, `spiratil`, `DansiDanutz`, `rasfies`, `radioflyer28`, `marcfargas`, `ethanio12345`, `jliounis`, `davidop`, `felipebianchini2006`, `aleckyann`. Common closure reasons: template violation, no linked approved issue, draft state, scope mismatch.

Recently merged PRs that DID succeed share these traits:
- **#3538** `fix(3537): route every phase-number ROADMAP regex through phaseMarkdownRegexSource` — single concern, scoped to one regex helper, linked confirmed-bug issue.
- **#3531** `feat(3530): STATE.md Document Module via generator (Phase 1 of #3524)` — Phase-1 of an approved multi-phase PRD, scoped exactly to approved boundaries, size/XL but pre-approved.
- **#3529** `docs(3524): CJS↔SDK hard-seam ADR + phased PRD` — docs-only PR, approved-enhancement label, links into existing ADR/PRD numbering convention.
- **#3506** `chore(ci): add 5-day auto-close for awaiting-retest / needs-reproduction` — single-line CI tweak with `no-changelog` label.
- **#2937** `feat(statusline): add opt-in context_position config for narrow terminals` — old issue (#2937) re-opened and shipped, demonstrates that approved-enhancement labels age well and a properly-scoped small enhancement can land.

The original `## Community Ports` section itself was added in commit `7e9b8dec` BY THE MAINTAINER (not by a community PR). The two rows that exist today (gsd-opencode, Discord) were both added by the maintainer. **There is no prior community-authored PR adding to this table.** The book-skill PR will be the first.

## Patterns from rejected PRs

From the 25 most recent closed-unmerged PRs (May 7–15, 2026):
- **#3401** (`davidop`) — Spanish README translation. Closed in <24h with "follow contribution guidelines." No approved issue, default template.
- **#3492** (`rasfies`) — SDK safety feature. Closed by bot for default-template body, despite the code being legitimate.
- **#3518** (`umee-man`) — Russian translations. Closed for template + no approved-enhancement.
- **#3296** (`aleckyann`) — Brazilian Portuguese localization. Same pattern.
- **#3410** (`jliounis`) — "feat: add Perplexity integrations." 24-of-32-tasks done, but closed: this is a feature-level addition without `approved-feature` and conflicts with "GSD is intentionally lean."
- **#3428** (`marcfargas`) — `--claude-plugin` install mode. Closed in draft state.
- **#3341, #3217, #3205** — CodeRabbit bot PRs auto-closed.

Rejection-reason taxonomy:
1. **Process violation** (no template / wrong template / no linked issue / draft) — ~70% of closures
2. **Scope mismatch** (feature without approved-feature label, scope creep beyond approved issue) — ~20%
3. **Conflicts with project design philosophy** (anything that adds maintenance burden to a solo-dev project without strong justification) — ~10%

Almost none of the closures were on technical merit. **The bar is procedural conformance, not code quality.** This is the load-bearing insight.

## Specific to gsd-book-skill

Mapping against our drafted PR materials in `.planning/gsd-pr/`:

**Boxes we already check:**
- Conformance-audit complete; SKILL.md names the GSD loop commands explicitly (`gsd-conformance-notes.md`)
- Drafted enhancement issue body (`STEP1_DISCUSSION_POST.md`) — uses GSD vocabulary, declares scope, demonstrates GSD integration
- Drafted PR body, title, README diff already prepared (`PR_BODY.md`, `PR_TITLE.txt`, `README_DIFF.md`)
- Conventional commit format planned: `docs(NNNN): list gsd-book-skill in Community table`
- Plan to use the Enhancement PR template (correct classification per recon)
- Plan to drop `.changeset/` fragment OR use `no-changelog` label
- Repo positions itself as an extension to GSD, not a competitor — already credits TÂCHES prominently in both README and SKILL.md
- PR-01 (`requirements.txt`, populated `examples/`, real HTTP-Referer URL) explicitly addresses the "reviewer clicks through to our repo and finds nothing embarrassing" failure mode

**Boxes we don't check, and shouldn't:**
- No CI in our repo. Empty `.github/workflows/` is conspicuous but the project doc correctly classifies it as a non-blocker for a docs-only README PR. Add only if a reviewer asks.
- No Discussion-first thread filed yet. Recon doc recommends this; we should consider filing one BEFORE the Enhancement issue to gauge openness, especially since this would be the first community-authored row in the Community table.

**Risks specific to us that the above doesn't fully cover:**
- The Community section is currently 2-column (Project | Platform). Our drafted `README_DIFF.md` needs to match exactly — do NOT introduce a 3rd "Description" column even if it would tell our story better. Restructuring the table is a feature, not an enhancement.
- The existing rows are a runtime port (gsd-opencode) and a chat link (Discord). A skill is genuinely a new category. The Enhancement issue body must defend why a skill belongs in this table without sounding like it's asking for category expansion (which would be feature-level).
- TÂCHES is solo-dev focused. Anything in the PR/issue body that reads as "this expands GSD's surface" raises maintenance-burden alarms. Frame as "an external project that uses GSD as documented, no maintainer action required."

**Recommended deliberate skips:**
- No README badge swap, no CI workflows, no extra logos, no new TLD assets. Single-row addition only.
- No translated-README updates. Let the translation owners port the row in their own cadence.
- No CHANGELOG.md edit — use `no-changelog` label since README isn't in the enforced changeset paths.

## Sources

- Local clone: `~/Projects/gsd-upstream/get-shit-done` (HEAD `a7f0af2c`, v1.42.1)
- `CONTRIBUTING.md` lines 19–155
- Commit `7e9b8dec` "docs: add community ports section to README" — origin of the Community section
- Commit `8d2651d1` Colin — only external community-table edit (and it was a fix removing a dead link, not adding)
- GitHub PR pages for #3401, #3492, #3518, #3296, #3410, #3428, #3505, #3506, #3531, #3538
- [GSD upstream PRs (closed, unmerged)](https://github.com/gsd-build/get-shit-done/pulls?q=is%3Apr+is%3Aclosed+is%3Aunmerged)
- [Anthropic skills marketplace contribution path](https://github.com/anthropics/skills)
- [Vercel Integration Approval Checklist](https://vercel.com/docs/integrations/create-integration/approval-checklist)
