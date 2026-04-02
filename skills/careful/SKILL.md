---
name: careful
description: >
  Safety guardrail that warns before destructive or risky operations.
  Intercepts dangerous commands and requires explicit confirmation without blocking.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /careful

Soft safety guardrail that warns before destructive operations without blocking them.

## What This Skill Does

- Intercepts commands matching known destructive patterns before execution
- Displays a warning with the specific risk and blast radius
- Requires explicit confirmation to proceed (does not auto-block)
- Logs all warnings to the activity log for audit trail
- Complements `/guard` (which hard-blocks); `/careful` only warns

## Workflow

1. **Intercept**: Match incoming commands against the destructive pattern registry.
2. **Assess**: Classify the risk level and describe the blast radius (what would be affected).
3. **Warn**: Display the warning with risk details. Wait for explicit confirmation.
4. **Log**: Record the warning, user response, and outcome to the activity log regardless of whether the user proceeds.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which patterns to intercept | MECHANICAL | Fixed registry: rm -rf, DROP TABLE, force push, branch deletion, credential exposure |
| Warning message content | MECHANICAL | Templated per pattern type |
| Whether to proceed after warning | USER_CHALLENGE | Human always decides |

## Safety Mechanisms

- Warning-only — never blocks execution, only delays until confirmation
- All warnings logged for audit trail even if user proceeds
- Pattern registry is append-only; patterns cannot be removed by agents
- Distinct from `/guard` (hard block) to allow layered safety

## Outputs & Persistence

- Warning displayed to user with risk classification
- Activity log entry: command, pattern matched, risk level, user decision, timestamp

## Invocation

```
/careful              # Enable careful mode for current session
/careful off          # Disable careful mode
/careful status       # Show current interception patterns
```
