# Submission Runbook — Listing `gsd-book-skill` in GSD's Community Table

**For:** Jeremy Schoemaker ([@shoemoney](https://github.com/shoemoney))
**Target repo:** [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done)
**Goal:** Add one row to the `## Community` table in `README.md` linking to `gsd-book-skill`.
**Format:** Path 2 (community-plugin PR), Discussion-gated.

---

## Why this isn't "just open a PR"

GSD's `CONTRIBUTING.md` is one of the strictest issue-first policies on GitHub. **A cold PR will be auto-closed by their CI** before any human reads it. Their default PR template literally opens with: *"Wrong template — please use the correct one for your PR type."*

The realistic path is:

1. **Discussion first** — gauge whether a "skill built on GSD" entry is welcome at all
2. **Then an Enhancement issue** — get it labeled `approved-enhancement`
3. **Then the PR** — using the enhancement template, with `Closes #NNN`

This protects you from doing work that gets nuked. Each step is cheap and bails out gracefully if the answer is no.

---

## Step 1 — Open a GitHub Discussion (gauge openness)

Go to: https://github.com/gsd-build/get-shit-done/discussions

Click **New discussion** → Category: **General** (or **Ideas** if available).

**Title:**
```
Listing a third-party Claude Code skill in the Community table — welcome?
```

**Body (copy-paste, adjust the introduction line if you want):**

```markdown
Hi TÂCHES and GSD maintainers — first off, thank you for GSD. I've used it to
ship two books end-to-end and the autonomous loop is the single biggest reason
those launches happened on time.

I'm writing to ask a simple yes/no question before doing any work.

**The ask:** would a single-row addition to the `## Community` table in `README.md`
be welcome, pointing to a third-party Claude Code skill that's built on GSD?

**The skill:**
- Repo: https://github.com/shoemoney/gsd-book-skill
- License: MIT
- What it is: a Claude Code skill that wraps GSD's 5-command loop with a
  domain-specific workflow for taking a markdown manuscript through an Amazon
  KDP launch (editorial review, AI chapter art, cover production, EPUB +
  paperback + hardcover compile, social pack, launch collateral).
- Built on GSD: initializes via `/gsd-new-project`, drives phases through the
  discuss/plan/execute/verify loop, supports `/gsd-autonomous` end-to-end.
- Trigger words are all book-launch-specific — zero overlap with GSD core.
- Production-verified across two real KDP launches.

**The exact diff I'm proposing:**

\`\`\`diff
 | [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
+| [gsd-book-skill](https://github.com/shoemoney/gsd-book-skill) | Claude Code skill — KDP book launch pipeline |
 | [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
\`\`\`

**If the answer is yes:** I'll file an enhancement issue using `enhancement.yml`,
wait for the `approved-enhancement` label, then open a PR with the
enhancement template. Per CONTRIBUTING.md, I won't touch any code before the
issue is approved.

**If the answer is no, or you'd rather keep that table curated tightly:**
that's completely fair — your README, your call. I'll keep gsd-book-skill
independent and continue linking to GSD prominently from our side. No hard
feelings either way; just wanted to ask before filing an issue you'd have
to spend cycles closing.

Thank you for the framework. Star, sponsor, and tell-other-authors-about-it
have all already happened from this corner.

— Jeremy (@shoemoney)
```

**Click Start discussion.** Then wait.

### Expectations

- TÂCHES is responsive on Discord and on GitHub. A reaction or comment within 1–2 weeks is normal.
- A reply of "yes" → proceed to Step 2.
- A reply of "we'd rather keep that table tight" or "not a fit right now" → stop. Go to **Fallback: Path 4** at the bottom of this runbook.
- A reply of "we'd accept it under a different format" (e.g., new sub-section, separate registry file, gallery doc) → ask follow-up questions in the same Discussion to align on the exact shape before filing an issue.
- No reply after 2 weeks → ping the Discord (`https://discord.gg/mYgfVNfA2r`) once, politely. If still no reply after another week, fall back to Path 4.

---

## Step 2 — File an Enhancement Issue (only after Discussion is positive)

Go to: https://github.com/gsd-build/get-shit-done/issues/new?template=enhancement.yml

The `enhancement.yml` template is at `.github/ISSUE_TEMPLATE/enhancement.yml` upstream. Fill it out completely. Every required field must be filled or the issue gets closed without revision.

**Suggested issue title:**
```
Enhancement: extend Community table with a row for gsd-book-skill (KDP launch skill built on GSD)
```

**For the "Problem being solved" field:**
> The `## Community` table in `README.md` currently lists one OpenCode runtime port and the Discord link. Downstream projects that build on GSD's framework (Claude Code skills, agents, domain extensions) have no surfacing path back to GSD users who would benefit from them. Adding a single row for an MIT-licensed, production-verified skill built on GSD's `.planning/` discipline and command loop closes the smallest possible discoverability gap without committing the maintainer to building a registry or accepting more entries.

**For the "Concrete benefit" field:**
> GSD users publishing books on KDP get a one-link path to a skill that's already been used to ship books end-to-end. Maintainer cost: one row in a Markdown table, one human approval, zero ongoing maintenance.

**For the "Scope of changes" field:**
> Single insertion of one row into the existing `## Community` table in `README.md`. Three cells: project link, dash, short platform description. No other files touched. No agent, command, workflow, hook, or test change. No localized-README sync (deferred to a separate PR if requested).

**For the "Alternatives considered" field:**
> 1. A new "Skills built on GSD" sub-section — rejected because it implies an open call for entries and creates ongoing curation burden. The single-row addition reuses the existing surface.
> 2. A separate `COMMUNITY.md` or `plugins.json` registry — rejected as out-of-scope for a single-entry listing. If the maintainer prefers this format, happy to file a separate feature issue for it instead.
> 3. Keeping the project independent (Path 4) — this is the no-op alternative. The maintainer can choose this by declining this enhancement; the skill stays useful either way.

**Link to the prior Discussion** (whichever URL you got from Step 1).

**Submit the issue.** Wait for a maintainer to label it `approved-enhancement`.

### Expectations

- Approval can take days to weeks. Do not poll.
- If the maintainer asks clarifying questions, answer them in the issue thread.
- **Do not write a single line of code until the label is applied.** PRs against unapproved enhancement issues are closed without review.

---

## Step 3 — Fork, Branch, Commit (only after `approved-enhancement` label is on)

Once your issue has the `approved-enhancement` label, you're cleared to do the work. Note the issue number — you'll need it for the commit message.

```bash
# 3.1 — Fork the repo on GitHub via the web UI
# Visit https://github.com/gsd-build/get-shit-done and click "Fork"
# (Default settings: only fork the `main` branch)

# 3.2 — Clone your fork locally
cd ~/Projects
git clone https://github.com/shoemoney/get-shit-done.git gsd-fork
cd gsd-fork

# 3.3 — Sync with upstream (safety: make sure you're at upstream HEAD)
git remote add upstream https://github.com/gsd-build/get-shit-done.git
git fetch upstream
git checkout main
git reset --hard upstream/main

# 3.4 — Create the feature branch
# Replace NNNN with your actual approved issue number (4 digits typically)
ISSUE_NUM=NNNN  # ← fill this in
git checkout -b docs/${ISSUE_NUM}-add-gsd-book-skill

# 3.5 — Apply the README diff
# Open README.md in your editor. Find line 244 (the Discord row).
# Insert a new line BEFORE it:
#
#   | [gsd-book-skill](https://github.com/shoemoney/gsd-book-skill) | Claude Code skill — KDP book launch pipeline |
#
# Save. Verify the diff is EXACTLY one row added:

git diff --stat README.md
# Expected output: README.md | 1 +
#                  1 file changed, 1 insertion(+)

git diff README.md
# Visually confirm this matches .planning/gsd-pr/README_DIFF.md
```

**If `git diff --stat` shows more than one insertion, STOP.** Revert and re-apply only the one row. Drift is a rejection reason.

```bash
# 3.6 — Commit, using GSD's commit-message convention
# Pattern (from their git log): docs(NNNN): <subject>
git add README.md
git commit -m "docs(${ISSUE_NUM}): list gsd-book-skill in Community table"

# 3.7 — Push the branch
git push -u origin docs/${ISSUE_NUM}-add-gsd-book-skill
```

---

## Step 4 — Open the PR

Go to: https://github.com/gsd-build/get-shit-done/compare

- **Base repository:** `gsd-build/get-shit-done`
- **Base branch:** `main`
- **Head repository:** `shoemoney/get-shit-done`
- **Compare branch:** `docs/NNNN-add-gsd-book-skill`

Click **Create pull request**.

### Critical: use the Enhancement template, not the default

When the PR form loads, you'll see the default "wrong template" body. Replace the URL with:

```
https://github.com/gsd-build/get-shit-done/compare/main...shoemoney:docs/NNNN-add-gsd-book-skill?template=enhancement.md
```

…or, after creating the PR, edit the body to use the enhancement template contents from `~/Projects/gsd-book-skill/.planning/gsd-pr/PR_BODY.md`.

**PR title:** `docs(NNNN): list gsd-book-skill in Community table` (replace NNNN).

**PR body:** copy the contents of `.planning/gsd-pr/PR_BODY.md` from this repo. Replace `#NNN` with your actual issue number.

### After opening, immediately:

1. **Apply the `no-changelog` label** if you have permission. README is not in the changeset-enforced paths, so a fragment isn't required — but the lint may still flag it. The `no-changelog` label opts you out. If you can't apply labels yourself, mention in a PR comment: *"Requesting `no-changelog` label — README is not in the changeset-enforced paths per CONTRIBUTING.md."*

2. **Confirm CI is green.** If any check fails, read the failure carefully. It's almost certainly the changeset check or the issue-link check. Fix and re-push.

---

## Step 5 — Review Cycle Expectations

- **Review time:** plausibly 3–14 days. CONTRIBUTING.md is explicit that maintainers run tests locally and validate the implementation matches the linked-issue spec — they don't just rubber-stamp green CI.
- **Possible feedback:**
  - "Tighten the platform description" → adjust the third column, push a fixup commit.
  - "Sync the localized READMEs in this PR" → either do it (push commits adding rows to `README.pt-BR.md`, `README.zh-CN.md`, `README.ja-JP.md`, `README.ko-KR.md` with the same row in the appropriate language equivalent) OR ask if they'd prefer a follow-up PR per the "one concern per PR" rule.
  - "Move to a separate community doc instead" → that's outside the approved scope of the issue. Ask the maintainer to re-scope the issue or close-and-refile.
  - "We've changed our mind" → withdraw the PR gracefully (`Closing as no-longer-pursued per maintainer guidance, thanks for the consideration`). Go to Fallback: Path 4.

- **Respond to feedback within 1–2 days** if you can. Lingering PRs get stale.
- **Do NOT scope-creep.** If you find yourself wanting to "also fix that typo while I'm in here" — don't. New PR, new issue.

---

## Step 6 — After Merge

1. Update your fork: `git checkout main && git fetch upstream && git reset --hard upstream/main && git push --force-with-lease origin main`
2. Delete the feature branch: `git branch -d docs/NNNN-add-gsd-book-skill && git push origin --delete docs/NNNN-add-gsd-book-skill`
3. Update `~/Projects/gsd-book-skill/README.md` to mention the upstream listing if you want.
4. Tweet / post / Discord-mention the merge. It legitimizes the skill and credits TÂCHES publicly.

---

## Fallback: Path 4 (if the PR is declined OR the Discussion gets a no)

Don't take it personally. TÂCHES has every right to keep the Community table curated tightly. A polite decline is not a rejection of the skill — it's a maintenance-budget call.

**Action plan for Path 4:**

1. **Keep `gsd-book-skill` 100% independent.** No upstream tie required.
2. **Leave the prominent "Built on GSD" attribution** in `README.md` and `skill/SKILL.md` exactly as it stands. The credit flows upstream either way.
3. **Encourage users in our README to install GSD first** (already done — line 22 of our README).
4. **Star, sponsor, and mention TÂCHES** publicly via your own channels (blog, X, Discord, podcast appearances).
5. **Optional:** publish the skill on a curated "Claude Code skills" list if one emerges, or build a small static site (`gsd-skills.com` or similar) that lists community skills built on GSD. If that list grows, TÂCHES may organically link to it from a future Community section — but that's a 6-12 month play, not something to force.

The OSS repo and the user-facing skill keep their value whether or not the upstream listing happens.

---

## Quick reference — file inventory in this directory

| File | Purpose |
|---|---|
| `PR_TITLE.txt` | One-line title for the PR (replace `NNNN` with issue number) |
| `PR_BODY.md` | Full PR body using the enhancement template |
| `README_DIFF.md` | Exact diff to apply to upstream `README.md` |
| `SUBMISSION_RUNBOOK.md` | This file |

Adjacent in `../`:

| File | Purpose |
|---|---|
| `../gsd-pr-recon.md` | Recon findings from inspecting the upstream repo |
| `../gsd-conformance-notes.md` | What was edited in `skill/SKILL.md` for GSD-conformance |

---

## TL;DR

1. **Discussion first.** Don't fork-and-PR cold — it'll get auto-closed.
2. **Then enhancement issue.** Wait for `approved-enhancement` label.
3. **Then PR.** One row, enhancement template, `Closes #NNN`.
4. **If declined:** Path 4 — stay independent, keep upstream attribution prominent.

You've already done 90% of the upstream-credit work in our own README. The PR is a nice-to-have, not a must-have.
