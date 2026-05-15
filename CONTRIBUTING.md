# Contributing to gsd-book-skill

Thanks for your interest in contributing. This project went from "a thing one indie author cobbled together" to "a thing other indie authors can use" because of contributions like yours.

## Quick start

```bash
git clone https://github.com/shoemoney/gsd-book-skill
cd gsd-book-skill

# (Once we have tests in CI)
python3 -m pip install --user pytest ruff
pytest tests/
```

## What we want help with

- **Bug reports** — open an issue describing what you ran, what you expected, what happened. Include OS, Python version, and which script(s) misbehaved.
- **Image-provider adapters** — currently only Google Gemini 3 Pro Image via OpenRouter. Adapters for FLUX, Imagen, Stable Diffusion, DALL-E would broaden the skill.
- **Genre aesthetic libraries** — the included style blocks lean thriller / Dan Brown. Cozy mystery, romance, sci-fi, literary fiction style blocks would help.
- **Non-English language support** — currently the build pipeline assumes English (Garamond, KDP English categories). Non-English locales need different fonts + KDP region settings.
- **KDP alternatives** — Apple Books, Kobo, Draft2Digital, IngramSpark workflow adapters.
- **Audiobook (ACX) Phase 6** — narrator selection, ACX upload prep.
- **Cost optimization** — Gemini regen prompts that hit fewer retries on the first attempt.

## What we don't want

- PRs that introduce new external dependencies without prior discussion. The whole point of this is to be lightweight — Python stdlib + PIL + pypdf + a couple of brew tools.
- PRs that embed your personal API keys or reference images (delete them and re-submit, please).
- Drive-by formatting PRs that change a single line. Bundle them with substance.

## Issue / PR guidelines

- One issue per topic.
- For PRs: ensure tests pass (`pytest`) and lint is clean (`ruff check .`).
- Squash before merge.
- Reference the related issue number in commit messages.

## Code style

- Python: PEP 8, single-quote strings, 4-space indent.
- Markdown: ATX headers (`#`), one sentence per line in long docs is fine but not required.
- Bash invocation inside Python: prefer `subprocess.run` with `check=True` for safety.

## Skill content style

When editing `skill/SKILL.md`:

- Keep the YAML frontmatter's `description:` ≤ 1024 chars and SPECIFIC. This is what Claude uses to decide when to load the skill.
- Use H2/H3 structure with short sections. Claude reads top-down; front-load the most important info.
- Avoid example output that's tied to a specific real book. Use `{{PLACEHOLDERS}}` or fictional book names.

## Code of Conduct

Be kind. We're all here trying to ship books. The full code of conduct is in [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md).
