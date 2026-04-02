---
name: qa
description: >
  Diff-aware systematic testing with fixes. Fix loop: locate, fix, commit,
  re-test, classify. Max 50 fixes with WTF-likelihood safety heuristic.
  Generates regression tests for behavioral bugs.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /qa

Diff-aware testing that finds bugs, fixes them, and generates regression tests.

## What This Skill Does

- Reads git diff to identify affected pages, components, and code paths
- Runs systematic tests against affected areas
- Fix loop: locate bug, fix, commit, re-test, classify result
- Generates regression tests for behavioral bugs
- Three modes: Quick (smoke test), Standard (full affected area), Exhaustive (deep edge cases)
- Safety limits: max 50 fixes, WTF-likelihood heuristic stops at 20% cumulative risk

## Workflow

1. **Diff Analysis**: Read git diff. Map changes to affected pages, components, API routes, and test files.
2. **Test Execution**: Run existing tests for affected areas. If no tests exist, generate and run smoke tests.
3. **Bug Detection**: Identify failures, visual regressions, and behavioral anomalies.
4. **Fix Loop**: For each bug: locate root cause, apply fix, commit with descriptive message, re-test to confirm fix, classify outcome (FIXED, PARTIAL, REGRESSED, SKIPPED).
5. **Regression Tests**: For each behavioral bug fixed, generate a regression test that fails without the fix.
6. **Safety Check**: Track cumulative WTF-likelihood. Stop if cumulative risk exceeds 20% or fix count reaches 50.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which areas to test | MECHANICAL | Derived from git diff |
| Fix approach per bug | TASTE | Agent selects simplest correct fix |
| Whether to skip a risky fix | TASTE | WTF-likelihood heuristic, auto-decided |
| Mode selection (Quick/Standard/Exhaustive) | TASTE | Standard default; user can override |
| Whether to proceed past 20% WTF threshold | USER_CHALLENGE | Human decides on risky fix batches |

## Safety Mechanisms

- Max 50 fixes per session (hard limit from operational limits)
- WTF-likelihood heuristic: stops at 20% cumulative risk of surprising outcomes
- Each fix is individually committed for easy revert
- Regression tests validate fixes don't mask deeper issues

## Outputs & Persistence

- Test results: per-area pass/fail with details
- Fix commits: individually committed and classifiable
- Regression tests: committed alongside fixes
- QA report: affected areas, bugs found, fixes applied, cumulative risk score

## Invocation

```
/qa                     # Standard mode (diff-aware)
/qa --quick             # Smoke test only
/qa --exhaustive        # Deep edge case testing
/qa --no-fix            # Equivalent to /qa-only
```
