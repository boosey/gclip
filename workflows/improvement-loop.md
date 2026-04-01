# Improvement Loop: Self-Improving Agent Organization

The weekly cycle that enables gclip agents to evolve through observation, analysis, and board-approved changes.

## Cycle Overview

```
Week N: Normal sprint work (Think → Plan → Build → Review → Test → Ship)
  |
  |  All agents run /learn on operational discoveries
  v
Sunday 23:00 UTC — CTO heartbeat → /retro (CIO reviews Monday)
  |
  |  Weekly metrics artifact produced
  v
Monday 02:00 UTC — Evaluator heartbeat → /evaluate
  |
  |  Performance analysis + findings classification
  |
  +-- VERIFIED findings → /propose-improvement → Board
  |     |
  |     +-- APPROVED → Learning Curator applies change
  |     +-- REJECTED → logged as learning (prevents re-proposal)
  |
  +-- UNVERIFIED/TENTATIVE → stored, revisited next cycle
  v
Monday 06:00 UTC — Learning Curator heartbeat
  |
  +-- Applies board-approved proposals (creates PRs)
  +-- Prunes stale learnings (>90 days, confidence < 0.5)
  +-- Promotes learnings (TENTATIVE → VERIFIED on confirmations)
  +-- Consolidates duplicate learnings
  v
Week N+1: Agents operate with updated definitions + learnings
```

## Evaluator Metrics

| Metric | Source | Alert Threshold |
|---|---|---|
| Heartbeat success rate | heartbeat_runs | < 90% |
| Cost per issue closed | cost_events + issues | Rising trend over 3 weeks |
| Rework rate | issues (backward status transitions) | > 15% |
| Phase duration ratio | issues (time-in-status) | Bottleneck > 2x average |
| Budget utilization | agents.spent_monthly_cents | > 80% |
| Learning velocity | learnings.json | 0 new learnings in 2 weeks |
| Handoff delay | activity_log timestamps | > 2 hours between phases |

## Proposal Types

| Type | Requires Board Approval | Example |
|---|---|---|
| `skill_addition` | Yes | Add /benchmark to Staff Engineer |
| `skill_removal` | Yes | Remove /codex from CTO (unused 60 days) |
| `budget_adjustment` | Yes | Increase QA budget (hitting 80% threshold) |
| `operational_limit_change` | Yes | Raise max_fixes_per_heartbeat |
| `workflow_reorder` | Yes | Insert CSO audit between Review and Test |
| `learning_promotion` | No (auto) | TENTATIVE → VERIFIED on 5 confirmations |
| `agent_hire` | Yes | New Design Engineer agent |
| `agent_termination` | Yes | Absorb Learning Curator into Evaluator |

## Guardrails

- **Max 3 proposals per week** — prevents change fatigue
- **168h cooldown** per agent per proposal type — prevents oscillation
- **Min 5 data points** required for any proposal — prevents knee-jerk reactions
- **Board rejection is a learning** — Evaluator records why and does not re-propose the same change
- **No self-modification** — Evaluator cannot propose changes to its own definition
