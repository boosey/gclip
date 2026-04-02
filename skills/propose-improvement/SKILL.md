---
name: propose-improvement
description: >
  Formats improvement proposals into Paperclip board approval payloads.
  Takes classified findings from /evaluate and generates structured
  approval requests with evidence, diffs, and rollback plans.
metadata:
  sources:
    - kind: github-file
      repo: paperclipai/companies
      attribution: PaperclipAI
      license: MIT
      usage: vendored
  requires_api:
    - POST /api/approvals
    - GET /api/approvals
    - POST /api/documents
---

# Propose Improvement

Formats classified findings from `/evaluate` into structured board approval requests. Each proposal includes evidence, a proposed change as a diff, and a mandatory rollback plan.

## What This Skill Does

Bridges the gap between evaluation findings and board action. Takes VERIFIED findings and packages them as approval payloads that the board can review, approve, or reject. Supports seven proposal types:

- `skill_addition` — Add a skill to an agent's definition
- `skill_removal` — Remove an unused or underperforming skill
- `budget_adjustment` — Modify an agent's monthly spend limit
- `operational_limit_change` — Adjust hard limits (max_fixes, thresholds)
- `workflow_reorder` — Insert, remove, or reorder workflow phases
- `agent_hire` — Propose a new agent with full definition
- `agent_termination` — Remove an agent (irreversible, requires strong evidence)

## Workflow

1. **Receive** — Accept classified findings from `/evaluate`. Each finding includes confidence level, action type, source metrics, and affected agent.
2. **Cooldown Check** — Query GET /api/approvals for prior proposals. If a proposal of the same type for the same agent was submitted within 168h, skip it and log the reason.
3. **Evidence Validation** — Verify the finding has minimum 5 data points. Reject findings that don't meet threshold — log as "insufficient evidence" in activity_log.
4. **Format Proposal** — Build the approval payload:
   - `summary` — One-line description of the proposed change
   - `evidence` — Metrics, data points, and finding classification
   - `proposed_change` — Diff format showing before/after state
   - `rollback_plan` — Exact steps to revert if the change causes regression
   - `proposal_type` — One of the seven types above
5. **Submit** — POST /api/approvals with the formatted payload.
6. **Record** — Log the proposal as a learning via POST /api/documents regardless of eventual outcome. Board rejections include the rejection reason to prevent re-proposal.

## Decision Classification

| Scenario | Classification | Action |
|---|---|---|
| Formatting proposal fields | MECHANICAL | Auto — deterministic mapping |
| Choosing between skill_removal vs budget_adjustment | TASTE | Evaluator decides based on evidence weight |
| Proposing agent_termination | USER_CHALLENGE | Board must approve; never auto-decided |
| Cooldown conflict | MECHANICAL | Auto — skip and log |

## Safety Mechanisms

- **Never proposes changes to the Evaluator's own definition** — self-modification is prohibited.
- **Board rejection is a learning** — rejected proposals are logged with the rejection reason. The same proposal type + agent combination cannot be re-submitted until new evidence emerges.
- **Rollback plan required** — every proposal must include explicit revert steps. Proposals without rollback plans are rejected before submission.
- **168h cooldown enforced** — duplicate proposal types for the same agent within the window are silently skipped.
- **Max 3 proposals per evaluation cycle** — excess findings are queued for next cycle.

## Outputs & Persistence

- Approval request → POST /api/approvals
- Proposal record → POST /api/documents (type: `proposal`, includes outcome when resolved)
- Skipped proposals → logged in activity_log with reason (cooldown, insufficient evidence)

## Invocation

```
/propose-improvement                    # Process all pending findings from /evaluate
/propose-improvement --finding <id>     # Submit a single finding as a proposal
/propose-improvement --dry-run          # Format proposals without submitting to board
```
