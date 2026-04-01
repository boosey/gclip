---
name: freeze
description: >
  Locks editing to specified files or directories during active review or audit.
  Prevents accidental out-of-scope changes. Paired with /unfreeze.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /freeze

Restricts file editing to a specified scope during reviews or audits.

## What This Skill Does

- Locks editing to a whitelist of files or directories
- Prevents agents from accidentally modifying files outside the review scope
- Maintains freeze state until explicitly released via `/unfreeze`
- Logs scope boundaries and any blocked edit attempts to the activity log
- Does not affect read access — frozen files can still be read

## Workflow

1. **Scope Definition**: Accept file paths, glob patterns, or directory paths as the frozen scope.
2. **Activation**: Enable scope restriction. All subsequent file write operations are checked against the frozen scope.
3. **Enforcement**: Block write attempts outside the frozen scope. Log the blocked attempt with the file path and requesting agent.
4. **Persistence**: Maintain freeze state across heartbeats until `/unfreeze` is invoked.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which files to freeze | MECHANICAL | Specified by the invoker |
| Blocking out-of-scope writes | MECHANICAL | Hard enforcement, no exceptions |
| Whether to unfreeze early | USER_CHALLENGE | Only human or `/unfreeze` can release |

## Safety Mechanisms

- Hard block on out-of-scope writes (not a warning — edits are rejected)
- Read access is never restricted
- Freeze state persists until explicit `/unfreeze` — does not auto-expire
- All blocked attempts are logged with full context

## Outputs & Persistence

- Freeze state: active scope boundaries stored in session
- Activity log entries: scope activation, blocked attempts, scope release
- Scope summary available via `/freeze status`

## Invocation

```
/freeze <path...>           # Freeze specified files/directories
/freeze --glob "src/**"     # Freeze by glob pattern
/freeze status              # Show current freeze scope
```
