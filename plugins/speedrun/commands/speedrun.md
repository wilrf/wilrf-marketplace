---
description: "Comprehensive codebase optimization using parallel hunter agents with before/after verification"
---

# Speedrun Command

Perform comprehensive optimization hunting across the codebase with mandatory measurement.

## Process

1. **Measure baseline** - Capture current metrics before any changes
2. **Analyze codebase** - Identify tech stack, structure, and applicable hunters
3. **Dispatch hunters in parallel** - Use Task tool to run multiple hunters simultaneously
4. **Collect findings** - Aggregate all optimization opportunities with ROI scores
5. **Write SPEEDRUN.md** - Create prioritized optimization document
6. **Apply safe fixes** - Auto-apply low-risk optimizations
7. **Request approval** - Ask user about risky changes
8. **Verify improvements** - Re-measure and report deltas

## Baseline Metrics to Capture

Before dispatching hunters, measure what you can:

```bash
# Bundle size (if applicable)
du -sh dist/ build/ .next/ out/

# Package count
cat package.json | grep -c '"'

# Lines of code
find . -name "*.ts" -o -name "*.tsx" -o -name "*.js" | xargs wc -l

# Build time (run build and time it)
time npm run build
```

## Hunters to Dispatch

### Always dispatch:
- bundle-hunter
- complexity-hunter
- dead-code-hunter
- dependency-hunter
- algorithm-hunter

### Dispatch if applicable:
- build-hunter (if has build config: webpack, vite, etc.)
- query-hunter (if has database: Supabase, Prisma, SQL files)
- web-vitals-hunter (if has frontend: React, Vue, HTML)
- memory-hunter (if has server runtime: Node, Python, Go)
- image-hunter (if has images: public/, assets/, static/)

## Dispatch Pattern

Use the Task tool with `subagent_type: "general-purpose"` and include:
1. The hunter's full instructions
2. Baseline metrics from your measurements
3. Tech stack detected
4. Key file paths to focus on

Dispatch ALL applicable hunters in a SINGLE message for parallel execution.

## ROI Scoring

For each finding, calculate:
- **Impact**: How much improvement (KB saved, ms reduced, complexity points)
- **Effort**: How hard to fix (1=trivial, 2=moderate, 3=complex)
- **ROI**: Impact / Effort

Sort findings by ROI descending. Quick wins with big impact first.

## After Hunting

Write SPEEDRUN.md with:
- Baseline metrics captured
- Summary table with ROI scores
- All optimizations by ROI tier
- Clear before/after expectations

Then apply safe fixes automatically, ask about risky ones.

Finally: "Verifying improvements..." and re-measure everything.
