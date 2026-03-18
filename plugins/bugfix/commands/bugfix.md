---
name: bugfix
description: Fix bugs from BUGHUNT.md with surgical precision. Validates each bug, applies minimal fixes, prefers deletion.
---

# Bugfix

Read `BUGHUNT.md` and fix each bug systematically.

## Process

1. Parse BUGHUNT.md for all reported bugs
2. Sort by severity: CRITICAL → HIGH → MEDIUM → LOW
3. For each bug:
   - Dispatch to matching fixer agent (security-fixer, type-safety-fixer, etc.)
   - Fixer **validates** the bug: classifies as CONFIRMED, PLAUSIBLE, or REJECTED
   - For CONFIRMED/PLAUSIBLE: fixer applies minimal fix
   - For REJECTED: fixer documents why it's a false positive — no code change
   - Commit the fix (or rejection note)
4. Generate BUGFIX.md report

## Validation Confidence Rubric

Before any fixer touches code, they classify the finding:

| Confidence | Meaning | Action |
|------------|---------|--------|
| CONFIRMED | Execution path traced, vulnerability verified | Fix immediately |
| PLAUSIBLE | Pattern looks real but needs context check | Verify then fix |
| REJECTED | False positive — framework handles it, input can't reach the sink | Document, skip |

Rejecting a false positive is correct behavior — hunters are intentionally
paranoid, fixers are intentionally skeptical.

## Fixer Philosophy

**Hierarchy of priorities:**
1. FIX the bug — non-negotiable
2. VERIFY the fix works
3. MINIMIZE footprint — smallest change possible
4. DELETE tactically — remove code only when it's the right solution

**Never:**
- Delete working code just to shrink the diff
- Add unnecessary abstractions
- Over-engineer the solution
- Leave the bug unfixed

## Dispatch Rules

Match bug type to fixer:
- Security vulnerabilities → security-fixer
- Auth/authorization → auth-fixer
- Edge cases → edge-case-fixer
- Error handling → error-handling-fixer
- Test issues → test-fixer
- Environment config → env-fixer
- Frontend/UI → frontend-fixer
- Performance → performance-fixer
- Backend logic → backend-fixer
- Database → database-fixer
- Type safety → type-safety-fixer
- API issues → api-fixer
- Dependencies → dependency-fixer

## Output

Generate `BUGFIX.md` with:
- Bugs confirmed (CONFIRMED/PLAUSIBLE) vs rejected (false positives)
- **Confidence** classification for every finding
- What was fixed and how
- Why false positives were rejected
- Diff stats per fix
- Total code impact
