---
name: bughunt
description: Use when performing comprehensive bug hunting, code auditing, or finding all issues in a codebase before release or after major changes
---

# Bughunt Skill

Comprehensive bug hunting that dispatches parallel agents to find bugs across all layers.

## The Iron Law

```
WHEN YOU THINK YOU'RE DONE, YOU'RE NOT.
```

Every hunter follows three passes. The second pass catches the bugs that hide after you feel "done."

## Quick Reference

| Hunter | Focus | Color |
|--------|-------|-------|
| frontend-hunter | React, CSS, a11y | yellow |
| backend-hunter | API, logic, data | cyan |
| type-safety-hunter | TypeScript, `any` | blue |
| error-handling-hunter | try/catch, boundaries | red |
| edge-case-hunter | null, empty, boundaries | magenta |
| security-hunter | XSS, injection, secrets | red |
| database-hunter | queries, RLS, N+1 | green |
| auth-hunter | sessions, tokens, perms | red |
| api-hunter | endpoints, validation | cyan |
| env-hunter | config, env vars | yellow |
| performance-hunter | N+1, memory, bundle | magenta |
| test-hunter | flaky, coverage, assertions | green |
| dependency-hunter | outdated, vulnerabilities | blue |

## Workflow

1. Analyze codebase structure
2. Dispatch ALL applicable hunters in PARALLEL
3. Collect findings from all hunters
4. Write BUGHUNT.md with prioritized bugs
5. Offer to clear context and fix all

## Output: BUGHUNT.md

```markdown
# Bug Hunt Report

**Date:** YYYY-MM-DD

## Summary
| Severity | Count |
|----------|-------|
| Critical | N |
| High | N |
| Medium | N |
| Low | N |

## Critical Issues
[bugs...]

## High Priority
[bugs...]
```
