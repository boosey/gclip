---
name: browse
description: >
  Persistent headless Chromium session with sub-200ms command latency.
  Supports navigate, click, fill, screenshot, JS evaluation, and wait commands.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /browse

Launches and manages a persistent headless Chromium browser session for automated interaction with web pages.

## What This Skill Does

- Launches headless Chromium with ~3s initial startup, then ~100-200ms per subsequent command
- Provides commands: navigate, click, fill, screenshot, evaluate (JS), wait
- Maintains session state (cookies, localStorage) across commands within the same session
- Auto-shuts down after 30 minutes of idle time
- Binds to localhost-only with Bearer token authentication

## Workflow

1. **Launch**: Start headless Chromium on a random port between 10000-60000. Generate Bearer token. Write connection details to `~/.gstack/browse.json`.
2. **Command Loop**: Accept commands via the connection. Each command executes against the current page context and returns results.
3. **Session Management**: Track idle time. Reset idle timer on each command. After 30 minutes idle, gracefully shut down the browser process.
4. **Cleanup**: On shutdown, remove connection file and release port.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Port selection | MECHANICAL | Random from range, retry on conflict |
| Idle timeout duration | TASTE | 30min default balances resource usage vs convenience |
| Whether to navigate to untrusted URLs | USER_CHALLENGE | Human must approve navigation to unknown domains |

## Safety Mechanisms

- Localhost-only binding — no remote access
- Bearer token authentication on every command
- Auto-shutdown on idle prevents orphaned browser processes
- Connection details stored with restricted file permissions

## Outputs & Persistence

- Connection file: `~/.gstack/browse.json` (port, token, PID)
- Screenshots saved to working directory when requested
- Session cookies and state persist in-memory only (not written to disk)

## Invocation

```
/browse                     # Launch new session
/browse navigate <url>      # Navigate to URL
/browse click <selector>    # Click element
/browse fill <selector> <text>  # Fill form field
/browse screenshot          # Capture current page
/browse eval <js-expression>    # Evaluate JavaScript
/browse wait <selector|ms>  # Wait for element or duration
```
