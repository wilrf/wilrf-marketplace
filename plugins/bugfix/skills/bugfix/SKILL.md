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

## Validation Confidence Rubric

All fixers classify each incoming finding before touching code:

| Confidence | Meaning | Action |
|------------|---------|--------|
| CONFIRMED | Reproduced or verified by static analysis | Fix it |
| PLAUSIBLE | Pattern looks right but context may differ | Verify first, then fix |
| REJECTED | False positive — not exploitable or not reachable | Document why, skip |

A REJECTED finding with a clear explanation is as valuable as a fix — it prevents
wasted work and validates that the hunter was wrong about this one.

## Fixer Principles

All fixers share these principles:

1. **SURGICAL** — Precise, minimal cuts. No collateral changes.
2. **REDUCTIVE** — Ask "what can I delete?" before "what should I add?"
3. **AUSTERE** — Zero tolerance for unnecessary abstraction.
4. **MINIMALIST** — Smallest diff that solves the problem.

## Validation Checklist

Before applying ANY fix:

- [ ] Classified as CONFIRMED, PLAUSIBLE, or REJECTED
- [ ] For PLAUSIBLE: verified the execution path before writing code
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

- **Status:** FIXED | REJECTED | DEFERRED
- **Confidence:** CONFIRMED | PLAUSIBLE | REJECTED
- **Original Severity:** CRITICAL | HIGH | MEDIUM | LOW
- **Validation:** [Why this bug is/isn't real — trace the execution path or explain the false positive]
- **Solution:** [What was changed]
- **Approach:** [Why this fix, especially if different from hunter's suggestion]
- **Diff:** +X -Y lines
- **Files:** [Modified files]
```

## BUGFIX.md Template

```markdown
# Bugfix Report

Generated: [timestamp]
Source: BUGHUNT.md

## Summary
- Bugs processed: N
- Confirmed and fixed: N
- False positives rejected: N
- Deferred (need human decision): N
- Total diff: +X -Y lines

## Fixes Applied

### Fix: [Bug Title]
- **Status:** FIXED
- **Confidence:** CONFIRMED
- **Original Severity:** CRITICAL | HIGH | MEDIUM | LOW
- **Validation:** [Execution path traced, confirmed real]
- **Solution:** [What changed]
- **Approach:** [Why this approach]
- **Diff:** +X -Y lines
- **Files:** [files]

## Rejected (False Positives)

### Rejected: [Bug Title]
- **Confidence:** REJECTED
- **Original Severity:** [severity]
- **Reason:** [Why this isn't real — framework handles it, input can't reach the sink, etc.]

## Deferred

### Deferred: [Bug Title]
- **Reason:** [Needs product decision, architectural change, or more context]
```
