# gclip

A hybrid agent company definition that runs on [Paperclip](https://github.com/paperclipai/paperclip) infrastructure with [gstack](https://github.com/garrytan/gstack) cognitive methodology.

## What is gclip?

gclip merges two open-source AI agent frameworks into a single opinionated company definition:

- **Paperclip** provides multi-agent orchestration: heartbeats, budgets, board governance, adapter abstraction, and telemetry.
- **gstack** provides cognitive methodology: decision triage, cognitive gearing, workflow phases, anti-sycophancy, and operational limits.

gclip is the superset. It takes Paperclip's infrastructure and injects gstack's workflow rigor, then adds two novel agents (Evaluator and Learning Curator) that create a self-improvement loop neither parent project has.

## Comparison

| Capability | Paperclip | gstack | gclip |
|---|---|---|---|
| Multi-agent orchestration | Yes | No (single-agent) | Yes (from Paperclip) |
| Heartbeat scheduling | Yes | No | Yes (from Paperclip) |
| Board governance | Yes | No | Yes (from Paperclip) |
| Budget management | Yes | No | Yes (from Paperclip) |
| Adapter abstraction | Yes (Claude, GPT, etc.) | No (Claude only) | Yes (from Paperclip) |
| Cognitive gearing | No | Yes | Yes (from gstack) |
| Decision triage | No | Yes | Yes (from gstack) |
| Workflow phases | No | Yes (7 phases) | Yes (mapped to Paperclip statuses) |
| Anti-sycophancy | No | Yes | Yes (from gstack) |
| Operational limits | No | Yes | Yes (from gstack) |
| Self-improvement loop | No | No | Yes (novel) |
| Institutional memory | No | No | Yes (novel) |

## Org Chart

```
                  BOARD (Human)
                      |
                      v
                    CEO --------------- Evaluator
                 (business)          (self-improvement)
               /    |    \    \           |
             CIO  CFO  COO  CMO    Learning Curator
            (dev   ...  ...  ...
           process)
              |
             CTO
          (tech lead)
         /   |    \    \    \
      Staff Rel.  QA  CSO  UX
      Eng   Eng   Eng (sec) Designer
```

## Agent Inventory

| Agent | Title | Skills | Budget | Heartbeat |
|---|---|---|---|---|
| CEO | Chief Executive Officer | office-hours | $1,000 | Monday (planning) |
| CIO | Chief Information Officer | autoplan, office-hours, plan-ceo-review, workflow-gate, learn | $800 | Monday (planning) + on demand |
| CTO | Chief Technology Officer | plan-eng-review, plan-design-review, retro, codex | $800 | On demand + Sunday 23:00 UTC (retro) |
| Staff Engineer | Staff Engineer | review, investigate, guard, learn | $500 | On demand (build/review) |
| Release Engineer | Release Engineer | ship, land-and-deploy, document-release, setup-deploy | $300 | On demand (ship phase) |
| QA Engineer | QA Engineer | browse, qa, qa-only, benchmark, canary, setup-browser-cookies | $400 | On demand (test phase) |
| CSO | Chief Security Officer | cso-audit, investigate, careful, freeze, unfreeze, guard | $300 | On demand (review phase) |
| UX Designer | UX Designer | design-consultation, design-review, design-html, design-shotgun, user-research, wireframe, information-architecture, interaction-design, ab-test-design, browse, learn | $400 | On demand (plan/review) |
| Evaluator | Agent Performance Evaluator | evaluate, propose-improvement, retro, learn | $600 | Monday 02:00 UTC |
| Learning Curator | Institutional Memory Manager | learn, retro | $200 | Monday 06:00 UTC |
| | | **Total** | **$5,300/mo** | |

## Skill Inventory

### Strategic
| Skill | Used By | Purpose |
|---|---|---|
| office-hours | CEO, CIO | Problem framing — CEO for business strategy, CIO for engineering initiatives |
| plan-ceo-review | CIO | 10-stage scope challenge review (EXPANSION / HOLD / REDUCTION) |
| autoplan | CIO | Orchestrate CIO → Design → Eng review pipeline |

### Engineering
| Skill | Used By | Purpose |
|---|---|---|
| plan-eng-review | CTO | Engineering plan review and approval |
| plan-design-review | CTO | Design review for API contracts and data models |
| codex | CTO | Code search and architectural exploration |
| review | Staff Engineer | Two-pass adversarial code review |
| investigate | Staff Engineer, CSO | Root-cause investigation with 3-strike rule |

### Quality
| Skill | Used By | Purpose |
|---|---|---|
| qa | QA Engineer | Full diff-aware test suite execution |
| qa-only | QA Engineer | Targeted test run without fix loop |
| browse | QA Engineer | Browser automation via persistent Chromium |
| benchmark | QA Engineer | Performance benchmarking against baseline |
| canary | QA Engineer | Post-deploy canary monitoring |

### Operations
| Skill | Used By | Purpose |
|---|---|---|
| ship | Release Engineer | Pre-merge gate checklist |
| land-and-deploy | Release Engineer | Merge, deploy, and verify production health |
| document-release | Release Engineer | Post-ship documentation updates |
| setup-deploy | Release Engineer | Deploy environment configuration |
| workflow-gate | CIO | Phase enforcement pre-check at each heartbeat |

### Design
| Skill | Used By | Purpose |
|---|---|---|
| user-research | UX Designer | User interviews, personas, journey maps, usability test plans |
| wireframe | UX Designer | Low-fidelity wireframes and clickable HTML prototypes |
| information-architecture | UX Designer | Navigation structure, taxonomy, content hierarchy |
| design-consultation | UX Designer | Build design systems (colors, typography, spacing, motion, components) |
| design-review | UX Designer | Live site visual audit with 80-item checklist and CSS-first fixes |
| design-html | UX Designer | Convert mockups to production React/Svelte/Vue components |
| design-shotgun | UX Designer | Generate 3 visual variants for rapid design exploration |
| interaction-design | UX Designer | Micro-interactions, state transitions, loading patterns, motion |
| ab-test-design | UX Designer | Experiment design with hypotheses, metrics, and decision rules |

### Safety
| Skill | Used By | Purpose |
|---|---|---|
| cso-audit | CSO | 14-phase OWASP + STRIDE security audit |
| careful | CSO | Conservative mode for security-sensitive operations |
| freeze | CSO | Lock files under active security review |
| unfreeze | CSO | Release scope lock after review completion |
| guard | Staff Engineer, CSO | Block destructive commands |

### Self-Improvement
| Skill | Used By | Purpose |
|---|---|---|
| evaluate | Evaluator | Performance analysis from Paperclip telemetry |
| propose-improvement | Evaluator | Generate board proposals from verified findings |
| retro | CTO, Evaluator, Learning Curator | Weekly retrospective metrics |
| learn | Staff Engineer, Evaluator, Learning Curator | Log operational learnings |

## Self-Improvement Loop

gclip agents evolve through a weekly observe-analyze-propose-apply cycle:

1. **Observe** -- All agents log operational learnings via `/learn` during normal work
2. **Retro** -- CTO produces weekly metrics (Sunday 23:00 UTC)
3. **Evaluate** -- Evaluator analyzes performance, classifies findings (Monday 02:00 UTC)
4. **Propose** -- VERIFIED findings become board proposals (max 3/week)
5. **Approve** -- Board votes on proposals (human decision)
6. **Apply** -- Learning Curator implements approved changes as PRs (Monday 06:00 UTC)
7. **Prune** -- Stale learnings removed, confirmed learnings promoted

Guardrails: 168h cooldown per proposal type, min 5 data points required, no self-modification, board rejections logged as learnings.

## Quick Start

```bash
paperclip company import --from ./gclip
```

This imports all 10 agents, 37 skills, workflows, and the self-improvement loop into your Paperclip instance.

## Sources

- [Paperclip](https://github.com/paperclipai/paperclip) -- Multi-agent orchestration platform (MIT)
- [gstack](https://github.com/garrytan/gstack) -- Cognitive workflow framework for AI agents (MIT)

## License

MIT -- see [LICENSE](LICENSE) for details.
