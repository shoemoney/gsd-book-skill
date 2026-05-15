---
generated: 2026-05-15
focus: concerns
---

# Concerns — `gsd-book-skill`

Tech-debt / risk / fragility audit. ~39 files, mostly `skill/scripts/*.py` plus
markdown docs. Maintainer has already done a self-audit in
`.planning/gsd-conformance-notes.md` and `.planning/gsd-pr-recon.md`; this
report focuses on what's *not* already flagged there.

## What the maintainer already knows

The maintainer has explicitly resolved (per `.planning/gsd-conformance-notes.md`):
- GSD `.planning/` artifact references in `skill/SKILL.md` — PASS post-patch.
- GSD command surface (`/gsd-new-project`, `/gsd-discuss-phase`, etc.) named in
  SKILL.md — PASS post-patch.
- Trigger-phrase collision with GSD core commands — PASS (book-launch only).
- TÂCHES / GSD credit — PASS (prominent in `README.md` and `skill/SKILL.md:10`).

Per `.planning/gsd-pr-recon.md` they also know upstream wants:
- Conventional commits with issue numbers (`docs(NNNN):`) — repo currently uses
  un-numbered `docs(plugin):` style commits, fine for own repo but will need
  reformatting for upstream PR.
- Discussion-first gating before any PR — already staged in
  `.planning/gsd-pr/STEP1_DISCUSSION_POST.md`.

The concerns below are things the conformance notes do NOT call out.

---

## 🔴 High — blocks usability or merge

1. **No `requirements.txt` / `pyproject.toml` / dependency lock.** Scripts import
   `PIL` (`skill/scripts/build_social_pack.py:37`,
   `skill/scripts/build_visual_companion.py:30`,
   `skill/scripts/compose_cover_wrap.py:34`) and `pypdf`
   (`skill/scripts/postprocess.py:22`) with no pin file anywhere in the repo.
   `postprocess.py:10` says "Note: this system uses Python's PEP 668 protection.
   Use pipx or a venv" but never tells users *what to install*. First-run UX is
   a `ModuleNotFoundError` cascade. Minimum fix: add a top-level
   `requirements.txt` (Pillow, pypdf) and a one-liner in `README.md` / `SKILL.md`
   install section.

2. **External-tool dependencies are undocumented at the top level.** Scripts
   shell out to `pandoc` (`skill/scripts/build_print_pdf.py:162`), headless
   Chrome (`build_print_pdf.py:181`), and ImageMagick `magick`
   (`skill/scripts/make_bw_variants.py:34`). Each script's docstring mentions
   its own tool, but the README install section does not list the full external
   toolchain. A new user following only `README.md` will hit
   `FileNotFoundError: pandoc` mid-phase-4.

3. **Hardcoded placeholder URL leaks into OpenRouter analytics.** Every API call
   ships `"HTTP-Referer": "https://github.com/local/kdp-book-launch"` —
   `skill/scripts/generate_chapter_images.py:80`,
   `skill/scripts/generate_front_cover.py:108`,
   `skill/scripts/generate_back_cover.py:108`. This is a placeholder string, not
   the real repo URL. Either point it at the real repo
   (`https://github.com/shoemoney/gsd-book-skill`) or make it configurable from
   `_launch_config.json`. As-is, all upstream OpenRouter dashboards show the
   skill as `local/kdp-book-launch`, which is both wrong and slightly
   misleading.

## 🟡 Medium — works but fragile

4. **Broad `except Exception` in image-gen retry loop** at
   `skill/scripts/generate_chapter_images.py:181` swallows every non-HTTP error
   (incl. `KeyboardInterrupt`-adjacent `SystemExit` is fine since that's
   `BaseException`, but `MemoryError`, programming errors, etc. all get
   coerced into "retry"). For a hard-cost loop ($0.14/image × 3 attempts) a
   programming bug means triple-billing. Narrow to `(urllib.error.URLError,
   json.JSONDecodeError, KeyError, RuntimeError)`.
   Same pattern at `skill/scripts/build_social_pack.py:417` re-raises after
   logging, which is fine; the silent `except Exception:` at
   `build_social_pack.py:443` (image-open in readme generation) is acceptable
   since size is cosmetic.

5. **macOS-first Chrome path discovery, Linux/Windows untested.**
   `skill/scripts/build_print_pdf.py:38-44` lists four Chrome paths — three
   Linux, two macOS, **zero Windows**. The script's docstring at
   `build_print_pdf.py:15` admits "this script defaults to macOS Chrome path."
   This is fine for the maintainer but every Windows user is dead on arrival
   for Phase 4 unless they know to set `$CHROME_BIN`. Add a Windows path
   (`C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe`) and surface
   the `$CHROME_BIN` knob in `README.md`.

6. **No protagonist-likeness / NSFW / copyright guardrails in image-gen.** The
   skill happily forwards arbitrary prompts and reference photos to
   `google/gemini-3-pro-image-preview` via OpenRouter. No prompt scrubbing, no
   refusal detection, no content-policy check on the response. The likeness
   methodology at `skill/references/likeness-audit-methodology.md` is purely a
   *post-hoc human audit* — nothing programmatic. Risk surfaces:
   - User uploads ref photos of a real person without consent → deepfake
     liability. README/SKILL.md never says "use your own photos or
     photos you have rights to."
   - User generates trademark/IP-violating cover art → KDP rejects on upload.
   - OpenRouter content-policy refusal returns a structured error;
     `generate_chapter_images.py:91-96` will raise `RuntimeError("No images in
     response.")` and the user has to read the raw JSON to figure out why.
     Surface the refusal reason explicitly.
   At minimum: add a short "Ethics / consent" note to `SKILL.md` and the README
   stating that the user is responsible for image rights and likenesses.

