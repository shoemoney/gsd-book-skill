# Open-Source Prep Tasks

This is the OSS-ification task list for `gsd-book-skill`. The skill works in private already; these tasks make it shareable.

Status legend: `[ ]` not started · `[~]` in progress · `[x]` done

---

## Phase A — Strip and parameterize (P0, blocking)

- [ ] **A1**: Run skill-packaging subagent's output through a scrub pass for personal/proprietary content. Anything Jeremy-Schoemaker-, Brandi-, or Jeremy-Christ-specific that leaked into a generic file gets parameterized.
- [ ] **A2**: Replace hardcoded paths in every script. No `Jeremy-Christ.md`, no `ref_images/jeremy1.png`, no hardcoded slugs in `NO_REF_SLUGS`.
- [ ] **A3**: Each script reads its book-specific config from a single source (e.g., `book.config.json` or env vars), not inline constants.
- [ ] **A4**: All prompts/templates use `{{PLACEHOLDER}}` syntax for substitution. Document every placeholder.
- [ ] **A5**: Verify every script's `--help` runs cleanly without an API key.
- [ ] **A6**: Verify no API keys, OpenRouter referrer URLs, or personal email addresses remain in source.

## Phase B — Repo hygiene (P0, blocking)

- [ ] **B1**: Write `LICENSE` (MIT recommended).
- [ ] **B2**: Write `CONTRIBUTING.md` — how to submit issues, PRs, run tests locally.
- [ ] **B3**: Write `CODE_OF_CONDUCT.md` (Contributor Covenant or similar standard).
- [ ] **B4**: Write `SECURITY.md` — where to report vulnerabilities (esp. for the API-key handling in scripts).
- [ ] **B5**: Write `.gitignore` — `__pycache__/`, `*.pyc`, `.DS_Store`, generated `dist/`, `images/`, `covers/`, `social/`.
- [ ] **B6**: Add `.editorconfig` for consistent formatting across contributors.
- [ ] **B7**: Add a CHANGELOG.md (Keep-a-Changelog format).

## Phase C — Examples + docs (P1)

- [ ] **C1**: Create a generic example walkthrough using a placeholder book ("Demo Book by Sample Author"). Stored in `examples/demo-book/`.
- [ ] **C2**: Write `examples/demo-book/README.md` showing the exact commands to run for each of the 5 phases.
- [ ] **C3**: Add 1-3 sanitized reference images (or instructions for the user to provide their own).
- [ ] **C4**: Record a short Loom or asciinema demo of the workflow end-to-end.
- [ ] **C5**: Document the 3 most common failure modes (model generates hair when prompt says bald; pandoc not installed; KDP rejects upload) with copy-paste recovery steps.

## Phase D — Tests + CI (P1)

- [ ] **D1**: Add `pytest` smoke tests that verify each script parses `--help`.
- [ ] **D2**: Add a "dry-run" mode to scripts that call APIs — exercise the prompt assembly logic without spending money.
- [ ] **D3**: Add GitHub Actions workflow at `.github/workflows/ci.yml` that runs `pytest` + script-parse checks on every PR.
- [ ] **D4**: Add a linting step (`ruff` or `black --check`).

## Phase E — Distribution (P2)

- [ ] **E1**: Decide name — `gsd-book-skill`, `kdp-book-launch`, `book-launch-kit`, or something else. Check name availability on GitHub + PyPI.
- [ ] **E2**: Create the public GitHub repo at the chosen name.
- [ ] **E3**: Push initial commits with full history (this work + skill content).
- [ ] **E4**: Tag `v0.1.0` as the first public release.
- [ ] **E5**: Decide if scripts should also be installable as a `pipx` package; if so, write `pyproject.toml` + entry points.
- [ ] **E6**: Decide if there should be an installable skill registry submission to Anthropic's skill marketplace if/when they open one.

## Phase F — Launch (P2)

- [ ] **F1**: Write a launch announcement blog post on shoemoney.com.
- [ ] **F2**: Cross-post on Hacker News, Indie Hackers, r/SelfPublish.
- [ ] **F3**: Tweet/X thread about it (the author has a built-in audience here).
- [ ] **F4**: Update both production book repos (WinnersWin, JeremyChrist) with a note that they're built using this skill, with a backlink.

## Phase G — Maintenance after launch (P3)

- [ ] **G1**: Triage first 30 days of issues; lock down the issue templates.
- [ ] **G2**: Update Gemini model ID when Gemini 4 ships.
- [ ] **G3**: Add support for alternative image providers (FLUX, Imagen, Sora) so users can swap.
- [ ] **G4**: Add `Black-and-white-only` mode for authors targeting cheaper print pricing.
- [ ] **G5**: Add audiobook (Audible/ACX) workflow as Phase 6.

---

## Pre-flight check before publishing

Don't publish until ALL of these pass:

- [ ] No personal data in any tracked file (`grep -r "shoemoney@\|jeremy@\|Schoemaker" --exclude-dir=.git .` returns nothing in core skill files; mentions are OK in README "acknowledgments" only)
- [ ] No API keys (`grep -r "sk-or-\|OPENROUTER_API_KEY=sk" --exclude-dir=.git .` returns nothing)
- [ ] No proprietary copyright (every file either MIT-licensed by the maintainer or attributed)
- [ ] Demo workflow runs end-to-end on a fresh machine using only public assets
- [ ] AI-disclosure note matches KDP's 2023 policy (covers + chapter illustrations must be disclosed)

---

## Notes

- The Anthropic skill format (YAML frontmatter + markdown body) is the canonical way to ship this for Claude users.
- For non-Claude users, the scripts work standalone — they just don't get the orchestrated workflow.
- Long term: if Anthropic opens a public skills marketplace, submit there too.
