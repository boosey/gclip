---
name: guard
description: >
  Hard-blocks destructive commands on critical paths. Intercepts rm -rf,
  DROP TABLE, force push to main, credential commits. Board override only.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /guard

Hard-blocks dangerous commands that could cause irreversible damage. Unlike `/careful` (warning only), `/guard` prevents execution entirely.

## What This Skill Does

- Intercepts and blocks destructive commands before execution
- Hard block: command does not execute, no confirmation bypass
- Covers: `rm -rf` on critical paths, `DROP TABLE/DATABASE`, `git push --force` to main/master, `git reset --hard`, credential file commits (`.env`, keys)
- All blocked commands are logged with full context
- Override requires board-level authorization

## Workflow

1. **Intercept**: Match incoming commands against the destructive command registry.
2. **Block**: Reject the command. Display the reason for blocking and the specific pattern matched.
3. **Log**: Record the blocked command, agent, timestamp, and matched pattern to the activity log.
4. **Override Path**: If the command is genuinely needed, the board can issue an explicit override. No agent-level bypass exists.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which commands to block | MECHANICAL | Fixed registry, not configurable by agents |
| Whether to block a matched command | MECHANICAL | Always blocked, no discretion |
| Override authorization | USER_CHALLENGE | Board-only decision |

## Safety Mechanisms

- Hard block — command is rejected, not just warned
- No agent-level override; only the board can authorize exceptions
- Pattern registry is immutable to agents
- Complementary to `/careful` (soft warning layer)

## Outputs & Persistence

- Block notification with matched pattern and risk explanation
- Activity log entry: command, pattern, agent, timestamp
- Override audit trail (if board grants exception)

## Invocation

```
/guard              # Enable guard (typically always-on)
/guard status       # Show blocked command registry
/guard log          # Show recent blocked commands
```
