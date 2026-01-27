---
name: speedrun
description: Use when optimizing codebase performance, reducing bundle sizes, improving build times, or eliminating code bloat. Launches parallel optimization hunters with mandatory before/after verification.
---

# Speedrun Skill

Comprehensive optimization hunting that dispatches parallel agents to find performance bottlenecks across all layers, then applies fixes with mandatory verification.

## The Iron Law

```
NO OPTIMIZATION CLAIM WITHOUT MEASUREMENT.
```

Every optimization must have before/after metrics. Unverified "improvements" are worthless.

## Quick Reference

| Hunter | Archetype | Focus | Color |
|--------|-----------|-------|-------|
| bundle-hunter | OBSESSIVE | File sizes, tree-shaking, code splitting | cyan |
| complexity-hunter | PEDANTIC | Cyclomatic complexity, nesting, function length | blue |
| dead-code-hunter | SUSPICIOUS | Unused exports, orphan files, unreachable paths | yellow |
| dependency-hunter | WARY | Bloat, duplicates, outdated, vulnerabilities | magenta |
| build-hunter | IMPATIENT | Compilation time, caching, parallelization | red |
| algorithm-hunter | ANALYTICAL | Big O, data structures, repeated computation | green |
| query-hunter | SKEPTICAL | N+1, missing indexes, slow queries | green |
| web-vitals-hunter | EMPATHETIC | LCP, CLS, INP, TTFB | yellow |
| memory-hunter | PARANOID | Leaks, allocation patterns, GC pressure | red |
| image-hunter | METICULOUS | Formats, compression, lazy loading | cyan |

## Workflow

1. **Measure baseline** - Gather current metrics (bundle size, build time, complexity scores)
2. **Analyze codebase** - Detect tech stack, structure, applicable hunters
3. **Inject context** - Pass baseline metrics and stack info to all agents
4. **Dispatch ALL applicable hunters in PARALLEL**
5. **Collect findings** - Aggregate with ROI scoring (Impact / Effort)
6. **Write SPEEDRUN.md** - Prioritized optimization report
7. **Apply fixes** - Safe fixes auto-apply, risky fixes require approval
8. **Verify improvements** - Re-measure and report deltas

## Fix Tiers

### Auto-Apply (Safe)
- Remove unused imports/exports
- Delete dead code files
- Optimize image formats
- Add missing lazy loading
- Remove unused dependencies

### Require Approval (Risky)
- Algorithm refactors
- Dependency swaps
- Code splitting changes
- Build config modifications
- Database index additions

## Output: SPEEDRUN.md

```markdown
# Speedrun Optimization Report

**Date:** YYYY-MM-DD
**Baseline:** Bundle: X MB | Build: Xs | Complexity: Y

## Summary
| Category | Issues | Potential Savings | ROI Score |
|----------|--------|-------------------|-----------|
| Bundle Size | N | -X KB | HIGH |
| Build Time | N | -Xs | MEDIUM |
| Complexity | N | -Y points | LOW |

## High ROI Optimizations
[Quick wins with big impact...]

## Medium ROI Optimizations
[Moderate effort, good returns...]

## Low ROI Optimizations
[Consider for later...]

## Verification
| Metric | Before | After | Delta |
|--------|--------|-------|-------|
| Bundle | X MB | Y MB | -Z% |
| Build | Xs | Ys | -Z% |
```

## After Optimization

Ask: "Would you like me to verify all improvements with fresh measurements?"
