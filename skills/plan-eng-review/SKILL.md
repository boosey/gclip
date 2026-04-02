---
name: plan-eng-review
description: >
  11-step engineering review covering architecture, API design, data model,
  error handling, testing, performance, security, observability, deployment,
  documentation, and code quality. Produces ASCII diagrams and test plans.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /plan-eng-review

Runs an 11-step engineering review producing system diagrams, test plans, and specific recommendations.

## What This Skill Does

- 11-step review covering the full engineering surface of a plan
- Produces ASCII diagrams of system boundaries and data flows
- Generates a test plan artifact with failure modes registry
- Each step produces specific, actionable recommendations
- Identifies architectural risks and proposes mitigations

## Workflow

1. **Architecture**: Review system architecture. Produce ASCII diagram of component boundaries, dependencies, and communication patterns.
2. **API Design**: Evaluate API surface: endpoint design, versioning, error contracts, pagination, rate limiting.
3. **Data Model**: Review schema design, relationships, indexing strategy, migration plan.
4. **Error Handling**: Catalog error scenarios. Identify gaps where errors would be swallowed or produce poor user experience.
5. **Testing Strategy**: Generate test plan with: unit/integration/e2e coverage targets, failure modes registry, edge cases.
6. **Performance**: Analyze query patterns, N+1 risks, caching strategy, connection pooling, resource budgets.
7. **Security Surface**: Review auth boundaries, input validation, output encoding, secrets management.
8. **Observability**: Evaluate logging strategy, metrics collection, tracing, alerting thresholds.
9. **Deployment**: Review deployment strategy, rollback plan, feature flags, database migrations.
10. **Documentation**: Assess documentation needs: API docs, runbooks, architecture decision records.
11. **Code Quality**: Review patterns, abstractions, naming, file organization, dependency management.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Architecture recommendations | TASTE | Agent recommends with trade-off analysis |
| Test coverage targets | TASTE | Based on risk profile of the change |
| API design patterns | TASTE | Follow existing project conventions, recommend improvements |
| Breaking changes to public APIs | USER_CHALLENGE | Require human sign-off |

## Safety Mechanisms

- Review only — does not modify code
- Each step independently reviewable
- ASCII diagrams provide visual verification of understanding
- Test plan includes failure modes to prevent blind spots

## Outputs & Persistence

- 11-step review document with per-step findings and recommendations
- ASCII system boundary diagrams
- Test plan artifact with failure modes registry
- Engineering readiness assessment

## Invocation

```
/plan-eng-review <plan>
/plan-eng-review --step <n> <plan>      # Run specific step only
/plan-eng-review --test-plan-only <plan>  # Generate test plan only
```
