---
name: setup-browser-cookies
description: >
  Imports real browser sessions into /browse for authenticated testing.
  Reads cookies from Chrome, Arc, Brave, Edge via OS credential stores.
  Cookies stored in memory only, never persisted to disk.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /setup-browser-cookies

Imports authenticated browser sessions into the `/browse` headless Chromium instance.

## What This Skill Does

- Reads cookies from installed browsers: Chrome, Arc, Brave, Edge
- Decrypts cookies via macOS Keychain or Windows Credential Store
- Injects cookies into the active `/browse` Chromium session
- Enables authenticated testing without manual login flows
- Cookies stored in memory only — never written to disk

## Workflow

1. **Browser Detection**: Identify installed browsers and their cookie database locations.
2. **Cookie Extraction**: Read and decrypt cookies from the selected browser's cookie store using OS-level credential APIs (macOS Keychain, Windows DPAPI).
3. **Domain Filtering**: If a target domain is specified, extract only cookies for that domain. Otherwise, extract all cookies.
4. **Injection**: Inject cookies into the active `/browse` session. Verify injection by checking cookie presence on a test navigation.
5. **Cleanup**: Cookies exist only in the `/browse` session memory. No disk persistence.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which browser to extract from | TASTE | Auto-detect primary browser; user can specify |
| Domain filtering | MECHANICAL | Specified by user or defaults to all |
| Whether to import sensitive cookies | USER_CHALLENGE | Human must explicitly approve session import |

## Safety Mechanisms

- Cookies stored in memory only — never persisted to disk
- Domain filtering prevents unnecessary cookie exposure
- Requires active `/browse` session — will not start one implicitly
- Cookie injection is session-scoped; destroyed when `/browse` session ends
- Explicit user approval required before extracting cookies

## Outputs & Persistence

- Cookie injection confirmation with domain count and cookie count
- No disk persistence — cookies exist only in browser session memory
- Session-scoped: cookies are lost when `/browse` session terminates

## Invocation

```
/setup-browser-cookies                          # Auto-detect browser, all domains
/setup-browser-cookies --browser chrome         # Specific browser
/setup-browser-cookies --domain example.com     # Specific domain only
```
