# README Diff for gsd-build/get-shit-done

This is the entire diff for the PR. **One file. One row added. No other changes.**

## Target file

`README.md` (the English-default README — not the localized variants)

## Insertion point

Section `## Community`, line 239+ in the current `main` HEAD (`a7f0af2c`). The new row goes BETWEEN the existing `gsd-opencode` row and the `Discord` row. This positions third-party projects together and keeps the Discord chat-link as the last entry (matches the original ordering intent).

## Unified diff

```diff
--- a/README.md
+++ b/README.md
@@ -240,6 +240,7 @@
 | Project | Platform |
 |---------|----------|
 | [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
+| [gsd-book-skill](https://github.com/shoemoney/gsd-book-skill) | Claude Code skill — KDP book launch pipeline |
 | [Discord](https://discord.gg/mYgfVNfA2r) | Community support |
```

## What the inserted row says, parsed

| Cell | Value | Why this exact value |
|---|---|---|
| Project (link text) | `gsd-book-skill` | Matches the repo name, follows the existing `gsd-*` prefix convention set by `gsd-opencode` |
| Project (link target) | `https://github.com/shoemoney/gsd-book-skill` | Public MIT-licensed repo, owned by Jeremy Schoemaker (`@shoemoney`) |
| Platform | `Claude Code skill — KDP book launch pipeline` | Matches the column's loose semantics (gsd-opencode says "Original OpenCode port", Discord says "Community support" — both describe the entry's *kind*, not literally a platform). Our description names the runtime (Claude Code) and the domain (KDP book launch) in the fewest words possible. |

## What this PR does NOT touch

- `README.pt-BR.md`, `README.zh-CN.md`, `README.ja-JP.md`, `README.ko-KR.md` — localized variants. If the maintainer wants these synced, that should be a separate PR (per their "one concern per PR" rule). Mention this in the PR body if asked.
- Any code, agent, command, workflow, or test file.
- `CHANGELOG.md` — never edited directly per their changeset policy. README is not in the enforced-changeset path list, so request `no-changelog` label.
- `docs/` — no doc updates needed for a community-listing addition.

## Verifying the diff before commit

```bash
# After applying the edit:
git diff --stat
# Expected: 1 file changed, 1 insertion(+), 0 deletions(-)

git diff README.md
# Expected: exactly the unified diff above
```

If `git diff --stat` shows anything else, you've drifted from the approved scope — revert and re-apply just the one row.
