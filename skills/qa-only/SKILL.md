---
name: qa-only
description: >
  Read-only test mode. Same analysis as /qa but reports findings without
  applying fixes. Used when QA should not modify the codebase.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /qa-only

Read-only testing that reports findings without modifying code.

## What This Skill Does

- Same diff-aware analysis and testing as `/qa`
- Does not apply any fixes — strictly read-only
- Reports findings with severity classification and suggested fixes
- Used when the codebase should not be touched during QA (e.g., pre-release freeze, external audit)

## Workflow

1. **Diff Analysis**: Read git diff. Map changes to affected pages, components, API routes, and test files.
2. **Test Execution**: Run existing tests for affected areas. Generate and run smoke tests for uncovered areas.
3. **Bug Detection**: Identify failures, visual regressions, and behavioral anomalies.
4. **Classification**: For each finding, classify severity (CRITICAL, HIGH, MEDIUM, LOW) and provide a suggested fix with affected file and line references.
5. **Report**: Generate findings report without applying any changes.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which areas to test | MECHANICAL | Derived from git diff |
| Severity classification | TASTE | Based on impact and blast radius |
| Suggested fix approach | TASTE | Recommended but not applied |
| Whether to fix any findings | USER_CHALLENGE | Human decides; this mode explicitly does not fix |

## Safety Mechanisms

- Strictly read-only — no file modifications, no commits
- Same analytical depth as `/qa` without the mutation risk
- Severity classification helps prioritize human review
- Can be safely run during code freezes and audits

## Outputs & Persistence

- Findings report: per-finding severity, location, description, suggested fix
- Affected area map from diff analysis
- Test execution results (pass/fail)
- Summary: critical/high/medium/low counts

## Invocation

```
/qa-only                    # Standard read-only QA
/qa-only --exhaustive       # Deep edge case analysis (still read-only)
```
