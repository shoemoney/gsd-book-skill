# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial repo scaffolding (README, LICENSE, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, .gitignore, CHANGELOG, TASKS).
- **Imported skill content** from the production extraction at `~/.claude/skills/kdp-book-launch/`:
  - `skill/SKILL.md` — 5-phase runbook (Manuscript Lock → Chapter Illustrations → Cover Production → Build & Compile → KDP Launch Prep)
  - `skill/scripts/` — 11 functional scripts + 1 shared `_config.py` loader
  - `skill/templates/` — 7 prompt/config templates with `{{PLACEHOLDERS}}`
  - `skill/references/` — 8 methodology docs
- Symlinked `~/.claude/skills/kdp-book-launch` → `~/Projects/gsd-book-skill/skill` so the canonical source is this repo and the installed skill stays in sync.
- Scrubbed Jeremy-Christ-specific examples out of `references/canon-audit-methodology.md` — replaced with `{{PLACEHOLDER}}` form plus a generic worked example.
- One Jeremy-Christ reference retained intentionally: `skill/SKILL.md` line 8 names the reference implementation in a single sentence at the top of the doc.

### Pending
- Phase A from `TASKS.md`: full scrub-and-parameterize pass on every script (verify no hardcoded paths leaked through).
- v0.1.0 release.
- Public GitHub publish.
- CI workflow.
- Example walkthrough (`examples/demo-book/`).

---

## [0.1.0] — TBD

Will mark the first public release once Phases A and B of [TASKS.md](./TASKS.md) are complete.
