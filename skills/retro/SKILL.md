---
name: retro
description: >
  Weekly self-analysis and team metrics. Gathers data from both git history
  and Paperclip telemetry API to produce retrospective artifacts with
  per-agent breakdowns and week-over-week deltas.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: gstack retro skill
      license: MIT
      usage: vendored
  requires_api:
    - GET /api/heartbeat-runs
    - GET /api/cost-events
    - GET /api/issues
    - GET /api/activity-log
    - GET /api/documents
    - POST /api/documents
---

# Retro

Weekly self-analysis and team metrics forked from gstack. Modified to gather data from both git history and Paperclip telemetry API, producing combined retrospective artifacts with per-agent breakdowns and week-over-week trend deltas.

## What This Skill Does

Produces a weekly retrospective artifact that combines code-level signals (git) with operational signals (Paperclip telemetry). The retro covers team-wide metrics, per-agent performance, and actionable habits for the next week.

### Data Sources

| Source | What It Provides |
|---|---|
| `git log` | Commits, lines changed, authors, file churn |
| `heartbeat_runs` | Per-agent success/failure rates, duration |
| `cost_events` | Budget spend trends, cost-per-issue |
| `issues` | Throughput, rework count, time-to-done |
| `activity_log` | Communication patterns, handoff timing |
| Prior retros | Week-over-week comparison baselines |

## Workflow

1. **Collect Git Data** — Run `git log --since=7.days` to gather commits, LOC deltas, and author activity. Group by agent (mapped from git author to agent_id).
2. **Collect Telemetry** — Pull from Paperclip APIs: heartbeat_runs, cost_events, issues, activity_log for the past 7 days.
3. **Load Baselines** — Read prior retro from `.context/retros/` or GET /api/documents (type: `retro`). Extract last week's metrics for delta computation.
4. **Compute Team Metrics** — Issues closed, heartbeat success rate, total cost, rework rate, average time-to-done. Calculate week-over-week deltas.
5. **Compute Per-Agent Metrics** — For each agent: review count, investigation count, learnings produced, commits authored. Include:
   - **Praise** — commit-anchored recognition (reference specific commits)
   - **Growth** — one actionable suggestion for improvement
6. **Select Highlights** — Ship of the week (most impactful delivery). Three habits for next week (derived from what went well and what didn't).
7. **Persist** — POST /api/documents (type: `retro`) and write to `.context/retros/{date}.md`.

## Decision Classification

| Scenario | Classification | Rationale |
|---|---|---|
| Metric computation | MECHANICAL | Deterministic math from data |
| Ship of the week selection | TASTE | Judgment call on impact; surfaced in retro |
| Per-agent growth suggestion | TASTE | Requires interpretation of performance data |
| Flagging agent for review | USER_CHALLENGE | Board decides on agent-level actions |

## Safety Mechanisms

- **Read-only data access** — retro only reads from git and Paperclip APIs. It never modifies issues, agents, or budgets.
- **Praise is commit-anchored** — recognition references specific work, never vague or generic.
- **Growth suggestions are singular** — one actionable item per agent per week. Multiple suggestions dilute focus.
- **Prior retro required for deltas** — if no baseline exists, metrics are reported as absolute values without delta claims.
- **No self-retro** — the retro skill does not generate a per-agent section for the agent running it (typically CTO).

## Outputs & Persistence

- Retro document → POST /api/documents (type: `retro`, tagged with ISO week)
- Local archive → `.context/retros/{YYYY-MM-DD}.md`
- Format: team summary, per-agent sections, ship of the week, next-week habits

## Invocation

```
/retro                       # Run weekly retro for current project
/retro global                # Cross-project retro (aggregates all project data)
/retro --week <YYYY-Www>     # Generate retro for a specific past week
```
