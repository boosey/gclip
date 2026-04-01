---
name: plan-ceo-review
description: >
  10-stage strategic review covering scope challenge, architecture, security,
  data flows, code quality, testing, performance, observability, and reversibility.
  Four scope modes with long-term trajectory scoring.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /plan-ceo-review

Runs a 10-stage strategic review of a plan from the CEO perspective, focusing on scope, risk, and long-term trajectory.

## What This Skill Does

- 10-stage review covering the full strategic surface of a plan
- 4 scope modes: EXPANSION, SELECTIVE_EXPANSION, HOLD, REDUCTION
- Stage 1 (Scope Challenge) is "nuclear" — can reject the entire plan premise
- Reversibility scoring on long-term commitments
- Produces an approved strategic plan with all review annotations

## Workflow

1. **Scope Challenge (nuclear)**: Question whether the plan should exist at all. Can reject the entire premise. Evaluate scope mode: is this EXPANSION, SELECTIVE_EXPANSION, HOLD, or REDUCTION?
2. **Architecture Validation**: Review system architecture decisions for alignment with company direction and technical debt trajectory.
3. **Error Handling Registry**: Catalog error handling patterns. Identify gaps where failures would be silent or unrecoverable.
4. **Security Model**: Review security boundaries, authentication flows, authorization model, and data protection.
5. **Data Flows & State**: Map data flows through the system. Identify state management patterns and consistency requirements.
6. **Code Quality Patterns**: Review for anti-patterns, unnecessary complexity, and maintainability concerns.
7. **Test Strategy**: Evaluate test coverage plan, test types, and gap analysis.
8. **Performance Analysis**: Review performance implications, scaling characteristics, and resource requirements.
9. **Observability & Monitoring**: Evaluate logging, metrics, alerting, and debugging capability.
10. **Long-term Trajectory & Reversibility**: Score each major decision for reversibility (easy to undo vs. locked in). Assess the plan's trajectory over 6-12 months.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Scope mode classification | TASTE | Agent classifies based on plan analysis |
| Stage 1 plan rejection | USER_CHALLENGE | Nuclear option requires human confirmation |
| Reversibility scores | TASTE | Agent scores with justification |
| Strategic direction approval | USER_CHALLENGE | CEO-level decisions require human sign-off |

## Safety Mechanisms

- Stage 1 can halt the entire pipeline — prevents investment in flawed premises
- Reversibility scoring prevents lock-in to irreversible decisions without awareness
- Each stage produces independently reviewable output
- Scope mode explicitly framed to prevent scope creep disguised as features

## Outputs & Persistence

- 10-stage review document with per-stage findings
- Scope mode classification with justification
- Reversibility score matrix for major decisions
- Approved strategic plan (if all stages pass)

## Invocation

```
/plan-ceo-review <plan>
/plan-ceo-review --stage <n> <plan>     # Run specific stage only
/plan-ceo-review --scope-only <plan>    # Stage 1 only (quick scope check)
```
