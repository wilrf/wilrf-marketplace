# Bughunt Plugin Marketplace

Psychologically-enhanced bug hunting for Claude Code using personality-primed parallel agents.

## What's New in v2.0.0

Based on research from [AI Agent Behavioral Science](https://arxiv.org/abs/2506.06366) and [Psychologically Enhanced AI Agents](https://arxiv.org/abs/2509.04343), each hunter now has:

- **Psychological Profile** — Domain-specific archetype (PARANOID, OBSESSIVE, EMPATHETIC, etc.)
- **Cognitive Style** — How they hunt (INTUITIVE, ANALYTICAL, DETAIL, HOLISTIC)
- **Voice** — Distinct personality in bug reports
- **Third Pass Reflection** — Opposite-style self-check for thoroughness
- **Confidence Scoring** — HIGH/MEDIUM/LOW on each finding

## Installation

```bash
/plugin marketplace add wilrf/bughunt-marketplace
/plugin install bughunt@bughunt-marketplace
```

## Usage

```bash
/bughunt
```

## Hunter Personalities

| Hunter | Archetype | Cognitive Style | Voice |
|--------|-----------|-----------------|-------|
| security-hunter | PARANOID | Intuitive | "This is wide open to injection" |
| edge-case-hunter | OBSESSIVE | Detail | "Nobody checked if this is null" |
| frontend-hunter | EMPATHETIC | Holistic | "A user would rage-quit here" |
| backend-hunter | SKEPTICAL | Analytical | "This assumption isn't validated" |
| type-safety-hunter | PEDANTIC | Analytical | "This `any` is hiding real issues" |
| error-handling-hunter | PESSIMISTIC | Detail | "When this throws, no one catches it" |
| database-hunter | SUSPICIOUS | Analytical | "This leaks user A's data to user B" |
| auth-hunter | DISTRUSTFUL | Intuitive | "This permission check is security theater" |
| performance-hunter | IMPATIENT | Holistic | "At 10,000 users, this will timeout" |
| test-hunter | CYNICAL | Detail | "This test proves nothing" |
| env-hunter | CAUTIOUS | Detail | "This works in dev. Production will differ." |
| api-hunter | METICULOUS | Analytical | "The API promises X, delivers Y" |
| dependency-hunter | WARY | Analytical | "This package is abandoned" |

## The Iron Law

Every hunter follows three passes:

1. **First Pass** — Systematic checklist
2. **Second Pass** — Uncomfortable questions
3. **Third Pass (The Reckoning)** — Switch cognitive styles to catch what the primary style missed

## Output

Creates `BUGHUNT.md` with findings including:
- Severity (CRITICAL/HIGH/MEDIUM/LOW)
- Voiced findings in each hunter's personality
- Evidence and fix suggestions
- Confidence level for prioritization

## Research Sources

- [AI Agent Behavioral Science](https://arxiv.org/abs/2506.06366) — Fogg Model, SDT applied to AI agents
- [Psychologically Enhanced AI Agents](https://arxiv.org/abs/2509.04343) — MBTI-in-Thoughts personality priming
- [Gamification Psychology](https://www.gianty.com/gamification-boost-user-engagement-in-2025/) — Feedback loops, flow state
