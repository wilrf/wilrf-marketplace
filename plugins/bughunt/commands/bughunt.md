---
description: "Comprehensive bug hunting across the entire codebase using parallel specialized hunter agents"
---

# Bughunt Command

Perform comprehensive bug hunting across the codebase using parallel agents.

## Process

1. **Detect tech stack** — Identify languages, frameworks, databases, auth libraries
2. **Dispatch hunters in parallel** — Use Task tool to run all applicable hunters simultaneously
3. **Collect all findings** — Aggregate reports from every hunter
4. **Deduplicate findings** — Merge findings that reference the same `file:line` from multiple hunters
5. **Write BUGHUNT.md** — Prioritized report with severity/confidence breakdown
6. **Offer to fix** — Ask user if they want to clear context and run `/bugfix`

## Hunters to Dispatch

### Always dispatch:
- frontend-hunter
- backend-hunter
- type-safety-hunter
- error-handling-hunter
- edge-case-hunter

### Dispatch if applicable (check before dispatching):
- `database-hunter` — if schema files, migration files, or ORM usage detected
- `auth-hunter` — if auth middleware, JWT, sessions, or login routes detected
- `api-hunter` — if REST/GraphQL/tRPC API routes detected
- `env-hunter` — if `.env`, `process.env`, or config files detected
- `security-hunter` — always for any production codebase
- `performance-hunter` — for codebases with 20+ components or DB queries
- `test-hunter` — if test files exist (`*.test.ts`, `*.spec.ts`)
- `dependency-hunter` — if `package.json` or `requirements.txt` exists

## Dispatch Pattern

Use the Task tool with `subagent_type: "general-purpose"` and include:
1. The hunter's full system prompt
2. Tech stack context you detected
3. Instruction to focus on recently changed files first

Dispatch ALL applicable hunters in a **SINGLE message** for parallel execution.

## Post-Collection: Deduplication

After all hunters report, before writing BUGHUNT.md:

1. Group all findings by `file:line` location
2. If multiple hunters flagged the same location, **merge into one finding**:
   - Use the highest severity across all reporters
   - Combine evidence from all hunters (they may have caught different angles)
   - Credit all contributing hunters: `**Hunter:** security-hunter, backend-hunter`
3. Sort merged findings by severity: CRITICAL → HIGH → MEDIUM → LOW

## BUGHUNT.md Format

```markdown
# Bug Hunt Report

**Date:** YYYY-MM-DD
**Stack:** [Detected stack]
**Hunters Run:** [List]

## Summary
| Severity | Count |
|----------|-------|
| Critical | N |
| High     | N |
| Medium   | N |
| Low      | N |

## Critical Issues
[Merged, deduplicated findings...]

## High Priority
[...]

## Medium / Low
[...]
```

Then ask: **"Would you like me to clear context and fix all bugs with `/bugfix`?"**
