# wilrf-marketplace

AI agents perform better when they have personalities. Paranoid agents find bugs others miss. Obsessive agents find bloat others ignore.

## Plugins

### bughunt
Paranoid hunters find bugs others miss.

### bugfix
Calm fixers apply minimal, surgical repairs.

### speedrun
Obsessive optimizers find every wasted byte and millisecond.

## The Pipelines

**Bug Hunting:**
```
/bughunt → BUGHUNT.md → /bugfix → BUGFIX.md + fixed code
```

**Optimization:**
```
/speedrun → baseline metrics → hunters → SPEEDRUN.md → fixes → verification
```

## Install

```
/plugin marketplace add wilrf/wilrf-marketplace
/plugin install bughunt@wilrf-marketplace
/plugin install bugfix@wilrf-marketplace
/plugin install speedrun@wilrf-marketplace
```

## Agents by Plugin

### bughunt - Bug Hunters

| Hunter | Archetype | Focus |
|--------|-----------|-------|
| security | PARANOID | XSS, injection, secrets |
| auth | DISTRUSTFUL | Sessions, tokens, perms |
| edge-case | OBSESSIVE | Null, empty, boundaries |
| error-handling | PESSIMISTIC | Try/catch, boundaries |
| test | CYNICAL | Flaky, coverage, assertions |
| env | CAUTIOUS | Config, env vars |
| frontend | EMPATHETIC | React, CSS, a11y |
| performance | IMPATIENT | N+1, memory, bundle |
| backend | SKEPTICAL | API, logic, data |
| database | SUSPICIOUS | Queries, RLS, N+1 |
| type-safety | PEDANTIC | TypeScript, `any` |
| api | METICULOUS | Endpoints, validation |
| dependency | WARY | Outdated, vulnerabilities |

### bugfix - Bug Fixers

| Fixer | Archetype | Approach |
|-------|-----------|----------|
| security | CALM | Minimal secure patches |
| auth | TRUSTING-BUT-VERIFYING | Verify then fix |
| edge-case | PRAGMATIC | Simple defensive code |
| error-handling | OPTIMISTIC | Graceful handling |
| test | CONSTRUCTIVE | Improve tests minimally |
| env | CONFIDENT | Simplify config |
| frontend | OBJECTIVE | Minimal UI fixes |
| performance | PATIENT | Measured optimizations |
| backend | TRUSTING | Minimal logic fixes |
| database | METHODICAL | Careful schema/query fixes |
| type-safety | PRACTICAL | Minimal type fixes |
| api | EFFICIENT | Minimal endpoint fixes |
| dependency | DECISIVE | Clean dependency changes |

### speedrun - Optimization Hunters

| Hunter | Archetype | Focus |
|--------|-----------|-------|
| bundle | OBSESSIVE | File sizes, tree-shaking, code splitting |
| complexity | PEDANTIC | Cyclomatic complexity, nesting, length |
| dead-code | SUSPICIOUS | Unused exports, orphan files |
| dependency | WARY | Bloat, duplicates, vulnerabilities |
| build | IMPATIENT | Compilation time, caching |
| algorithm | ANALYTICAL | Big O, data structures |
| query | SKEPTICAL | N+1, indexes, slow queries |
| web-vitals | EMPATHETIC | LCP, CLS, INP, TTFB |
| memory | PARANOID | Leaks, unbounded caches |
| image | METICULOUS | Formats, compression, lazy loading |

## Philosophy

### Bug Hunting
Find every bug. Miss nothing. Three passes minimum.

### Bug Fixing
1. **FIX the bug** — non-negotiable
2. **VERIFY it works** — no false fixes
3. **MINIMIZE footprint** — smallest change possible
4. **DELETE tactically** — remove code when it's the right solution

The best diff: `+100 -1000`

### Optimization
1. **MEASURE first** — no claims without baseline
2. **FIND everything** — parallel hunters, multiple perspectives
3. **PRIORITIZE by ROI** — impact ÷ effort
4. **AUTO-FIX safe changes** — trivial wins without asking
5. **VERIFY improvements** — before/after metrics required

The Iron Law: `NO OPTIMIZATION CLAIM WITHOUT MEASUREMENT`

## Output Files

| Plugin | Output | Purpose |
|--------|--------|---------|
| bughunt | `BUGHUNT.md` | Bugs with severity, evidence, confidence |
| bugfix | `BUGFIX.md` | Fixes applied, false positives rejected |
| speedrun | `SPEEDRUN.md` | Optimizations with ROI, before/after metrics |
