---
name: canary
description: >
  Post-deploy monitoring that checks for console errors, performance regressions,
  visual anomalies, and HTTP error spikes. Triggers rollback recommendations on failure.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /canary

Monitors production health after deployment by comparing post-deploy behavior against pre-deploy baselines.

## What This Skill Does

- Checks for new console errors not present in the pre-deploy baseline
- Detects performance regressions by comparing against `/benchmark` baselines
- Identifies visual anomalies via screenshot comparison
- Monitors HTTP error rate spikes (4xx/5xx) in the deployment window
- Triggers rollback recommendation if regressions exceed thresholds

## Workflow

1. **Baseline Load**: Retrieve pre-deploy baseline from the most recent `/benchmark` run or stored canary snapshot.
2. **Health Checks**: Navigate to monitored endpoints. Collect console output, performance metrics, screenshots, and HTTP status codes.
3. **Comparison**: Diff console errors against baseline (new errors only). Compare Core Web Vitals against baseline thresholds. Run visual diff on screenshots. Aggregate HTTP error rates.
4. **Verdict**: If any check exceeds regression thresholds, generate rollback recommendation with specific evidence. Otherwise, mark deployment as healthy.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which endpoints to monitor | MECHANICAL | Derived from project config or previous canary runs |
| Regression threshold sensitivity | TASTE | Default thresholds, adjustable per project |
| Whether to execute rollback | USER_CHALLENGE | Rollback is recommended, never auto-executed |

## Safety Mechanisms

- Read-only monitoring — does not modify production state
- Rollback is a recommendation, not an automatic action
- Pre-deploy baseline required; refuses to run without comparison data
- Time-bounded: monitoring window configurable (default 10 minutes)

## Outputs & Persistence

- Canary report: per-check pass/fail with evidence
- Rollback recommendation (if applicable) with specific regression data
- Post-deploy snapshot stored as new baseline for future canary runs

## Invocation

```
/canary                     # Run post-deploy monitoring
/canary --baseline <id>     # Compare against specific baseline
/canary --duration <minutes>  # Set monitoring window
```
