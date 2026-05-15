---
generated: 2026-05-15
focus: quality
---

# Code Conventions

Observed by reading `skill/scripts/_config.py`, `skill/scripts/build_book_md.py`,
`skill/scripts/build_print_pdf.py`, `skill/scripts/postprocess.py`,
`skill/SKILL.md`, `skill/references/*.md`, and `skill/templates/*.template`.

## Python style

- **Target:** Python 3.10+ (uses PEP 604 unions like `str | None`, `from __future__ import annotations`). Explicit reminder in `skill/scripts/postprocess.py` header: "Python 3.10+ ... pipx or venv (PEP 668)."
- **Shebang + docstring:** Every executable script opens with `#!/usr/bin/env python3` followed by a triple-quoted module docstring that documents purpose, pipeline steps, required external tools, and `Usage:` examples. See `skill/scripts/build_print_pdf.py` lines 1-25.
- **Imports:** `from __future__ import annotations` first, then stdlib, then third-party (`pypdf` in `postprocess.py`), then the local `_config` import. Local sibling import uses an explicit `sys.path` insert so scripts work when run as `python3 path/to/script.py`:
  ```python
  sys.path.insert(0, str(Path(__file__).resolve().parent))
  from _config import add_config_arg, load_config, project_root  # noqa: E402
  ```
- **Paths:** Always `pathlib.Path`, never `os.path.join`. Resolve early with `.expanduser().resolve()`.
- **Type hints:** Used on public functions (`resolve_image_path(slug: str, root: Path, ...) -> str | None`), often omitted on small internal helpers.

## Naming

- **Files:** `snake_case.py`. Shared/private modules prefixed with underscore: `_config.py`.
- **Functions:** `snake_case`, verbs (`find_config`, `load_config`, `make_front_matter`, `insert_image`).
- **Constants:** `UPPER_SNAKE_CASE` at module level (`DEFAULT_CONFIG_NAME`, `DEFAULT_CHROME_PATHS`, `CSS`, `HTML_WRAPPER` in `build_print_pdf.py`).
- **Private helpers:** Leading underscore (`_strip_comments`).

## CLI / argparse pattern

Every user-facing script in `skill/scripts/` follows the same template:

```python
def main():
    ap = argparse.ArgumentParser(description=__doc__.split("\n\n")[0])
    ap.add_argument(...)
    add_config_arg(ap)              # injects --config flag
    args = ap.parse_args()
    cfg = load_config(args.config)
    ...

if __name__ == "__main__":
    main()
```

`description` reuses the first paragraph of the module docstring. Custom flags use kebab-case names with snake_case `dest` (`--print` → `print_mode`).

## Configuration loading

Single canonical pattern centralized in `skill/scripts/_config.py`:

1. Resolution order: `--config` arg → `$KDP_LAUNCH_CONFIG` env var → `./_launch_config.json` in CWD.
2. JSON is parsed with `json.load`, then `_strip_comments` walks the tree removing any `"_comment"` keys so the template can carry inline notes without breaking strict consumers.
3. `project_root()` is always the directory containing the resolved config — every relative `paths.*` in the config is interpreted relative to that root.
4. Scripts read `cfg["paths"]["..."]` for input/output locations and `cfg["chapters"]` for per-chapter metadata. Templates document required and optional keys (`skill/templates/launch_config.json.template`).

## Error handling

- **Fail fast with `sys.exit(message)`.** No custom exception classes, no traceback gymnastics. Examples in `_config.py`: `sys.exit(f"config not found: {p}")`, `sys.exit(f"no config found. Looked for ...")`.
- **Warnings, not errors, for recoverable cases:** `print(f"  WARN: no image for {slug}", file=sys.stderr)` in `build_book_md.py` — script continues, user sees it on stderr.
- **External-tool failures** rely on `subprocess.run(..., check=True)` to raise `CalledProcessError`. Headless Chrome stdout/stderr are silenced (`DEVNULL`) in `build_print_pdf.py`.
- **Cleanup with `try/finally`** for temp files (the temp HTML in `build_print_pdf.py` is unlinked even on Chrome failure).

## Comment / docstring practices

- Module docstrings are long-form and tutorial-style: explain *why*, list pipeline steps, name external dependencies, include `Usage:` block. They double as `--help` text via `description=__doc__.split("\n\n")[0]`.
- Inline comments are sparse and reserved for non-obvious regex intent ("Pattern A: header + optional blank + ### location block + trailing blank" in `build_book_md.py`).
- Function docstrings are short or omitted when the name + signature carry the meaning.

## Markdown conventions (`skill/SKILL.md`, `skill/references/*.md`)

- **YAML frontmatter only on `SKILL.md`** (`name:` + `description:` — Anthropic skill manifest format). References are plain markdown, no frontmatter.
- **ATX headings** (`#`, `##`, `###`). H1 reserved for document title.
- **Horizontal rules (`---`) as section separators** between major chunks of a reference doc.
- **Hard-wrapped prose** around 65-72 columns in references (`canon-audit-methodology.md`, `likeness-audit-methodology.md`); SKILL.md uses longer lines.
- **Bullet style:** `-` for lists. Bold-prefixed labels: `**HIGH** — Must be fixed before launch.`
- **Code fences are language-tagged** (` ```python `, ` ```json `).

## Template conventions (`skill/templates/*.template`)

- **Placeholder syntax:** `{ALL_CAPS_PLACEHOLDER}` — not Jinja, not Mustache, not `${}`. Filled by humans (or by Claude editing the file in place), never by Python `str.format()`. Examples: `{BOOK TITLE}`, `{PROTAGONIST_NAME}`, `{PRONOUN_POSSESSIVE}` in `chapter-image-prompt.txt.template`; `{TITLE}`, `{AUTHOR}` in `kdp-listing.md.template`.
- **JSON templates** (`launch_config.json.template`, `book_config.json.template`) carry a top-level `"_comment"` documenting copy/edit instructions, which `_config._strip_comments` removes at load time. This lets the template be a working JSON file that doubles as inline docs.
- **`.template` suffix** signals "copy this, then edit" — copies are gitignored at the project (consumer) level, not here.
