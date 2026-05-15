# gsd-book-skill

> Take a markdown manuscript through a complete Amazon KDP launch — editorial review, AI-generated chapter illustrations, cover art with title baked in, EPUB + paperback + hardcover builds, full cover wraps, bonus PDFs, social media graphic pack, and launch collateral — all from your terminal.

A reusable [Claude skill](https://docs.claude.com/en/docs/claude-code/skills) plus a kit of standalone Python scripts. Originally extracted from the production launch of two books (*Why Winners Win* and *Jeremy Christ*) by Jeremy Schoemaker.

---

## ⚡ Built on GSD ⚡

**This entire workflow runs on top of [Get Shit Done (GSD)](https://github.com/gsd-build/get-shit-done) by [TÂCHES](https://github.com/gsd-build).**

GSD is a meta-prompting, context engineering, and spec-driven development system for Claude Code, OpenCode, Gemini CLI, Codex, Copilot, Cursor, Windsurf, and more. It provides the framework that makes book-launch-as-a-skill actually work:

- `/gsd-new-project` — initializes the `.planning/` directory with PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md
- `/gsd-discuss-phase` → `/gsd-plan-phase` → `/gsd-execute-phase` → `/gsd-verify-work` → `/gsd-ship` — the five-command loop that drives every phase
- `/gsd-autonomous` — parallel subagent dispatch with fresh 200k-token contexts and atomic commits per task
- The agent orchestration, state management, and atomic-commit guarantees that make a 35-commit autonomous run actually safe

**If you find this skill useful, the credit goes upstream.** Install GSD first:

```bash
npx get-shit-done-cc@latest
```

Then drop this skill in alongside it. GSD makes Claude Code productive; this skill teaches it to ship books.

Real talk: I tried to build a launch pipeline without a framework first. It was a mess. GSD gave me the bones — phases, gates, planning artifacts, subagent dispatch — and this skill just specializes those bones for the book-launch domain. **All the hard architectural work is TÂCHES's.** Star [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done), pay them in attention, and consider sponsoring if it ships your stuff.

---

## What this is

A complete pipeline that takes you from `manuscript.md` to **KDP-upload-ready files** in five phases:

| Phase | What happens | What you get |
|------:|--------------|--------------|
| 1 | **Manuscript Lock** — editorial pass, continuity audit, family/character canon, structural cleanup, religion/voice audit | Locked manuscript at agreed final state |
| 2 | **Chapter Illustrations** — one AI image per chapter, consistent likeness, color + B&W variants | `images/chapters/*.png` (color for ebook) + `images/chapters-bw/*.png` (print) |
| 3 | **Cover Production** — front cover (title + tagline + author credits baked in by AI), back cover (full blurb baked in), spine | `covers/front-cover.jpg`, `covers/back-cover.jpg` |
| 4 | **Build & Compile** — KDP EPUB (validated by EPUBCheck), paperback PDF, hardcover PDF, full cover wrap PDFs at KDP dimensions, bonus visual companion PDF | `dist/*.epub`, `dist/*-paperback.pdf`, `dist/*-paperback-wrap.pdf`, etc. |
| 5 | **Launch Prep** — KDP listing copy, press release, subscriber email, blog post, Goodreads listing, A+ Content, Amazon Ads, social media pack | `social/` (~54 graphics) + `.planning/launch/*.md` |

The skill is **genre-agnostic** — works for thrillers, memoirs, self-help, fiction, nonfiction. The aesthetic templates (Da Vinci Code / Roger Deakins / etc.) are swappable.

## Who this is for

- Indie authors using [Claude Code](https://claude.com/claude-code) or the Claude API
- Writers who have a finished manuscript and want to ship to KDP themselves
- Anyone tired of paying $3,000+ for a cover designer and $1,500+ for an editor when AI can do the first 80% and you can iterate on the last 20%

## What it costs

| Component | Where it spends | Per book |
|-----------|----------------|---------|
| Chapter illustrations | OpenRouter Gemini 3 Pro Image | ~$0.14/image × N chapters = **$3–5** |
| Cover art (front + back) | OpenRouter Gemini 3 Pro Image | ~$0.30 |
| Editorial pass (subagent) | Anthropic API / Claude Code allowance | Free if you have Pro/Max, otherwise ~$2–5 |
| Build pipeline | Local (free) | $0 |
| Social media pack | Local (PIL only) | $0 |
| Press release / launch collateral | Anthropic API / Claude Code allowance | Free if you have Pro/Max |

**Total OpenRouter spend for a 21-chapter book: ~$4.** (Verified production cost.)

## Repo layout

```
gsd-book-skill/
├── skill/                ← the installable Claude skill (this is what you symlink)
│   ├── SKILL.md          ← runbook Claude loads when triggered
│   ├── scripts/          ← Python toolkit (11 scripts + 1 shared loader)
│   ├── templates/        ← prompt + config templates with {{PLACEHOLDERS}}
│   └── references/       ← methodology docs
├── examples/             ← (planned) generic walkthroughs
└── ... README, LICENSE, CONTRIBUTING, etc.
```

The entire `skill/` folder is the installable unit. Symlink it to `~/.claude/skills/kdp-book-launch/` and Claude will pick it up.

## What the skill provides

**Skill content** at `skill/SKILL.md` — Claude loads this when you ask it to launch a book on KDP. Walks Claude through the 5-phase workflow with clear gates for user decisions.

**Scripts** at `skill/scripts/`:

| Script | Purpose |
|--------|---------|
| `generate_chapter_images.py` | OpenRouter Gemini 3 Pro Image generator with multi-ref support (`NO_REF_SLUGS`, `STRONG_REFS`, `CUSTOM_REFS`) |
| `generate_front_cover.py` | Front cover with title text baked in |
| `generate_back_cover.py` | Back cover with full blurb baked in |
| `make_bw_variants.py` | Color → B&W variants for print interior (ImageMagick) |
| `build_visual_companion.py` | Bonus PDF assembly (PIL) |
| `build_book_md.py` | Manuscript → kdp-book-generator input, with `--print` flag for B&W image swap |
| `epub_embed_images.py` | Bundle image files into KDP-built EPUBs (KDP's tool doesn't do this) |
| `build_print_pdf.py` | Pandoc → Chrome headless → PDF fallback for the print build |
| `compose_cover_wrap.py` | Full paperback + hardcover wrap PDFs at KDP dimensions |
| `build_social_pack.py` | 50+ social media graphics (PIL only, no API spend) |

**Templates** at `skill/templates/` — prompt templates (chapter image / cover / editorial / KDP listing) that you fill in for your book.

**References** at `skill/references/` — methodology docs that explain how and why:

- `editorial-review-methodology.md` — HIGH/MEDIUM/LOW severity classification
- `likeness-audit-methodology.md` — auditing AI image consistency against ref photos
- `canon-audit-methodology.md` — family canon and religion canon audits
- `cover-regen-troubleshooting.md` — recovering when the model produces hair on a bald protagonist, etc.
- `kdp-specifications.md` — trim sizes, spine widths, bleed dimensions
- `prompt-structure-guide.md` — writing image prompts that work
- `aesthetic-libraries.md` — style blocks for different genres
- `phase-checklist.md` — copy-paste task list for the 5 phases

**Examples** at `examples/` — generic walkthroughs using a placeholder demo book (no real manuscripts included).

## Getting started

```bash
# Clone the repo
git clone https://github.com/shoemoney/gsd-book-skill ~/Projects/gsd-book-skill

# Install the skill at the user level via symlink
ln -s ~/Projects/gsd-book-skill/skill ~/.claude/skills/kdp-book-launch

# Verify Claude can see it
ls ~/.claude/skills/kdp-book-launch/      # should list SKILL.md + scripts/ + templates/ + references/

# In your book project, start the workflow
cd ~/Projects/my-book

# Then in Claude Code:
#   "Launch my book on KDP"
# Claude will load this skill and walk you through the 5 phases.
```

## Requirements

**OS:** macOS or Linux. Developed on macOS; should work on Linux with the same brew/apt tooling. Windows is not currently supported (some scripts hardcode macOS font paths).

**Python 3.10+** with these libraries:

```bash
pip install -r requirements.txt
```

(`Pillow` and `pypdf`. See [`requirements.txt`](./requirements.txt) and [`pyproject.toml`](./pyproject.toml) for exact versions. On systems with PEP 668 enforcement, use `pipx` or a virtualenv.)

**External CLIs** (must be on `$PATH`):

- **[pandoc](https://pandoc.org/installing.html)** — Markdown → HTML conversion (used by `build_print_pdf.py`)
- **headless Chrome or Chromium** — HTML → PDF rendering (used by `build_print_pdf.py`). On macOS: `/Applications/Google Chrome.app/...`. On Linux: `chromium` or `google-chrome`.
- **[ImageMagick 7+](https://imagemagick.org/script/download.php)** — provides the `magick` binary used by `compose_cover_wrap.py` for cover assembly.
- **[epubcheck](https://www.w3.org/publishing/epubcheck/)** — EPUB validator (`brew install epubcheck` on macOS).
- **Node.js + `npx`** — required to invoke [`kdp-book-generator`](https://www.npmjs.com/package/kdp-book-generator), the separate Node CLI this skill orchestrates for EPUB packaging.

**API key** (for cover/chapter image generation):

- `OPENROUTER_API_KEY` — used by `generate_front_cover.py`, `generate_back_cover.py`, and `generate_chapter_images.py` to call `gemini-3-pro-image-preview` via OpenRouter.

**Inputs you supply:**

- Your manuscript (as `chapters/NN-slug.md` files; see [`examples/`](./examples/))
- One or more reference photos of the author (or the cover model) if any chapter scenes feature them — passed to the image-gen prompts for likeness anchoring

## License

MIT — see [LICENSE](./LICENSE).

## Roadmap & contributing

See [TASKS.md](./TASKS.md) for the OSS-prep task list and roadmap items.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## Acknowledgments

### The skill exists because of GSD

**[Get Shit Done (GSD)](https://github.com/gsd-build/get-shit-done) by [TÂCHES](https://github.com/gsd-build)** is the meta-framework this skill plugs into. Every phase, every `/gsd-*` command, every atomic commit, every parallel subagent dispatch — all GSD's design. This skill is a domain-specific extension; GSD does the heavy architectural lifting.

If this skill saves you time shipping a book, the upstream debt is owed to TÂCHES. Star the repo. Sponsor the project. Tell other authors. GSD is the kind of infrastructure that disappears into the background once it works — which is exactly why it deserves louder credit.

- **GSD repo**: https://github.com/gsd-build/get-shit-done
- **GSD on npm**: `npx get-shit-done-cc@latest`
- **Author**: TÂCHES — [@gsd-build](https://github.com/gsd-build) on GitHub

### Other shoulders standing on

- [Anthropic Claude Code](https://claude.com/claude-code) — the skill system + agent dispatch runtime
- [Google Gemini 3 Pro Image](https://deepmind.google/technologies/gemini/) via [OpenRouter](https://openrouter.ai) — image generation
- [`kdp-book-generator`](https://www.npmjs.com/package/kdp-book-generator) — markdown → KDP PDF/EPUB
- [Pillow (PIL)](https://pillow.readthedocs.io), [pypdf](https://pypdf.readthedocs.io), ImageMagick, pandoc, EPUBCheck — the unsung trio that turns ideas into print files

### Reference implementation

See the `JeremyChrist` book project (private) for a complete end-to-end production run that produced this skill — 35+ commits, 21 chapter illustrations, full EPUB + paperback + hardcover builds, 54 social media graphics, launch collateral, all driven by `/gsd-autonomous`.
