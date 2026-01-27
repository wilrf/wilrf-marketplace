---
description: "Comprehensive bug hunting across the entire codebase using parallel hunter agents"
---

# Bughunt Command

Perform comprehensive bug hunting across the codebase.

## Process

1. **Analyze codebase** - Identify tech stack, structure, and what hunters apply
2. **Dispatch hunters in parallel** - Use Task tool to run multiple hunters simultaneously
3. **Collect findings** - Aggregate all bug reports
4. **Write BUGHUNT.md** - Create prioritized fix document
5. **Offer to fix** - Ask user if they want to clear context and fix all bugs

## Hunters to Dispatch

### Always dispatch:
- frontend-hunter
- backend-hunter
- type-safety-hunter
- error-handling-hunter
- edge-case-hunter

### Dispatch if applicable:
- database-hunter (if has database)
- auth-hunter (if has auth)
- api-hunter (if has API routes)
- env-hunter (if has env config)
- security-hunter (always for production code)
- performance-hunter (for larger codebases)
- test-hunter (if has tests)
- dependency-hunter (if has package.json)

## Dispatch Pattern

Use the Task tool with `subagent_type: "general-purpose"` and include the hunter's full instructions in the prompt. Dispatch ALL applicable hunters in a SINGLE message for parallel execution.

## After Hunting

Write BUGHUNT.md with:
- Summary table (Critical/High/Medium/Low counts)
- All bugs by severity
- File paths, descriptions, fixes

Then ask: "Would you like me to clear context and fix all bugs systematically?"
