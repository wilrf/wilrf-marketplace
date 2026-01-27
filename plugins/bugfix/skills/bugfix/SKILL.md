---
name: bugfix
description: Surgical bug fixing skill. Validates bugs, applies minimal fixes, prefers tactical deletion.
---

# Bugfix Skill

## Core Philosophy

```
The best fix removes code.
But the bug MUST be fixed.
```

## Fixer Principles

All fixers share these principles:

1. **SURGICAL** — Precise, minimal cuts. No collateral changes.
2. **REDUCTIVE** — Ask "what can I delete?" before "what should I add?"
3. **AUSTERE** — Zero tolerance for unnecessary abstraction.
4. **MINIMALIST** — Smallest diff that solves the problem.

## Validation Checklist

Before applying ANY fix:

- [ ] Is this bug real? (reject false positives from paranoid hunters)
- [ ] What's the SMALLEST fix?
- [ ] Can I DELETE code instead of adding?
- [ ] Does existing code already handle this?
- [ ] Will this fix cause regressions?

## Before Committing

- [ ] Bug is confirmed FIXED (not just changed)
- [ ] No regressions introduced
- [ ] Diff is minimal for THIS fix
- [ ] Deletions are tactical, not cosmetic
- [ ] Code compiles/runs

## Output Format

Each fixer reports:

```markdown
### Fix: [Bug Title from BUGHUNT.md]

- **Status:** FIXED | REJECTED (false positive) | DEFERRED
- **Original Severity:** CRITICAL | HIGH | MEDIUM | LOW
- **Validation:** Why this bug is/isn't real
- **Solution:** What was changed
- **Approach:** Why this fix (especially if different from hunter's suggestion)
- **Diff:** +X -Y lines
- **Files:** List of modified files
```

## BUGFIX.md Template

```markdown
# Bugfix Report

Generated: [timestamp]
Source: BUGHUNT.md

## Summary
- Bugs processed: N
- Bugs fixed: N
- False positives rejected: N
- Deferred: N
- Total diff: +X -Y lines

## Fixes Applied

[Individual fix reports...]

## Rejected (False Positives)

[Why each rejected bug wasn't real...]

## Deferred

[Bugs that need human decision...]
```
