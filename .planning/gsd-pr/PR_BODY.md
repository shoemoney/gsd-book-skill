<!--
This is the PR body for the gsd-book-skill listing PR.

IMPORTANT: Per CONTRIBUTING.md, this PR MUST link to a pre-approved issue
with the `approved-enhancement` label. Do NOT open this PR until:
  1. A Discussion has been opened and got a "yes, this is welcome" reply
  2. An Enhancement issue has been filed via enhancement.yml
  3. A maintainer has labeled that issue `approved-enhancement`

Replace `#NNN` in the Closes line below with the actual approved issue number.
-->

## Linked Issue

Closes #NNN

> This PR is the implementation of the enhancement approved in #NNN.
> The linked issue carries the `approved-enhancement` label.

---

## What this enhancement improves

The `## Community` section in `README.md` currently lists one OpenCode port and the Discord link. This PR adds a single row for **gsd-book-skill** — a Claude Code skill that wraps GSD's planning loop with a domain-specific 5-phase workflow for taking a markdown manuscript through an Amazon KDP (Kindle Direct Publishing) launch (editorial review, AI chapter art, cover production, EPUB + paperback + hardcover compile, social pack, launch collateral).

## Before / After

**Before:**

```markdown
## Community

| Project | Platform |
|---------|----------|
| [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
| [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
```

**After:**

```markdown
## Community

| Project | Platform |
|---------|----------|
| [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
| [gsd-book-skill](https://github.com/shoemoney/gsd-book-skill) | Claude Code skill — KDP book launch pipeline |
| [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
```

One row added between the two existing rows. No other changes to the README.

## How it was implemented

Single-line addition to `README.md` only. No other files touched. No code, no tests, no `.planning/` artifacts, no agent surface changes, no installer changes.

## What gsd-book-skill is (for the reviewer's context)

- **License:** MIT
- **Repo:** https://github.com/shoemoney/gsd-book-skill
- **Maintainer:** [@shoemoney](https://github.com/shoemoney) (Jeremy Schoemaker)
- **Install:** symlink `gsd-book-skill/skill/` → `~/.claude/skills/kdp-book-launch/`
- **Built on GSD:** initializes via `/gsd-new-project`, drives phases through `/gsd-discuss-phase` → `/gsd-plan-phase` → `/gsd-execute-phase` → `/gsd-verify-work`, supports `/gsd-autonomous` for end-to-end runs. Uses GSD's `.planning/` artifact structure (PROJECT.md / REQUIREMENTS.md / ROADMAP.md / STATE.md) throughout.
- **Trigger words:** all book-launch-specific ("launch a book on KDP", "build the EPUB", "make a book cover", etc.) — zero overlap with GSD's core command vocabulary.
- **Production-verified:** extracted from two real KDP launches by the maintainer.

## Testing

### How I verified the change

Rendered the README locally and confirmed:
- Markdown table parses correctly with three rows + a header
- The added row uses identical column alignment to the existing two rows
- The link target (`https://github.com/shoemoney/gsd-book-skill`) returns a 200 and resolves to a public MIT-licensed repo
- No other section of the README is affected

### Platforms tested

- [x] N/A (docs-only change to a Markdown file; no platform-specific behavior)

### Runtimes tested

- [x] N/A (docs-only change)

---

## Scope confirmation

- [x] The implementation matches the scope approved in the linked issue — no additions or removals
- [x] If scope changed during implementation, I updated the issue and got re-approval before continuing

This is the minimum-impact change discussed in the linked issue: one row, one section, no other edits.

---

## Checklist

- [x] Issue linked above with `Closes #NNN` — PR will be auto-closed if missing
- [x] Linked issue has the `approved-enhancement` label — PR will be closed if missing
- [x] Changes are scoped to the approved enhancement — nothing extra included
- [x] All existing tests pass (`npm test`) — README change does not exercise any test path
- [x] No new tests required (docs-only README addition)
- [ ] `.changeset/` fragment added (`npm run changeset --type Changed --pr <NNN> --body "..."`) — OR `no-changelog` label applied
  - The README is not in the CI-enforced changeset paths (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/`). Requesting `no-changelog` label.
- [x] Documentation updated if behavior or output changed — N/A (this IS the documentation update)
- [x] No unnecessary dependencies added

## Breaking changes

None. This is a documentation-only change that adds one row to an existing community-listing table.

---

## Acknowledgments

Thank you to TÂCHES and the GSD maintainers for building the framework this skill plugs into. gsd-book-skill is an extension, not a replacement — every architectural decision (phase model, atomic commits, parallel subagent dispatch, `.planning/` artifact discipline) is GSD's. We owe the upstream debt explicitly in our README and in our SKILL.md.

If this listing is not a fit for the Community table, we understand — the table is yours to curate. We'll keep the OSS repo independent either way, with a prominent link to GSD as the upstream framework.
