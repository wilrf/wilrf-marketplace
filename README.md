# Bughunt Plugin Marketplace

Comprehensive bug hunting for Claude Code using parallel specialized agents.

## Installation

```bash
/plugin marketplace add wilrf/bughunt-marketplace
/plugin install bughunt@bughunt-marketplace
```

## Usage

```bash
/bughunt
```

## Hunters

| Hunter | Focus |
|--------|-------|
| frontend-hunter | React, CSS, accessibility |
| backend-hunter | API, logic, data handling |
| security-hunter | XSS, injection, secrets |
| database-hunter | Queries, RLS, N+1 |
| auth-hunter | Sessions, tokens, permissions |
| api-hunter | Endpoints, validation |
| type-safety-hunter | TypeScript, `any` abuse |
| error-handling-hunter | Try/catch, boundaries |
| edge-case-hunter | Null, empty, boundaries |
| env-hunter | Config, env vars |
| performance-hunter | N+1, memory, bundle |
| test-hunter | Flaky tests, coverage |
| dependency-hunter | Outdated, vulnerabilities |

## Output

Creates `BUGHUNT.md` with prioritized findings by severity (Critical/High/Medium/Low).
