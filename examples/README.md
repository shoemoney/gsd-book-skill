# examples/

This directory documents the **input contract** for the `kdp-book-launch` skill.
Drop your own manuscript and configs into a project, then run the skill via
`/gsd-new-project` or invoke the scripts under `skill/scripts/` directly.

We do **NOT** ship a sample manuscript. Use your own text.

## Expected project layout

The skill expects a project (your repo, not this one) shaped like this:

```
my-book/
  Manuscript.md            # your full manuscript, H2 per chapter
  chapters/                # OR split per-chapter files
    01-introduction.md
    02-chapter-name.md
    ...
  ref_images/              # protagonist / character reference images
  book_config.json         # build config (kdp-book-generator)
  launch_config.json       # launch-script config (this skill)
```

Two starter configs live here:

- [`book_config.json.example`](./book_config.json.example) — mirrors
  `../skill/templates/book_config.json.template`. Controls EPUB/PDF build
  (typography, page size, margins, metadata).
- [`launch_config.json.example`](./launch_config.json.example) — mirrors
  `../skill/templates/launch_config.json.template`. Drives the launch pipeline
  (chapters, protagonist refs, cover, social pack, PDF metadata). The `slug`
  field is the join key between `chapters/NN-slug.md`, `prompts/chapters/`, and
  `images/chapters/`.

Copy each `*.example` to your project root, drop the `.example` suffix, and
fill in the placeholder values.

## Pipeline overview

The skill runs a 5-phase pipeline with `[GATE]` checkpoints between phases:

1. Manuscript Lock
2. Chapter Illustrations
3. Cover Production
4. Build & Compile
5. KDP Launch Prep

See [`../skill/SKILL.md`](../skill/SKILL.md) for the canonical workflow and
[`../skill/references/phase-checklist.md`](../skill/references/phase-checklist.md)
for the detailed runbook.