7. **`pyproject.toml`/`pytest`/CI absent.** No `.github/workflows/`,
   no linter config, no tests, no CHANGELOG-generation tooling. Fine for a
   solo-maintained skill, but `CONTRIBUTING.md` exists and implies external
   contributors — they have nothing to validate against. Either add a minimal
   `ruff` + smoke-test workflow or trim `CONTRIBUTING.md` to reflect the actual
   contribution surface.

8. **`_config.py` uses `sys.exit()` for missing config.** Every consumer script
   `from _config import ...` and any miss raises `SystemExit`
   (`skill/scripts/_config.py:32`, `:39`, `:46`). Fine for CLI use but makes the
   helpers unusable as a library and untestable. Raise a custom
   `ConfigError` and let `main()` translate to exit code.

9. **`tempfile.NamedTemporaryFile(... delete=False)` cleanup is best-effort.**
   `skill/scripts/build_print_pdf.py:176-194` writes the temp HTML next to the
   project root (good — Chrome needs file:// siblings for image resolution) and
   deletes in `finally`, swallowing `OSError`. If Chrome leaves an orphaned
   process the `.html` lingers in the project dir. Use a unique prefix
   (e.g. `f"_kdp_build_{os.getpid()}_"`) so users can `rm _kdp_build_*.html` if
   they accumulate.

10. **Chrome runs with `--no-sandbox`** at `build_print_pdf.py:185`. Necessary
    on some Linux distros / Docker, but on macOS it's a footgun — it disables
    process isolation while rendering arbitrary book HTML. Practically
    low-risk because the HTML is locally-generated, but worth a comment
    explaining why or making it conditional on Linux.

## 🟢 Low — nice-to-have polish

11. **SKILL.md is 428 lines / ~24 KB.** Skill descriptions don't have a hard
    cap, but per Anthropic skill conventions, brevity matters for context
    window. The 5-phase runbook is the bulk; consider moving phase detail
    fully into `references/phase-checklist.md` and keeping `SKILL.md` as the
    orchestration layer. Frontmatter `description:` is ~485 chars, well within
    the 1024-char skill description limit — that's fine.

12. **`__pycache__/` is committed.** Visible at
    `skill/scripts/__pycache__/`. `.gitignore` lists `__pycache__/` but it was
    presumably committed before the ignore was added. Run `git rm -r
    --cached skill/scripts/__pycache__/`.

13. **No `pdf_metadata.producer` provenance for the launch skill itself.**
    `skill/scripts/postprocess.py:41` writes `"/Producer":
    "kdp-book-generator + kdp-book-launch"` — fine, but the skill repo is
    `gsd-book-skill`, not `kdp-book-launch`. Mild naming drift between the
    skill name (`kdp-book-launch` in SKILL.md frontmatter), the repo name
    (`gsd-book-skill`), and the producer string. Pick one canonical name and
    use it everywhere.

14. **`templates/launch_config.json.template` has `"isbn": "OPTIONAL - leave
    blank for KDP-assigned"` as the *value*.** That string will pass through
    JSON parsing and get embedded as the literal ISBN in any PDF metadata
    written by `postprocess.py` unless the user edits it. Use an empty string
    or `null` plus a `_comment` key (which `_config.py:_strip_comments` already
    handles).

15. **No KDP trim-size validation.** `compose_cover_wrap.py:12` hardcodes 6×9
    trim; `kdp-specifications.md:112` documents other sizes (5×8, 5.5×8.5,
    etc.) but the wrap composer has no `--trim` arg and no config validation.
    A user who sets `trim_in: [5, 8]` in their config still gets a 6×9 wrap.
    Either thread trim from config or fail-fast.

16. **`make_bw_variants.py` requires ImageMagick 7+ `magick` binary**
    (`make_bw_variants.py:34`). On older systems `convert` is the binary name.
    Detect and fall back, or document the minimum version in the script's
    install note.

---

## Summary

The maintainer has been thorough on GSD conformance and upstream PR mechanics.
The real concerns this audit surfaces are:
- **Dependency / toolchain discoverability** (items 1, 2, 7) — biggest hit to
  first-run UX.
- **Ethics & content guardrails** (item 6) — only programmatic safety net is a
  *post-hoc human audit*; consent/likeness/copyright caveats are absent from
  user-facing docs.
- **Cross-platform fragility** (items 5, 10, 16) — macOS-centric assumptions
  leak through Chrome paths and tool names.
- **Naming drift** between `kdp-book-launch`, `gsd-book-skill`, and
  `local/kdp-book-launch` (items 3, 13) — cosmetic but visible to OpenRouter
  and to anyone reading the producer metadata of finished PDFs.

None of the above block the planned upstream Path-2 community-table PR, which
is a one-line README addition to `gsd-build/get-shit-done`. They do affect
downstream-user experience once the skill gains traction.
