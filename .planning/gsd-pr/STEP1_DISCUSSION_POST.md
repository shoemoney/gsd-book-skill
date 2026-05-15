# Step 1 — Ready-to-Paste Discussion Post

This is the standalone, polished version of the Step 1 Discussion post. Copy from inside the code fences below directly into GitHub's New Discussion form.

**Target URL:** https://github.com/gsd-build/get-shit-done/discussions/new

**Category:** General (or Ideas, whichever they have configured)

---

## Title (one line, copy verbatim)

```
Listing a third-party Claude Code skill in the Community table — welcome?
```

---

## Body (copy from inside the fenced block)

```markdown
Hi TÂCHES and the GSD maintainers — first, thank you. I've shipped two books
end-to-end with GSD as the orchestrator, and the autonomous loop is the single
biggest reason those launches happened on time. The `.planning/` artifact
shape, the wave-based `execute-phase`, the atomic commits — it just works.

Writing to ask a yes/no question before doing any work.

**The ask:** would a single-row addition to the `## Community` table in
`README.md` be welcome, pointing to an external project — a community-authored
Claude Code skill — that uses GSD as documented? Zero maintenance burden on
you: the skill lives in its own repo with its own maintainer, and the only
thing landing in your repo is one table row.

**The skill — gsd-book-skill:**
- Repo: https://github.com/shoemoney/gsd-book-skill
- License: MIT
- What it is: a Claude Code skill that wraps GSD's 5-command loop with a
  domain-specific workflow for taking a markdown manuscript through an Amazon
  KDP launch — editorial review and canon audit, AI chapter art, cover
  production (front + back + spine), EPUB + paperback + hardcover compile,
  social pack, launch collateral.
- Uses GSD as documented: initializes via `/gsd-new-project`, drives phases
  through the discuss → plan → execute → verify loop, supports
  `/gsd-autonomous` end-to-end. The skill's `SKILL.md` credits GSD
  prominently at the top and links to your repo. A conformance audit in the
  skill's repo confirms `.planning/` artifact discipline, atomic commits,
  and wave-based execution match GSD's contract.
- Trigger words are all book-launch-specific — zero overlap with GSD core
  commands.
- Production-verified across two real KDP launches before being extracted as
  a reusable skill.

**The exact diff I'm proposing:**

\`\`\`diff
 | [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
+| [gsd-book-skill](https://github.com/shoemoney/gsd-book-skill) | Claude Code skill — KDP book launch pipeline |
 | [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
\`\`\`

One row. No new section. No code changes. No new dependencies.

**If yes** — I'll file an enhancement issue using `enhancement.yml`, wait for
the `approved-enhancement` label, then open a PR with the enhancement
template. Per CONTRIBUTING.md, I won't touch code before the issue is
approved.

**If no, or you'd rather keep the table tightly curated** — that's
completely fair. Your README, your call. I'll keep `gsd-book-skill`
independent and continue linking back to GSD prominently. Asking first so I
don't file an issue you'd have to spend cycles closing.

Thank you for the framework. Star, sponsor, and tell-other-authors-about-it
have all already happened from this corner.

— Jeremy (@shoemoney)

P.S. Happy to take this conversation to Discord if that's preferred — just
let me know the better venue.
```

---

## After clicking "Start discussion"

- Note the Discussion URL — you'll reference it from the issue and PR if those steps happen.
- Subscribe to it so you get notifications.
- Then wait. Don't bump it for at least 5 business days.

## What's in the polishes vs. the runbook's earlier draft

- Opens with a specific, concrete sentence ("shipped two books end-to-end with GSD as the orchestrator") instead of a generic thank-you. Establishes you're a real user, not a drive-by.
- Names two specific GSD features by name (`.planning/`, `execute-phase`, atomic commits) so it reads like someone who actually used the framework.
- Tightened the "if no" paragraph from 4 sentences to 3.
- Added a P.S. offering to move to Discord — gives TÂCHES an easy lower-friction option if a public Discussion thread feels heavy.

## Notes on tone

This is the GSD repo. TÂCHES is a real person who built something they care about. The discussion should read like one indie maker emailing another — warm, specific, no marketing-speak, no list of feature requests, no aggressive ask. Two minutes of their attention, one yes/no, both branches handled gracefully. That's the bar.
