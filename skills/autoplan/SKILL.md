---
name: autoplan
description: >
  Orchestrates sequential CEO, Design, and Engineering reviews on a plan.
  Runs parallel Claude+Codex subagents per phase with consensus gating.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /autoplan

Runs a plan through the full CEO → Design → Engineering review pipeline, enforcing sequential gates with parallel subagent execution within each phase.

## What This Skill Does

- Orchestrates three review phases in strict sequence: CEO review, Design review, Engineering review
- Spawns parallel Claude and Codex subagents within each phase for adversarial coverage
- Builds a consensus table at each gate summarizing agreements and disagreements
- Applies the six auto-decision principles for TASTE-level decisions
- Produces a fully approved plan with all review artifacts attached

## Workflow

1. **CEO Review (blocking)**: Invoke `/plan-ceo-review` via parallel Claude+Codex subagents. Merge findings into a consensus table. Gate: plan must pass strategic review before proceeding.
2. **Design Review (conditional)**: If the plan involves UI or user-facing changes, invoke `/plan-design-review` via parallel subagents. Skip if no UI surface detected. Gate: design scores must meet thresholds.
3. **Engineering Review (blocking)**: Invoke `/plan-eng-review` via parallel subagents. Merge findings into final consensus table. Gate: no unresolved CRITICAL items.
4. **Approval Synthesis**: Combine all review artifacts into a single approved plan document with consolidated recommendations.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Review sequencing order | MECHANICAL | Fixed pipeline: CEO → Design → Eng |
| Skip design review when no UI | TASTE | Auto-decided via six principles (pragmatic) |
| Resolve subagent disagreements | TASTE | Consensus table surfaces conflicts, six principles resolve |
| Override a gate failure | USER_CHALLENGE | Human must decide whether to proceed despite review failure |

## Safety Mechanisms

- Each gate is blocking — downstream phases cannot begin until the prior gate passes
- Consensus table explicitly surfaces disagreements rather than silently averaging
- Codex subagent has filesystem boundary: cannot read skill definitions
- 3-strike rule: if a phase fails consensus three times, escalate to board

## Outputs & Persistence

- Approved plan document with all review annotations
- Consensus tables for each phase (stored in activity log)
- Review metadata: timestamps, subagent versions, gate pass/fail status

## Invocation

```
/autoplan <plan-file-or-description>
```
