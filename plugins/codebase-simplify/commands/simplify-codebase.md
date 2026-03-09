---
description: "Post-refactoring cleanup: analyze staleness, simplify active code, archive dead files"
allowed-tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]
---

# /simplify-codebase

Full post-refactoring codebase cleanup with three phases:

1. **Analyze** — Classify files as fresh/stale/dead using git history and dependency graphs
2. **Simplify** — Clean up active code module-by-module with auto-commits
3. **Archive** — Move dead files to `_archive/` with provenance log

## Usage

```
/simplify-codebase                          # Full run, 30-day staleness window
/simplify-codebase --window 14              # 14-day staleness window
/simplify-codebase --analyze-only           # Just produce the inventory, don't act
/simplify-codebase --archive-only           # Skip simplification, just archive dead files
/simplify-codebase --exclude "docs,scripts" # Exclude directories from analysis
```

## What Happens

* Uses the **staleness-analyzer** agent to build a scored inventory
* Presents dead file candidates for your approval before archiving
* Uses the **code-simplifier** agent on active code (tests after each module)
* Uses the **stale-archiver** agent to move dead files with `git mv`
* Resumes automatically if a previous session was interrupted
* Maintains `_archive/ARCHIVE_LOG.md` so nothing is ever lost

## Commit Format

Simplification commits:
```
refactor: simplify <module-path>
```

Archive commits:
```
chore: archive stale files from <directory>
```

All commits include `Co-Authored-By: Claude <noreply@anthropic.com>`.
