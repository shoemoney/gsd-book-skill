---
plan: 01-04
status: complete
---

Populated the previously-empty `examples/` directory with three artifacts that
document the skill's input contract without shipping a sample manuscript:

- `examples/README.md` — orientation, expected project layout, pipeline
  overview, pointers to `skill/SKILL.md` and `skill/references/phase-checklist.md`.
  Explicitly states no sample manuscript is shipped.
- `examples/book_config.json.example` — mirrors
  `skill/templates/book_config.json.template` with placeholder title/author and
  retains the `_comment` annotation pattern. Validates as JSON.
- `examples/launch_config.json.example` — mirrors
  `skill/templates/launch_config.json.template` with 3 sample chapters whose
  `slug` join keys follow the `chNN-name` convention. Validates as JSON.

Reviewer can now answer "how do I use this thing?" from `examples/` alone.
