# GSD Upstream Recon — Path-2 PR Feasibility

Date: 2026-05-15
Upstream clone: `~/Projects/gsd-upstream/get-shit-done` (read-only reference)
Upstream HEAD: `a7f0af2c` (v1.42.1 era — confirmed via `docs/RELEASE-v1.42.1.md`)

---

## 1. Is there an existing "Community Plugins" / "Built on GSD" section?

**Partially yes.** `README.md` lines 239–245 has a `## Community` section:

```markdown
## Community

| Project | Platform |
|---------|----------|
| [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
| [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
```

Two rows, two columns. The current entries are:
- A runtime port (gsd-opencode)
- A chat link (Discord)

There is **no dedicated "Skills built on GSD" or "Community Skills" or "Extensions" registry** — this Community table is the only community-facing surface in the README. There is **no `plugins.json` or `skills.json`** registry file anywhere in the repo.

**The recommended insertion point: add ONE new row to this existing table.** Do not create a new section — that's a bigger ask and reads as a feature request rather than an enhancement.

## 2. Does GSD accept community-plugin PRs at all?

**Implicitly yes** (the gsd-opencode row exists), but **not on the casual PR path.**

The CONTRIBUTING.md governance is extremely strict. Pulled out the load-bearing constraints:

- **"No code before approval."** Hard rule, no exceptions.
- **Three PR types** with rising bars: Fix, Enhancement, Feature. Each has a separate PR template at `.github/PULL_REQUEST_TEMPLATE/{fix,enhancement,feature}.md`.
- **Every PR must link an approved-labeled issue.** Auto-closed if not.
- The default `.github/pull_request_template.md` literally says: *"Every PR must use a typed template. Using this default template is a reason for rejection."*
- The default template has a **narrow carve-out** at the bottom for "CI/tooling changes, dependency updates, or doc-only fixes with no linked issue" — but "add my third-party project to your community table" is closer to promotional content than a doc fix. Banking on this carve-out is risky.
- **No draft PRs** — auto-closed.
- **Conventional commits required** — git log confirms `feat(NNNN):`, `fix(NNNN):`, `docs(NNNN):` patterns with issue numbers prefixed.

A docs-only README addition is **most credibly classified as an Enhancement** (it improves an existing section — the Community table — by extending it). That requires:

1. Open an Enhancement issue (`enhancement.yml`)
2. Wait for `approved-enhancement` label from a maintainer
3. ONLY THEN open a PR using `enhancement.md` template
4. Drop a `.changeset/` fragment OR use the `no-changelog` label

## 3. Commit message convention

From recent git log:
```
fix(3537): route every phase-number ROADMAP regex through phaseMarkdownRegexSource (#3538)
feat(3530): STATE.md Document Module via generator (Phase 1 of #3524) (#3531)
docs(3524): CJS↔SDK hard-seam ADR + phased PRD (#3529)
```

Pattern: `<type>(<issue#>): <subject> (#<pr#>)`. Both the issue number and the PR number appear.

For our PR, the commit will look like:
```
docs(NNNN): list gsd-book-skill in Community table
```

…where NNNN is the issue number GitHub assigns to the enhancement issue we file.

## 4. PR templates

- `.github/PULL_REQUEST_TEMPLATE/fix.md` — bug fixes only
- `.github/PULL_REQUEST_TEMPLATE/enhancement.md` — **our target** (improves existing Community section)
- `.github/PULL_REQUEST_TEMPLATE/feature.md` — net-new commands/workflows
- `.github/pull_request_template.md` — default scolding template, NOT to be used

Enhancement template requires: `Closes #NNN` with `approved-enhancement` label, Before/After examples, scope confirmation, platforms-tested checklist, changeset fragment.

## 5. Changeset requirement

Every PR touching certain paths must drop a `.changeset/*.md` fragment via `npm run changeset --`. README is NOT in the enforced path list (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/`) — so a README-only PR likely qualifies for the `no-changelog` label opt-out. If unsure, add the fragment.

## 6. Naming conventions for plugins/extensions

None codified. The single precedent (`gsd-opencode`) uses `gsd-` prefix. Our project is already named `gsd-book-skill`, which fits.

## 7. Should we go Path 2 or Path 4?

**Path 2 is viable but requires a Discussion-first gated approach.** A cold fork-and-PR will be auto-closed by their CI before any human reads it. The runbook MUST start with a GitHub Discussion to gauge openness BEFORE filing an enhancement issue.

**Reasonable expectation:** modest acceptance probability. The maintainer (TÂCHES) is solo-dev focused and intentionally lean. Adding a community-listing row is the minimum-imposition ask possible, but they may still prefer to keep the table focused on runtime ports + official channels.

**Path 4 fallback** (publish independently, link to GSD upstream from our README): already what we have today. Costs nothing to fall back to. The Discussion conversation alone has value — it puts us on TÂCHES's radar and may lead to other surfaces (Discord pin, social mention, a future curated list).

## 8. Recommendation

**Proceed with Path 2 via Discussion-first.** All PR materials prepared and staged in `.planning/gsd-pr/`. If the Discussion gets a "no thanks" or no reply within 2 weeks, fall back to Path 4 and keep the OSS repo independent. The materials we prepare are reusable for an external "Built with GSD" gallery if one ever shows up.
