---
generated: 2026-05-15
focus: stack
---

# STACK — GSD Community Plugin Ecosystem

## What "GSD-aligned plugin" means today

There is **no formal "GSD-aligned" certification, registry, or manifest** in the upstream `gsd-build/get-shit-done` repo. The Community table in upstream `README.md` has exactly two rows (`gsd-opencode` and a Discord link) and no admission criteria are codified anywhere in `CONTRIBUTING.md` (Confidence: HIGH — verified via WebFetch of README and CONTRIBUTING; recon notes in `.planning/gsd-pr-recon.md` corroborate).

In practice, what reviewers and the wider community treat as "GSD-aligned" is a *behavioral* contract, not a file format:

- [ ] Reads/writes the `.planning/` artifact structure (`PROJECT.md`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md`)
- [ ] Routes work through GSD's slash-command surface — at minimum `/gsd-new-project`, the discuss → plan → execute → verify loop, and/or `/gsd-autonomous`
- [ ] Skill triggers do not collide with any `/gsd-*` core command vocabulary
- [ ] Credits TÂCHES and the canonical repo prominently (link to `gsd-build/get-shit-done` and/or `glittercowboy/get-shit-done`)
- [ ] Does NOT impersonate a core command (no `gsd-` prefix in the skill name unless it's a runtime port like `gsd-opencode`)
- [ ] Installs to `~/.claude/skills/<skill-name>/` cleanly (symlink or copy)

This is the same checklist applied in `.planning/gsd-conformance-notes.md` for this repo — all six items PASS post-patch.

## Required artifacts vs nice-to-haves

| Artifact | Required for upstream PR? | Rationale | Source |
|---|---|---|---|
| `SKILL.md` with valid YAML frontmatter | YES | Mandatory for the skill to load in Claude Code at all. | https://code.claude.com/docs/en/skills |
| Public GitHub repo with permissive license | YES (implicit) | The only existing precedent (`gsd-opencode`) is public + MIT. | https://github.com/rokicool/gsd-opencode |
| `README.md` with install instructions + TÂCHES credit | YES | Reviewer clicks through from the table row; an embarrassing README sinks the PR. | gsd-opencode README, this repo's `README.md` |
| Skill name does NOT start with `gsd-` (unless runtime port) | YES | Avoids implying official-core status. `kdp-book-launch` ✓; the *repo* name `gsd-book-skill` is fine. | Naming convention inferred from `gsd-opencode` (a runtime port, legitimately prefixed) |
| Routes through `.planning/` + GSD slash commands | STRONGLY EXPECTED | Defines the "GSD-aligned" claim; without it, the PR is just promotion. | `.planning/gsd-conformance-notes.md`; upstream README mentions `~/.claude/skills/gsd-*/` install path |
| `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md` in the plugin repo | NICE | Signals maintained-not-abandoned. Already present in this repo. | This repo |
| CI workflows (`.github/workflows/*`) | NICE | Empty `.github/workflows/` is visible but not a blocker for docs-only README enhancement upstream. | `.planning/PROJECT.md` line 26 — deliberately deferred |
| `marketplace.json` / `plugin.json` manifest | NOT REQUIRED | Neither upstream GSD nor the gsd-opencode precedent uses one. Anthropic's skill spec only requires `SKILL.md`. | https://github.com/anthropics/skills; gsd-opencode tree |
| `requirements.txt` / `pyproject.toml` for Python deps | EXPECTED | Reviewer will check that `pip install -r` works. Listed as PR-01 in `.planning/PROJECT.md`. | This repo PROJECT.md |
| `examples/` fixture | EXPECTED | A reviewer will look for a runnable smoke-test artifact. PR-01 in PROJECT.md. | This repo PROJECT.md |
| Real `HTTP-Referer` URL in any OpenRouter callers | EXPECTED | No `local/` placeholders in reviewer-visible text. | This repo PROJECT.md |

## SKILL.md frontmatter conventions

Two fields are **required** by Anthropic's spec; everything else is optional.

```yaml
---
name: kdp-book-launch
description: Use when the user wants to take a manuscript through a full KDP launch — ...
---
```

Validation rules (Confidence: HIGH — primary source is Claude Code docs and Agensi's spec reference):

- **`name`** — 1–64 chars, lowercase alphanumeric with single hyphens, no leading/trailing hyphen, no consecutive hyphens. Regex: `^[a-z0-9]+(-[a-z0-9]+)*$`. Must match the directory name containing `SKILL.md`.
- **`description`** — 1–1024 chars. Best practice: third-person ("Use when the user wants to…"), front-load the trigger conditions, list synonymous trigger phrases. The description is what Claude reads to decide whether to load the skill, so trigger-rich phrasing is load-bearing.

Optional fields seen in the wild: `allowed-tools`, `version`, `author`, `license`. None of these are required by Anthropic's loader; this repo's current SKILL.md uses only `name` + `description`, which is the conservative choice.

Sources:
- https://code.claude.com/docs/en/skills
- https://www.agensi.io/learn/skill-md-format-reference
- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- https://github.com/anthropics/claude-code/blob/main/plugins/plugin-dev/skills/skill-development/SKILL.md

## Community precedent (gsd-opencode)

The single existing Community-table entry. Structure observed via WebFetch on https://github.com/rokicool/gsd-opencode:

- **Distribution model:** `npx gsd-opencode` / `npm install -g gsd-opencode` (it's a runtime port, so it ships as an npm package — not directly applicable to a skill, but useful as a maintained-look signal).
- **README opening:** `"GET SHIT DONE for OpenCode. (Based on TÂCHES v1.38.5 - 2026-04-25)"` — explicit version-pinned attribution to TÂCHES in the very first line.
- **Badges:** npm version, npm downloads, MIT License, GitHub stars.
- **Credit pattern:** Links to `glittercowboy/get-shit-done` as the "TÂCHES Original GitHub Repository" and self-describes as "Adapted for OpenCode by rokicool and enthusiasts."
- **No plugin manifest** — no `marketplace.json`, no `plugin.json`, no `SKILL.md` at the repo root (it's a runtime port, not a Claude Code skill).
- **Top-level layout:** `.bg-shell/`, `.github/workflows/`, `.planning/` (yes, it dogfoods GSD), `assets/`, `gsd-opencode/`, `local/`, `original/`, `reports/`, plus `package.json` and `opencode.json`.

**Takeaway for our PR:** the bar set by precedent is "public repo, MIT-equivalent license, README that opens with TÂCHES attribution, visible maintenance signals (badges or recent commits)." This repo meets all of those.

## Changeset conventions

Source: https://github.com/gsd-build/get-shit-done/blob/main/.changeset/README.md (verified via WebFetch).

**Filename pattern:** `<adjective>-<noun>-<noun>.md` (three random words) to prevent merge conflicts. Some PRs also use `<NNNN>-<slug>.md` or `fix-<NNNN>-<slug>.md` patterns.

**Generation command:**
```
node scripts/changeset/new.cjs \
  --type Fixed \
  --pr 1234 \
  --body "fix the thing — explain the user-visible change in one sentence"
```

**Frontmatter + body format (example fragment):**
```
---
type: Fixed
pr: 1234
---
**`/gsd-foo` no longer drops trailing slashes** — explain the user-visible change.
```

**Allowed `type:` values** (Keep a Changelog convention): `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.

**For our PR** (README Community-table addition):
- README is NOT in the changeset-enforced path list (`bin/`, `get-shit-done/`, `agents/`, `commands/`, `hooks/`, `sdk/src/` per `.planning/gsd-pr-recon.md` §5).
- The official guidance is "when unsure, add the fragment."
- Pragmatic choice: include a fragment with `type: Added`, body something like `**Community: `kdp-book-launch` skill listed** — domain-specific KDP launch skill that drives a 5-phase book-launch runbook via the GSD planning loop.` This avoids requiring the `no-changelog` label, which only a maintainer can apply.

## Confidence levels

| Finding | Confidence | Source |
|---|---|---|
| Upstream Community table is a 2-row markdown table at README §239 | HIGH | WebFetch of upstream README; `.planning/gsd-pr-recon.md` §1 |
| Three PR templates exist: `fix.md`, `enhancement.md`, `feature.md` | HIGH | WebFetch of `.github/PULL_REQUEST_TEMPLATE` directory |
| `approved-enhancement` label is mandatory before PR | HIGH | WebFetch of `enhancement.md` template — quote: "No `approved-enhancement` label on the issue = immediate close." |
| Changeset format is `<adjective>-<noun>-<noun>.md` with `type:`+`pr:` frontmatter | HIGH | WebFetch of `.changeset/README.md` |
| SKILL.md name regex `^[a-z0-9]+(-[a-z0-9]+)*$`, description ≤1024 chars | HIGH | code.claude.com/docs/en/skills, Agensi spec reference |
| No marketplace.json / plugin.json convention required | MEDIUM-HIGH | Anthropic skills repo shows only SKILL.md; gsd-opencode has none; no upstream GSD requirement |
| "GSD-aligned" is a behavioral contract, not a manifest | MEDIUM | No formal definition exists; inferred from CONTRIBUTING.md silence + gsd-opencode precedent + reviewer expectations |
| Other GSD-aligned plugins exist (Superpowers, Shipyard, gsd-milestone-planner) | MEDIUM | WebSearch hits; not independently verified against upstream README — they are NOT listed in the Community table |
| README-only PRs likely qualify for `no-changelog` OR a `type: Added` fragment | MEDIUM | Recon notes §5; safer choice is to include the fragment |
| CI is nice-to-have but not blocking for docs-only enhancement | MEDIUM | Inferred from PROJECT.md decision + scope of the change; reviewer may still ask |

Sources cited:
- https://github.com/gsd-build/get-shit-done/blob/main/README.md
- https://github.com/gsd-build/get-shit-done/blob/main/CONTRIBUTING.md
- https://github.com/gsd-build/get-shit-done/blob/main/.github/PULL_REQUEST_TEMPLATE/enhancement.md
- https://github.com/gsd-build/get-shit-done/blob/main/.changeset/README.md
- https://github.com/rokicool/gsd-opencode
- https://github.com/anthropics/skills
- https://code.claude.com/docs/en/skills
- https://www.agensi.io/learn/skill-md-format-reference
- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- https://thenewstack.io/beating-the-rot-and-getting-stuff-done/
