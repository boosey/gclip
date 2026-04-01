---
name: workflow-gate
description: >
  Enforces gstack's Think-Plan-Build-Review-Test-Ship-Reflect phase ordering
  on Paperclip's issue status machine. Runs as a pre-check at each heartbeat
  before an agent performs work.
metadata:
  sources:
    - kind: original
      repo: boosey/gclip
      attribution: gclip project
      license: MIT
      usage: workflow phase enforcement via Paperclip issue status
  requires_api:
    - GET /api/issues
---

# Workflow Gate

Enforces gstack's immutable 7-phase workflow on Paperclip's issue status machine. Runs as a pre-check at every heartbeat — if an agent's workflow phases don't match the current issue status, execution is blocked before any work begins.

## What This Skill Does

Validates that an issue's Paperclip status is appropriate for the agent about to act on it. This prevents out-of-order execution: a Release Engineer cannot ship code that hasn't been reviewed, a QA Engineer cannot test code still in progress.

### Phase-to-Status Mapping

| Phase | Valid Paperclip Status | Transition On Completion |
|---|---|---|
| THINK | backlog | backlog → todo |
| PLAN | todo | todo → in_progress |
| BUILD | in_progress | — (stays in_progress) |
| REVIEW | in_progress | in_progress → in_review |
| TEST | in_review | — (stays in_review) |
| SHIP | in_review | in_review → done |
| REFLECT | done | — (post-close, no transition) |

## Workflow

1. **Read Status** — GET /api/issues for the current issue. Extract the Paperclip status field.
2. **Read Agent Phases** — Look up the acting agent's `cognitive.workflow_phases` from AGENTS.md. This defines which phases the agent is permitted to operate in.
3. **Match** — Check if the issue's current status falls within any of the agent's valid phases using the mapping table above.
4. **Decide** —
   - **BLOCKED**: Status does not match any of the agent's phases. Log a finding to activity_log. Return `succeeded` with no work performed. The heartbeat completes cleanly — blocking is not a failure.
   - **PROCEED**: Status matches. Allow normal skill execution to continue.

## Decision Classification

| Scenario | Classification | Rationale |
|---|---|---|
| Status matches agent phase | MECHANICAL | One right answer: proceed |
| Status does not match | MECHANICAL | One right answer: block |
| REFLECT phase agent on done issue | MECHANICAL | Always proceed — REFLECT is post-close |
| Agent has no workflow_phases defined | MECHANICAL | Block — missing config is a hard stop |

All workflow-gate decisions are MECHANICAL. There are no taste or user-challenge scenarios — the phase ordering is immutable.

## Safety Mechanisms

- **Never modifies issue status** — the gate only reads status. Transitioning status is the executing agent's responsibility as its last action in a heartbeat.
- **Logs all blocks** — every BLOCKED decision is recorded as an activity_log entry with the agent name, expected status, actual status, and reason.
- **Does not block REFLECT phase agents** — agents with REFLECT in their workflow_phases always proceed on `done` issues. Retrospectives and evaluations must not be gated.
- **Returns succeeded on block** — a blocked heartbeat is a normal outcome, not an error. The agent simply performs no work and the issue stays in its current state.
- **Read-only API usage** — only issues a GET request; cannot corrupt state.

## Outputs & Persistence

- PROCEED → no output; execution continues to the agent's primary skill
- BLOCKED → activity_log entry with gate decision payload:
  ```json
  {
    "gate": "workflow-gate",
    "agent": "<agent-name>",
    "expected_status": ["<valid statuses>"],
    "actual_status": "<current status>",
    "action": "BLOCKED",
    "reason": "<human-readable explanation>"
  }
  ```

## Invocation

Workflow-gate is not invoked directly. It runs automatically as a pre-check at the start of every agent heartbeat. The Paperclip adapter calls it before dispatching to the agent's primary skill.
