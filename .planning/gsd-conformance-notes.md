# GSD Conformance Audit — `skill/SKILL.md`

Date: 2026-05-15

## Checklist

| Criterion | Status | Notes |
|---|---|---|
| Uses `.planning/` artifact structure (PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md) | PASS (after patch) | SKILL.md references `.planning/notes/MANUSCRIPT-NOTES.md`, `.planning/kdp-listing.md`, `.planning/launch/*.md` throughout. Post-patch it also explicitly names PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md as the GSD-init artifacts. |
| Routes through GSD's command surface (`/gsd-new-project`, `/gsd-plan-phase`, `/gsd-autonomous`) | PASS (after patch) | Pre-patch: SKILL.md mentioned GSD as the foundation in the header but did not specify which slash commands to use. Post-patch: explicit "Driving this via GSD" paragraph naming the four loop commands plus `/gsd-autonomous`. |
| Trigger description does not conflict with GSD core commands | PASS | Trigger phrases are book-launch-specific ("launch on KDP", "make a book cover", etc.). Zero overlap with GSD's command vocabulary. |
| Installs cleanly to `~/.claude/skills/` | PASS | Already symlinked at `~/.claude/skills/kdp-book-launch/` per the user's setup. README documents the symlink install. |
| Credits TÂCHES + GSD prominently | PASS | Header callout in SKILL.md ("Built on Get Shit Done (GSD) by TÂCHES"). README has a full "⚡ Built on GSD ⚡" section AND a dedicated "Acknowledgments → The skill exists because of GSD" section. |
| Does NOT claim to be a GSD core command | PASS | Skill name is `kdp-book-launch`, not `gsd-*`. Description explicitly positions it as a "domain-specific extension that plugs into" GSD. |

## Edits applied

**File:** `skill/SKILL.md`
**Change:** Added one paragraph in the "5-phase workflow" section explicitly naming the GSD loop commands and the `/gsd-autonomous` entry point.

Diff (conceptual):

```
 ## The 5-phase workflow

 Detailed checklist at `references/phase-checklist.md`. Each phase has author-approval gates. The hard-cost gate is Phase 2 (OpenRouter API spend on chapter images).
+
+**Driving this via GSD:** initialize the project with `/gsd-new-project` (creates `.planning/PROJECT.md`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md` with one roadmap phase per phase below). From there, the loop is `/gsd-discuss-phase N` → `/gsd-plan-phase N` → `/gsd-execute-phase N` → `/gsd-verify-work N` per phase, or `/gsd-autonomous` to run end-to-end with parallel subagent dispatch and atomic per-task commits. The author-approval gates in each phase below are the natural `/gsd-verify-work` checkpoints.
```

## Why this edit and not more

The advisor flagged that the skill was already strong on GSD conformance — the only gap was that `/gsd-autonomous` was named in README.md but not in SKILL.md. SKILL.md is the file Claude loads at runtime; if it doesn't explicitly say "you can drive this via /gsd-autonomous", Claude may default to running each script directly instead of using the GSD loop. The one-paragraph addition closes that gap without bloating the runbook.

**Did not touch:** scripts, templates, references. Their content is fine. The conformance gap was discoverability of the command surface, not the underlying procedure.

## What this conformance buys us for the PR

The PR body can credibly claim:
- "Uses GSD's `.planning/` artifact structure"
- "Routes through `/gsd-new-project` and the discuss/plan/execute/verify loop, plus `/gsd-autonomous` for end-to-end"
- "Trigger phrases avoid all conflicts with GSD core commands"

Each of those is now verifiable in `skill/SKILL.md` lines 36-44.
