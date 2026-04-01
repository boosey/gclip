# Sprint Lifecycle: gstack Phases on Paperclip Infrastructure

Maps gstack's immutable 7-phase workflow to Paperclip's issue status machine.

## Phase-to-Status Mapping

| Phase | Paperclip Status Transition | Active Agent(s) | Skills Invoked | Gate to Next Phase |
|---|---|---|---|---|
| **THINK** | backlog → todo | CEO | office-hours, plan-ceo-review | CEO marks issue as `todo` |
| **PLAN** | todo → in_progress | CTO | plan-eng-review, plan-design-review, autoplan | CTO marks issue as `in_progress` |
| **BUILD** | in_progress | Staff Engineer | (adapter executes implementation) | Staff Engineer marks `in_review` when code complete |
| **REVIEW** | in_progress → in_review | Staff Engineer, CSO | review, investigate, cso-audit, guard | All review findings resolved or deferred |
| **TEST** | in_review | QA Engineer | qa, qa-only, benchmark, browse, canary | QA passes or blocks |
| **SHIP** | in_review → done | Release Engineer | ship, land-and-deploy, document-release | Deployed and verified in production |
| **REFLECT** | done (post-close) | Evaluator, CTO | retro, evaluate, learn | Retro artifact stored |

## Enforcement

The `workflow-gate` skill runs as a pre-check at each heartbeat. Before an agent performs work, it:

1. Reads the current issue status
2. Checks if the status is appropriate for the agent's `cognitive.workflow_phases`
3. If mismatched: blocks execution, logs a finding, returns `succeeded` with no work performed
4. If matched: proceeds with normal skill execution

### Example: Blocked Execution

```json
{
  "gate": "workflow-gate",
  "agent": "release-engineer",
  "expected_status": ["in_review"],
  "actual_status": "in_progress",
  "action": "BLOCKED",
  "reason": "Issue has not passed review phase. Cannot ship unreviewed code."
}
```

## Phase Transitions

Each agent changes issue status as its **last action** in a heartbeat, signaling handoff to the next phase:

- CEO: backlog → todo (thinking complete, ready for planning)
- CTO: todo → in_progress (plan locked, ready for implementation)
- Staff Engineer: in_progress → in_review (code + review complete)
- QA Engineer: leaves in_review (test complete, ready for ship)
- Release Engineer: in_review → done (shipped and verified)
- Evaluator/CTO: done triggers reflect (retro + evaluation)

## Backward Transitions (Rework)

Issues can move backward when quality gates fail:

- QA finds critical bug → in_review → in_progress (back to Build)
- CSO finds security issue → in_review → in_progress (back to Build)
- Canary detects regression → done → in_progress (hotfix cycle)

Backward transitions increment the issue's `rework_count`. The Evaluator tracks rework rate as a key performance metric.
