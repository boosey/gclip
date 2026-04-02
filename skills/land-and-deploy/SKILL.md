---
name: land-and-deploy
description: >
  Merges PR, monitors CI pipeline, verifies production health checks, and
  auto-triggers canary monitoring. Recommends rollback on failure.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /land-and-deploy

Merges a PR and monitors the full deployment pipeline through to production health verification.

## What This Skill Does

- Merges the specified PR after confirming green CI status
- Monitors the CI/CD pipeline through deployment completion
- Verifies production health checks post-deploy
- Automatically triggers `/canary` for regression detection
- Recommends rollback if health checks or canary monitoring fail
- Logs deployment metadata: commit SHA, timestamp, environment

## Workflow

1. **Pre-Merge Check**: Verify CI is green on the PR. Confirm no merge conflicts. Refuse to proceed if CI is failing.
2. **Merge**: Merge the PR using the project's configured merge strategy (squash, rebase, or merge commit).
3. **Pipeline Monitor**: Watch the CI/CD pipeline. Report stage progress. Alert on pipeline failures.
4. **Health Check**: After deployment completes, hit production health check endpoints. Verify expected responses.
5. **Canary**: Trigger `/canary` to run post-deploy regression monitoring. Report canary results.
6. **Metadata**: Log deployment record with commit SHA, timestamp, environment, pipeline duration, and canary verdict.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Merge strategy | MECHANICAL | Uses project-configured strategy |
| Whether to proceed with failing CI | USER_CHALLENGE | Never auto-proceeds; human must override |
| Rollback recommendation | TASTE | Recommended based on canary/health data; human executes |
| Whether to execute rollback | USER_CHALLENGE | Human always decides on rollback |

## Safety Mechanisms

- Will not merge with failing CI — hard gate
- Rollback is recommended, never auto-executed
- Deployment metadata logged for audit and incident response
- Canary monitoring is automatic post-deploy, not optional

## Outputs & Persistence

- Deployment record: commit SHA, timestamp, environment, pipeline status, canary verdict
- Canary report (via `/canary`)
- Rollback recommendation with evidence (if applicable)

## Invocation

```
/land-and-deploy <pr-number>
/land-and-deploy --skip-canary <pr-number>   # Skip post-deploy canary (not recommended)
```
