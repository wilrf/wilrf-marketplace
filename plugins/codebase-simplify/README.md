# codebase-simplify

Post-refactoring codebase cleanup for Claude Code. Identifies what's fresh, stale, and dead in your codebase, then simplifies the active code and archives the rest.

## What It Does

After a refactoring session, your codebase typically has three kinds of files:

* **Fresh** — Recently modified, actively imported, the good stuff
* **Stale** — Not recently modified but still referenced by live code
* **Dead** — Not modified, not referenced, safe to archive

This plugin uses git history and import/dependency analysis to classify every file, then:

1. **Simplifies** the fresh code (clarity, consistency, naming, imports)
2. **Archives** the dead code to `_archive/` with full git history preserved
3. **Logs** everything to `ARCHIVE_LOG.md` so nothing is ever truly lost

## Installation

```bash
# From within Claude Code
/plugin marketplace add dougfowler-nb/codebase-simplify
/plugin install codebase-simplify@codebase-simplify
```

Or install from a local directory:

```bash
claude --plugin-dir /path/to/codebase-simplify
```

## Usage

```
/simplify-codebase                    # Full cleanup
/simplify-codebase --window 14        # 14-day staleness window
/simplify-codebase --analyze-only     # Just produce the inventory
/simplify-codebase --archive-only     # Skip simplification, archive only
```

Or trigger naturally:

* "Clean up the codebase after refactoring"
* "Find and archive stale code"
* "What files are dead in this project?"
* "Simplify everything"

## Architecture

Three specialized agents, orchestrated by one skill:

```
/simplify-codebase (command)
  -> simplify-codebase (skill/orchestrator)
      -> staleness-analyzer (agent)    — classify files
      -> code-simplifier (agent)       — clean active code
      -> stale-archiver (agent)        — move dead files
```

### staleness-analyzer

Uses `git log` to find recently modified files, builds an import/dependency graph, and scores each file's archivability (0-100). Understands framework entrypoints (Next.js routes, Convex functions, .NET Program.cs) to avoid false positives.

### code-simplifier

Operates ONLY on fresh files. Removes dead variables, simplifies nesting, extracts repeated logic, improves naming, cleans imports. Tests after each module. Follows project standards from CLAUDE.md.

### stale-archiver

Moves dead files to `_archive/` using `git mv` (preserves full history). Creates mirrored directory structure. Cleans up broken imports. Maintains `ARCHIVE_LOG.md` with dates, confidence scores, and restoration instructions.

## Resume Support

Progress is tracked in `simplify-progress.json`. If a session ends mid-run, the next invocation picks up exactly where it left off. No duplicate work, no questions asked.

## Supported Project Types

* Next.js / React (JS/TS)
* Convex
* .NET / C#
* Python
* Rust
* Go

Detection is automatic based on project files.

## Safety

* Every archived file keeps its full git history via `git mv`
* Tests run between every batch of changes
* Failed batches are automatically reverted
* A final reference check runs before each file is archived
* Framework entrypoints and config files are never auto-archived
* The user approves the archive list before anything moves

## Restoring Archived Files

```bash
git mv _archive/src/old-widget.tsx src/old-widget.tsx
```
