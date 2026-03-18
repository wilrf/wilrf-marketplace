---
description: "Comprehensive codebase optimization using parallel hunter agents with before/after verification"
---

# Speedrun Command

Perform comprehensive optimization hunting across the codebase with mandatory measurement.

## Process

1. **Measure baseline** - Capture current metrics before any changes
2. **Detect stack** - Identify framework, build tooling, database, image handling
3. **Dispatch applicable hunters in parallel** - Stack-gated (skip irrelevant hunters)
4. **Collect findings** - Aggregate with P0-P3 priority and confidence ratings
5. **Write SPEEDRUN.md** - Prioritized optimization document
6. **Apply safe fixes** - Auto-apply P0/P1 low-risk optimizations
7. **Request approval** - Ask user about complex or risky changes
8. **Verify improvements** - Re-measure and report deltas

## Baseline Metrics to Capture

Before dispatching hunters, measure what you can:

```bash
# Bundle size (if applicable)
du -sh dist/ build/ .next/ out/ 2>/dev/null

# Package count
cat package.json | grep -c '"' 2>/dev/null

# Lines of code
find . -name "*.ts" -o -name "*.tsx" -o -name "*.js" | grep -v node_modules | xargs wc -l 2>/dev/null | tail -1

# Build time (if build command exists)
time npm run build 2>/dev/null
```

## Hunters to Dispatch

### Always dispatch:
- bundle-hunter
- complexity-hunter
- dead-code-hunter
- dependency-hunter
- algorithm-hunter

### Dispatch if applicable:
- build-hunter (has build config: webpack.config*, vite.config*, turbo.json)
- query-hunter (has database: Supabase, Prisma, SQL files, Mongoose)
- web-vitals-hunter (has frontend: React, Vue, HTML files)
- memory-hunter (has server runtime: Node.js, Python, Go)
- image-hunter (has images in public/, assets/, or static/)

## Priority System

All findings are classified P0–P3 by ROI:

| Priority | Impact | Effort | Action |
|----------|--------|--------|--------|
| P0 | >50% improvement or >100KB savings | Trivial | Fix immediately |
| P1 | 20-50% improvement or 50-100KB savings | Straightforward | Fix this sprint |
| P2 | 10-20% improvement or 10-50KB savings | Moderate | Plan for later |
| P3 | <10% improvement or <10KB savings | Any | Backlog |

Fix in priority order. P0 always before P1.

## Dispatch Pattern

Use the Task tool with `subagent_type: "general-purpose"` and include:
1. The hunter's full instructions
2. Baseline metrics from your measurements
3. Tech stack detected
4. Key file paths to focus on

Dispatch ALL applicable hunters in a SINGLE message for parallel execution.

## After Hunting

Write SPEEDRUN.md with:
- Baseline metrics captured
- Summary table by hunter and priority tier (P0/P1/P2/P3)
- All optimizations sorted by Priority then Confidence
- Clear before/after expectations per finding

Then apply safe fixes automatically, ask about risky ones.

Finally: re-measure everything and report the verified delta. Never claim an improvement without measurement.
