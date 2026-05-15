<!--
This is the PR body for the gsd-book-skill listing PR.

IMPORTANT: Per CONTRIBUTING.md, this PR MUST link to a pre-approved issue
with the `approved-enhancement` label. Do NOT open this PR until:
  1. A Discussion has been opened and got a "yes, this is welcome" reply
  2. An Enhancement issue has been filed via enhancement.yml
  3. A maintainer has labeled that issue `approved-enhancement`

Replace `#NNNN` in the Closes line below with the actual approved issue number.
-->

## Enhancement PR

Add one row to the existing `## Community` table in `README.md` listing **gsd-book-skill** — a community-authored Claude Code skill that uses GSD as documented for an Amazon KDP (Kindle Direct Publishing) book launch workflow. This is an external project with zero maintenance burden on TÂCHES or the GSD maintainers. Closes #NNNN.

## Linked Issue

Closes #NNNN

> This PR is the implementation of the enhancement approved in #NNNN.
> The linked issue carries the `approved-enhancement` label.

---

## What this enhancement improves

The `## Community` table in `README.md` currently lists one OpenCode port (gsd-opencode) and the Discord link. This PR adds a single row for **gsd-book-skill** — the first community-authored skill row in that table. It is an external project that uses GSD's command surface as documented (`/gsd-new-project`, the discuss → plan → execute → verify loop, `/gsd-autonomous`, and the `.planning/` artifact structure) to drive a domain-specific 5-phase workflow for taking a markdown manuscript through a KDP launch (editorial review, AI chapter art, cover production, EPUB + paperback + hardcover compile, social pack, launch collateral).

## Before / After

The change is a single inserted row inside the existing Community table. Unified diff:

```diff
 | Project | Platform |
 |---------|----------|
 | [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
+| [gsd-book-skill](https://github.com/shoemoney/gsd-book-skill) | Claude Code skill — KDP book launch pipeline |
 | [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
```

One row added between the two existing rows. No other changes to the README.

## How it was implemented

The PR diff is exactly: 1 row added to the `## Community` table in `README.md` + 1 `.changeset/` fragment (or `no-changelog` label requested). No other files. No code, no tests, no `.planning/` artifacts, no agent surface changes, no installer changes.

### What gsd-book-skill is (reviewer context)

- **License:** MIT
- **Repo:** https://github.com/shoemoney/gsd-book-skill
- **Maintainer:** [@shoemoney](https://github.com/shoemoney) (Jeremy Schoemaker)
- **Install:** symlink `gsd-book-skill/skill/` → `~/.claude/skills/kdp-book-launch/`
- **Relationship to GSD:** external project; uses GSD's documented command surface and `.planning/` artifact structure (PROJECT.md / REQUIREMENTS.md / ROADMAP.md / STATE.md). Trigger words are all book-launch-specific ("launch a book on KDP", "build the EPUB", "make a book cover", etc.) — zero overlap with GSD's core command vocabulary.
- **Conformance audit:** the skill's repo includes a documented conformance audit confirming it follows GSD's contract (atomic commits, wave-based parallelization, `.planning/` discipline). See the skill's SKILL.md and README for the prominent TÂCHES / GSD credit.
- **Production-verified:** extracted from two real KDP launches by the maintainer.

## Testing

Documentation-only change; no automated tests are added or modified. Verified locally that:

- Markdown table parses correctly with three rows + a header
- The added row uses identical column alignment to the existing two rows
- The link target (`https://github.com/shoemoney/gsd-book-skill`) returns a 200 and resolves to a public MIT-licensed repo
- No other section of the README is affected

Platforms tested: N/A (docs-only change to a Markdown file; no platform-specific behavior).
Runtimes tested: N/A (docs-only change).

---

## Scope confirmation

- [x] The implementation matches the scope approved in the linked issue — no additions or removals
- [x] If scope changed during implementation, I updated the issue and got re-approval before continuing

This is the minimum-impact change discussed in the linked issue: one row, one section, no other edits.

---

## Checklist

- [x] Issue linked above with `Closes #NNNN` — PR will be auto-closed if missing
- [x] Linked issue has the `approved-enhancement` label — PR will be closed if missing
- [x] Changes are scoped to the approved enhancement — nothing extra included
- [x] All existing tests pass (`npm test`) — README change does not exercise any test path
- [x] No new tests required (docs-only README addition)
- [ ] `.changeset/` fragment added (`npm run changeset --type Changed --pr <NNNN> --body "..."`) — OR `no-changelog` label applied
  - The README is not in the CI-enforced changeset paths (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/`). Requesting `no-changelog` label.
- [x] Documentation updated if behavior or output changed — N/A (this IS the documentation update)
- [x] No unnecessary dependencies added

---

## Breaking changes

None. This is a documentation-only change that adds one row to an existing community-listing table.

## Acknowledgments

Thank you to TÂCHES and the GSD maintainers for building the framework this skill plugs into. gsd-book-skill is an external project, not a replacement — every architectural decision (phase model, atomic commits, parallel subagent dispatch, `.planning/` artifact discipline) is GSD's. We credit the upstream debt explicitly in our README and in our SKILL.md.

If this listing is not a fit for the Community table, we understand — the table is yours to curate. We'll keep the OSS repo independent either way, with a prominent link to GSD as the upstream framework.
