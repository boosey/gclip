---
name: evaluate
description: >
  Analyzes performance of ALL agents in the company — including the CEO
  and the Evaluator itself. Computes per-agent metrics, classifies findings
  by confidence and action type, and feeds verified improvements to
  /propose-improvement. No agent is exempt from evaluation.
metadata:
  sources:
    - kind: original
      repo: boosey/gclip
      attribution: gclip project
      license: MIT
      usage: agent performance analysis using Paperclip telemetry
  requires_api:
    - GET /api/heartbeat-runs
    - GET /api/cost-events
    - GET /api/issues
    - GET /api/activity-log
    - GET /api/documents
    - POST /api/documents
---

# Evaluate

Analyzes agent performance across Paperclip telemetry sources, classifies findings by confidence level and action type, and routes verified improvements through the board approval pipeline.

## What This Skill Does

Reads from five Paperclip data sources to build a composite performance picture per agent:

- **heartbeat_runs** — success/failure rates, duration per heartbeat
- **cost_events** — budget utilization, cost-per-issue
- **issues** — throughput, rework rate (backward status transitions), time-in-status
- **activity_log** — handoff delays between workflow phases
- **documents** — prior learnings, retros, evaluation history

## Workflow

1. **Gather** — Pull data from all five Paperclip API endpoints. Filter to the evaluation window (default: 7 days). Require minimum 5 data points per metric before computing.
2. **Compute** — Calculate per-agent metrics: heartbeat success rate, cost/issue, rework rate, phase duration ratio, budget utilization, learning velocity, handoff delay. Compare against alert thresholds from improvement-loop.md.
3. **Classify** — Cross confidence level with action type for each finding:
   - Confidence: **VERIFIED** (3+ corroborating data points) · **UNVERIFIED** (single observation) · **TENTATIVE** (statistical inference)
   - Action: **AUTO-FIX** (mechanical, one right answer) · **ASK** (taste decision, needs board input) · **CRITICAL** (user-challenge, blocks until resolved)
4. **Report** — Generate evaluation report as a Paperclip document via POST /api/documents. Include raw metrics, findings, and classification rationale.
5. **Route** — Pass VERIFIED+AUTO-FIX and VERIFIED+ASK findings to `/propose-improvement`. UNVERIFIED and TENTATIVE findings are stored for revisit next cycle.

## Decision Classification

| Finding Type | Classification | Example |
|---|---|---|
| Success rate below 90% with 10+ heartbeats | VERIFIED | Agent failing consistently |
| Single heartbeat timeout | UNVERIFIED | Could be transient |
| Cost trend rising over 3 data points | TENTATIVE | Statistical inference, not causal |
| Budget above 80% threshold | VERIFIED + CRITICAL | Hard operational limit |
| Zero learnings in 14 days | VERIFIED + ASK | May indicate scope issue or idle agent |

## Safety Mechanisms

- **Max 3 proposals per evaluation** — prevents change fatigue on the board.
- **Min 5 data points** required for any finding — prevents knee-jerk reactions from noise.
- **Self-evaluation required** — the Evaluator MUST include itself in every evaluation cycle. If proposal acceptance rate drops below 50% over 4 weeks, it must propose changes to its own evaluation criteria. Self-proposals follow the same Board approval path.
- **CEO evaluation escalation** — findings about the CEO bypass the CEO and go directly to the Board, since the CEO cannot objectively approve proposals about themselves.
- **168h cooldown** per agent per proposal type — prevents oscillating recommendations.
- **Findings below threshold are stored, not discarded** — they accumulate toward future VERIFIED status.

## Outputs & Persistence

- Evaluation report → POST /api/documents (type: `evaluation`, tagged with cycle date)
- VERIFIED findings → forwarded to `/propose-improvement`
- UNVERIFIED/TENTATIVE findings → stored in evaluation document for next cycle review

## Invocation

```
/evaluate                    # Run full evaluation for current cycle
/evaluate --agent <name>     # Evaluate a single agent (still subject to cooldown)
/evaluate --dry-run          # Compute metrics and classify without routing proposals
```
