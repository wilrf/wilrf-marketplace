---
name: simplify-codebase
description: >-
  Full post-refactoring codebase cleanup. Use when the user says "simplify the
  whole codebase", "clean up after refactoring", "find and archive stale code",
  "what files are dead", "archive unused files", "clean up the whole repo",
  "simplify everything", "identify stale code", or "post-refactor cleanup".
  Orchestrates three specialized agents: staleness-analyzer (classifies files as
  fresh/stale/dead via git history and dependency graphs), code-simplifier (cleans
  up active code), and stale-archiver (moves dead files to _archive/ with full
  provenance). Handles resume across sessions.
---

# Codebase Simplify

Orchestrate a full post-refactoring cleanup by analyzing what's fresh, what's stale, and what's dead, then simplifying the good code and archiving the rest.

## Three-Agent Architecture

This skill delegates to three specialized agents:

1. **staleness-analyzer** — Classifies every file as FRESH, STALE, or DEAD using git history, import/dependency graphs, and framework awareness. Produces a scored inventory with confidence ratings.

2. **code-simplifier** — Operates only on FRESH files. Simplifies code for clarity and consistency while preserving functionality. Follows project standards from CLAUDE.md.

3. **stale-archiver** — Moves DEAD files to `_archive/` preserving directory structure and git history. Maintains `ARCHIVE_LOG.md` with full provenance.

## Phase 1: Initialize

### Pre-Flight Checks

1. Run `git status --porcelain`. If non-empty, abort:

   > "Working tree has uncommitted changes. Commit or stash before running."

2. Check for `simplify-progress.json` at the project root:
   * If it exists with pending phases, skip to **Resume**.
   * If it exists and all phases are complete, report completion and stop.
   * If it doesn't exist, continue.

### Detect Project Type

Identify the project's language/framework to configure the staleness-analyzer:

| Signal | Type |
|--------|------|
| package.json + next.config.* | Next.js/React |
| convex/ directory | Convex backend |
| *.csproj or *.sln | .NET/C# |
| pyproject.toml or setup.py | Python |
| Cargo.toml | Rust |
| go.mod | Go |

Store the detected type. This determines which import patterns and framework entrypoints the staleness-analyzer checks.

### Discover Test Command

Resolve in priority order:

1. CLAUDE.md — "Commands" or "Common Commands" section
2. package.json — `scripts.test`
3. pyproject.toml — `[tool.pytest]` or `[tool.hatch]`
4. Makefile — `test` target
5. If none found, ask the user.

### Write Progress File

Create `simplify-progress.json`:

```json
{
  "started": "2026-03-09",
  "projectType": "nextjs-convex",
  "testCommand": "npm test",
  "stalenessWindow": 30,
  "phases": {
    "analyze": "pending",
    "simplify": "pending",
    "archive": "pending",
    "verify": "pending"
  },
  "inventory": null,
  "commits": [],
  "errors": []
}
```

## Phase 2: Analyze (staleness-analyzer agent)

Invoke the **staleness-analyzer** agent scoped to the entire project.

The agent will:

1. Identify all files modified in the staleness window (default 30 days)
2. Build a dependency/import graph
3. Classify every source file as FRESH, STALE, or DEAD with confidence scores
4. Produce a markdown inventory table

After the inventory is produced:

1. Present the inventory to the user as a summary:

   > "Found N fresh files, M stale files, and K dead files (confidence 60+). Here are the dead files I recommend archiving:"
   > [table of DEAD files]

2. Ask for confirmation before proceeding:

   > "Should I proceed with simplifying the fresh code and archiving the dead files? You can also exclude specific files."

3. Save the inventory to `simplify-progress.json`.
4. Update phase: `"analyze": "done"`.

## Phase 3: Simplify (code-simplifier agent)

For each module containing FRESH files, invoke the **code-simplifier** agent.

### Module Discovery

Group fresh files by their top-level directory:

* `src/dashboard/*.tsx` = one module
* `src/api/*.ts` = another module
* `convex/*.ts` = another module

If any module has 20+ files, split into sub-batches of ~10.

### Module Loop

For each module:

1. Invoke the **code-simplifier** agent scoped to that module
2. Run the test command
3. **Tests pass**: auto-commit with message:

   ```
   refactor: simplify <module-path>

   <bullet summary of changes>

   Co-Authored-By: Claude <noreply@anthropic.com>
   ```

4. **Tests fail**: revert with `git checkout -- .`, log the failure, skip
5. Update `simplify-progress.json` with commit SHA or skip reason

After all modules: update phase `"simplify": "done"`.

## Phase 4: Archive (stale-archiver agent)

Invoke the **stale-archiver** agent with the DEAD files from the inventory.

The agent will:

1. Re-verify each file has no live references (final safety check)
2. Create `_archive/` with mirrored directory structure
3. `git mv` each dead file into the archive
4. Clean up broken references in remaining files
5. Append to `_archive/ARCHIVE_LOG.md`

Archive in batches by directory, running tests between batches:

* **Tests pass**: commit with `chore: archive stale files from <dir>`
* **Tests fail**: restore that batch, log, skip, continue

After all batches: update phase `"archive": "done"`.

## Phase 5: Verify

1. Run the full test suite
2. If anything fails, report which archived file likely caused it and offer to restore: `git mv _archive/<path> <path>`
3. Print a final summary:

```markdown
#### Codebase Simplify Complete

##### Simplified (N modules)
| Module | Changes | Commit |
|--------|---------|--------|
| src/dashboard | 12 fixes | abc1234 |

##### Archived (K files, ~M lines removed)
| File | Confidence | Reason |
|------|-----------|--------|
| src/old-widget.tsx | 92 | No imports, 120 days stale |

##### Skipped (J items)
| Item | Reason |
|------|--------|
| src/utils | Tests failed after simplification |

##### Impact
- Lines removed: ~M
- Files archived: K
- Active code simplified: N modules
- All tests passing: Yes/No
```

4. Delete `simplify-progress.json`.

## Resume Behavior

When invoked and `simplify-progress.json` exists:

1. Read the progress file.
2. Find the first phase still marked `pending` or `in-progress`.
3. If a phase is `in-progress`, check `git status`:
   * Clean tree: treat as pending (session ended before committing)
   * Dirty tree: revert and treat as pending
4. Continue from that phase. No questions asked.

## Configuration

Users can customize via `simplify-progress.json` or command arguments:

* **stalenessWindow**: Days since last modification to consider stale (default: 30)
* **archiveDir**: Where to put archived files (default: `_archive/`)
* **autoArchive**: Skip confirmation for files with confidence 90+ (default: false)
* **excludePaths**: Glob patterns to never archive (e.g., `["docs/**", "scripts/**"]`)

## Important Notes

* The staleness-analyzer runs first and its inventory gates everything else. No simplification or archiving happens without the classification step.
* The code-simplifier ONLY touches FRESH files. It never modifies stale or dead code (that would be wasted effort).
* The stale-archiver uses `git mv` so full file history is preserved. Restoring an archived file is a single command.
* Each phase commits independently so progress is never lost.
* If context limits approach mid-phase, commit progress and update the progress file so the next session resumes cleanly.
