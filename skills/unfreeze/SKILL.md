---
name: unfreeze
description: >
  Removes scope restriction set by /freeze. Restores normal editing permissions
  after review or audit completion. Requires explicit invocation.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /unfreeze

Removes the file editing scope restriction set by `/freeze`.

## What This Skill Does

- Releases the scope restriction created by `/freeze`
- Restores normal editing permissions for all agents
- Logs the scope release to the activity log with timestamp and invoker
- Requires explicit invocation — freeze does not auto-expire
- Paired with `/freeze`: together they provide scoped editing control during reviews

## Workflow

1. **Verification**: Confirm an active freeze scope exists. If no freeze is active, report and exit.
2. **Release**: Remove the scope restriction. All file paths are now writable.
3. **Logging**: Record the unfreeze event: timestamp, invoker, previous scope boundaries, duration of freeze.
4. **Confirmation**: Report the unfreeze with a summary of what was previously restricted.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Whether to unfreeze | MECHANICAL | Explicit user/agent invocation required |
| Partial unfreeze (subset of frozen paths) | TASTE | Supported if specified; defaults to full release |

## Safety Mechanisms

- Does not auto-trigger — freeze persists until explicitly unfrozen
- Full audit trail: freeze activation, blocked attempts during freeze, and unfreeze event all logged
- Reports previous scope boundaries so the invoker knows what was restricted

## Outputs & Persistence

- Unfreeze confirmation with previous scope summary
- Activity log entry: timestamp, invoker, previous scope, freeze duration
- Freeze state cleared from session

## Invocation

```
/unfreeze                   # Release all frozen paths
/unfreeze <path...>         # Release specific paths (partial unfreeze)
```
