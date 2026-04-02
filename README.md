# PaperclipAI

PaperclipAI is a company package that blends three things:

- Paperclip's importable company structure and governance surface
- Garry Tan's gstack engineering philosophy as the reference for the dev org
- A broader company layer for finance, marketing, legal, and self-improvement

## Import

```bash
paperclip company import --from .
```

## Org Chart

```
BOARD (Human)
  |
  +-- CEO
  |   |-- CIO
  |   |   `-- CTO
  |   |       |-- Staff Engineer
  |   |       |-- Release Engineer
  |   |       |-- QA Engineer
  |   |       |-- CSO
  |   |       `-- UX Designer
  |   |-- CFO
  |   |-- CMO
  |   `-- General Counsel
  `-- Evaluator
      `-- Learning Curator
```

## Agent Inventory

| Agent | Title | Skills | Heartbeat |
|---|---|---|---|
| CEO | Chief Executive Officer | office-hours | Monday (planning) |
| CIO | Chief Information Officer | autoplan, office-hours, plan-ceo-review, workflow-gate, learn | Monday (planning) + on demand |
| CFO | Chief Financial Officer | office-hours, investigate, learn | On demand + weekly budget review |
| CMO | Chief Marketing Officer | office-hours, browse, user-research, ab-test-design, learn | On demand + campaign review |
| General Counsel | General Counsel | office-hours, investigate, careful, guard, learn | On demand + legal review |
| CTO | Chief Technology Officer | plan-eng-review, plan-design-review, retro, codex | On demand + Sunday 23:00 UTC (retro) |
| Staff Engineer | Staff Engineer | review, investigate, guard, learn | On demand (build/review) |
| Release Engineer | Release Engineer | ship, land-and-deploy, document-release, setup-deploy | On demand (ship phase) |
| QA Engineer | QA Engineer | browse, qa, qa-only, benchmark, canary, setup-browser-cookies | On demand (test phase) |
| CSO | Chief Security Officer | cso-audit, investigate, careful, freeze, unfreeze, guard | On demand (review phase) |
| UX Designer | UX Designer | design-consultation, design-review, design-html, design-shotgun, user-research, wireframe, information-architecture, interaction-design, ab-test-design, browse, learn | On demand (plan/review) |
| Evaluator | Agent Performance Evaluator | evaluate, propose-improvement, retro, learn | Monday 02:00 UTC |
| Learning Curator | Institutional Memory Manager | learn, retro | Monday 06:00 UTC |

## Why This Shape Works

The engineering spine stays gstack-like: CIO scopes, CTO plans, Staff Engineer builds, CSO and QA harden, Release Engineer ships, and Evaluator/Learning Curator close the loop. The added C-suite roles make the package feel like a company instead of a software-only org, while still keeping one clear operating philosophy.

## Sources

- [Paperclip](https://github.com/paperclipai/paperclip)
- [paperclipai/companies gstack](https://github.com/paperclipai/companies/tree/main/gstack)
- [gstack](https://github.com/garrytan/gstack)
