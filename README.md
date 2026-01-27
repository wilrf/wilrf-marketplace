# Bughunt

AI agents find more bugs when they have personalities.

## Why

Research shows personality-primed AI agents outperform neutral ones ([arXiv:2509.04343](https://arxiv.org/abs/2509.04343)). A paranoid security hunter catches exploits a generic agent misses. An obsessive edge-case hunter won't stop asking "but what if it's null?"

## The Science

- **Archetype priming** — Each hunter adopts a domain-specific mindset (PARANOID, OBSESSIVE, SKEPTICAL, etc.)
- **Cognitive diversity** — Hunters use different thinking styles (INTUITIVE vs ANALYTICAL, DETAIL vs HOLISTIC)
- **Self-reflection** — Third pass forces opposite-style thinking to catch blind spots

## Install

```
/plugin marketplace add wilrf/bughunt-marketplace
/plugin install bughunt@bughunt-marketplace
```

## Use

```
/bughunt
```

## Hunters

| Hunter | Personality | Style |
|--------|-------------|-------|
| security | PARANOID | Intuitive |
| auth | DISTRUSTFUL | Intuitive |
| edge-case | OBSESSIVE | Detail |
| error-handling | PESSIMISTIC | Detail |
| test | CYNICAL | Detail |
| env | CAUTIOUS | Detail |
| frontend | EMPATHETIC | Holistic |
| performance | IMPATIENT | Holistic |
| backend | SKEPTICAL | Analytical |
| database | SUSPICIOUS | Analytical |
| type-safety | PEDANTIC | Analytical |
| api | METICULOUS | Analytical |
| dependency | WARY | Analytical |

## Output

`BUGHUNT.md` — Prioritized bugs with severity, evidence, and confidence levels.
