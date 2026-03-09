---
name: simplify-codebase
description: >-
  This skill should be used when the user asks to "simplify the whole codebase",
  "simplify all files", "codebase-wide cleanup", "simplify everything",
  "run simplify on the entire project", or "clean up the whole repo".
  Orchestrates module-by-module /simplify sweeps with auto-commits and
  cross-session resume.
---

# Codebase-Wide Simplify

Orchestrate a full-codebase simplification sweep by breaking the project into
logical modules and running `/simplify` on each one sequentially.

## Key Behaviors

- Auto-commit after each module passes tests
- Resume from where the last session left off
- Revert and skip modules whose tests fail
- Track progress in `simplify-progress.md` at the project root

---

## Phase 1: Initialize

### Pre-Flight Checks

1. Run `git status --porcelain`. If output is non-empty, abort with:
   "Working tree has uncommitted changes. Commit or stash before running
   simplify-codebase."
2. Check for `simplify-progress.md` in the project root.
   - If it exists and all modules are marked `[x]`, report completion,
     delete the file, and stop.
   - If it exists with pending modules, skip to **Phase 2: Execute**.
   - If it does not exist, continue to discovery.

### Discover Modules

Scan for source directories to build the module list:

1. If `src/` exists, list its immediate subdirectories as modules.
2. Otherwise, scan the project root for directories containing 3+ source
   files (`.py`, `.ts`, `.js`, `.go`, `.rs`, `.java`).
3. Add `tests/` as the final module (simplified last, after source changes).
4. If any module contains 20+ source files, split it into sub-batches of
   ~10 files each, grouped by subdirectory or alphabetically.

### Discover Test Command

Resolve the test command in priority order:

1. **CLAUDE.md** — look for a "Commands" or "Common Commands" section
   containing a `pytest`, `npm test`, `cargo test`, or similar command.
2. **pyproject.toml** — check `[tool.pytest]` or `[tool.hatch]` sections.
3. **package.json** — check `scripts.test`.
4. **Makefile** — check for a `test` target.
5. If none found, warn and ask the user for the test command.

### Write Progress File

Create `simplify-progress.md` at the project root with the discovered
module list and test command:

```
# Codebase Simplify Progress
Started: YYYY-MM-DD
Test command: <discovered command>

## Modules
- [ ] src/data/ — pending
- [ ] src/models/ — pending
- [ ] src/features/ — pending
- [ ] tests/ — pending

## Findings Log
```

---

## Phase 2: Execute Module Loop

For each module marked `pending` or `in progress` in `simplify-progress.md`:

### Step 1: Mark In Progress

Update the module line in the progress file from `pending` to `in progress`.

### Step 2: Invoke /simplify

Invoke the `/simplify` skill scoped to the current module:

```
/simplify Focus on <module-path>. Review all source files in this directory
for code reuse, quality, and efficiency issues. Fix any issues found.
```

This delegates the three-agent review (reuse, quality, efficiency) to the
existing `/simplify` skill.

### Step 3: Run Tests

Run the test command from the progress file. Capture the exit code.

- **Tests pass** → continue to Step 4.
- **Tests fail** → revert all changes with `git checkout -- .`,
  update the progress file to mark this module as `skipped (tests failed)`,
  log the failure in the Findings Log, and continue to the next module.

### Step 4: Auto-Commit

Stage and commit changes for the current module:

```bash
git add <module-path>
git commit -m "refactor: simplify <module-path>" \
  -m "<summary of key changes>"
```

Include a `Co-Authored-By: Claude <noreply@anthropic.com>` trailer.

### Step 5: Update Progress

Update the module line in `simplify-progress.md`:

```
- [x] <module-path> — <N> fixes, committed <short-sha>
```

Add a section under **Findings Log** with bullet points describing
the changes made.

### Step 6: Continue

Move to the next pending module. Repeat from Step 1.

---

## Phase 3: Complete

When all modules are processed:

1. Print a summary table showing modules simplified (with commit SHAs),
   modules skipped (with failure reasons), and total fixes applied.
2. Delete `simplify-progress.md` from the project root.
3. Run the full test suite one final time to confirm everything passes.

---

## Resume Behavior

When invoked and `simplify-progress.md` already exists:

1. Read the progress file.
2. Identify the first module marked `pending` or `in progress`.
3. If a module is marked `in progress`, check `git status`:
   - If clean, treat as `pending` (previous session may have ended
     before committing).
   - If dirty, revert with `git checkout -- .` and treat as `pending`.
4. Continue the module loop from Phase 2.

No questions are asked on resume — pick up and continue automatically.

---

## Important Notes

- Never modify files outside the current module being reviewed.
- The `/simplify` skill handles all review logic — this skill only
  orchestrates module iteration, progress tracking, and commits.
- If context limits are approaching mid-module, commit progress so far
  and update the progress file so the next session can resume.
