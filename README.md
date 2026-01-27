# Bughunt Marketplace

AI agents find more bugs when they have personalities. And fix them better when they're calm.

## Plugins

### bughunt
Paranoid hunters find bugs others miss.

### bugfix
Calm fixers apply minimal, surgical repairs.

## The Pipeline

```
/bughunt → BUGHUNT.md → /bugfix → BUGFIX.md + fixed code
```

## Install

```
/plugin marketplace add wilrf/bughunt-marketplace
/plugin install bughunt@bughunt-marketplace
/plugin install bugfix@bughunt-marketplace
```

## Hunters vs Fixers

| Domain | Hunter | Fixer |
|--------|--------|-------|
| security | PARANOID | CALM |
| auth | DISTRUSTFUL | TRUSTING-BUT-VERIFYING |
| edge-case | OBSESSIVE | PRAGMATIC |
| error-handling | PESSIMISTIC | OPTIMISTIC |
| test | CYNICAL | CONSTRUCTIVE |
| env | CAUTIOUS | CONFIDENT |
| frontend | EMPATHETIC | OBJECTIVE |
| performance | IMPATIENT | PATIENT |
| backend | SKEPTICAL | TRUSTING |
| database | SUSPICIOUS | METHODICAL |
| type-safety | PEDANTIC | PRACTICAL |
| api | METICULOUS | EFFICIENT |
| dependency | WARY | DECISIVE |

## Fixer Philosophy

1. **FIX the bug** — non-negotiable
2. **VERIFY it works** — no false fixes
3. **MINIMIZE footprint** — smallest change possible
4. **DELETE tactically** — remove code when it's the right solution

The best diff: `+100 -1000`

## Output

- `BUGHUNT.md` — Bugs found with severity, evidence, confidence
- `BUGFIX.md` — Fixes applied, false positives rejected, diff stats
