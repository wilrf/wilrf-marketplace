---
description: "Simplify the entire codebase module-by-module with auto-commits and resume support"
---

# Simplify Codebase

Orchestrate a full-codebase simplification sweep using `/simplify` on each module.

## Process

1. **Pre-flight** — Verify clean working tree, check for existing progress file
2. **Discover modules** — Scan `src/` subdirectories, add `tests/` last
3. **Discover test command** — Read from CLAUDE.md, pyproject.toml, package.json, or Makefile
4. **Write progress file** — Create `simplify-progress.md` with module list
5. **Module loop** — For each pending module:
   a. Mark in progress
   b. Invoke `/simplify` scoped to the module
   c. Run tests
   d. If pass → auto-commit → update progress → next
   e. If fail → revert → log → skip → next
6. **Complete** — Summary table, delete progress file, final test run

## Resume

If `simplify-progress.md` exists, read it and continue from the next pending module. If a module is marked `in progress` with a clean working tree, treat as pending. If dirty, revert and retry.

## Auto-Commit Format

```
refactor: simplify <module-path>

<bullet summary of changes>

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Edge Cases

- **Dirty working tree** — Abort, ask user to commit or stash
- **20+ files in module** — Split into ~10-file sub-batches
- **No test command found** — Warn and ask user
- **All modules done** — Report completion, delete progress file
