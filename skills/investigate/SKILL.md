---
name: investigate
description: >
  4-phase root-cause debugging: reproduce, isolate, verify, prevent. Uses
  3-strike rule for failed hypotheses. Evidence-based with testable predictions.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /investigate

Systematic root-cause debugging with evidence-based hypothesis testing and a built-in escalation circuit breaker.

## What This Skill Does

- 4-phase structured debugging: reproduce, isolate, verify, prevent
- Every hypothesis must have a testable prediction before investigation begins
- 3-strike rule: after 3 consecutive failed hypotheses, escalates to manager
- Produces a regression test as the final artifact to prevent recurrence
- Evidence-based: no speculative fixes, every conclusion backed by observation

## Workflow

1. **Reproduce**: Confirm the bug exists. Establish reliable reproduction steps. If the bug cannot be reproduced, document the conditions tried and escalate.
2. **Isolate**: Form a hypothesis with a testable prediction. Use binary search strategy to narrow the root cause (bisect code paths, toggle features, reduce input). If the hypothesis fails, log it and form the next one. After 3 consecutive failures, escalate to manager.
3. **Verify**: Confirm the fix addresses the root cause by re-running reproduction steps. The fix must eliminate the bug without introducing new failures.
4. **Prevent**: Write a regression test that fails without the fix and passes with it. Ensure the test covers the specific failure mode, not just the happy path.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Reproduction strategy | TASTE | Agent chooses the most efficient reproduction path |
| Hypothesis ordering | TASTE | Most likely cause first, based on evidence |
| When to escalate (3-strike) | MECHANICAL | Hard rule: 3 consecutive failed hypotheses triggers escalation |
| Fix approach | TASTE | Agent selects fix, but verification is mandatory |
| Whether the bug is worth fixing | USER_CHALLENGE | Human decides priority and resource allocation |

## Safety Mechanisms

- 3-strike escalation prevents infinite debugging loops
- Every hypothesis logged with prediction and outcome (pass/fail)
- Fix must pass verification phase before being considered complete
- Regression test required — a fix without a test is incomplete

## Outputs & Persistence

- Investigation log: reproduction steps, hypotheses tested (with predictions and outcomes), root cause identified
- Fix commit with clear description of root cause and resolution
- Regression test covering the specific failure mode
- Escalation report (if 3-strike triggered)

## Invocation

```
/investigate <bug-description>
/investigate --issue <issue-url>
/investigate --reproduce-only       # Phase 1 only, no fix attempt
```
