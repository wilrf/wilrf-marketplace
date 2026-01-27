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
   - Fixer validates the bug is real
   - Fixer applies minimal fix
   - Commit the fix
4. Generate BUGFIX.md report

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
- Bugs confirmed vs rejected (false positives)
- What was fixed and how
- Diff stats per fix
- Total code impact
