---
name: review
description: >
  Two-pass adversarial PR review. Pass 1 (structural): SQL safety, prompt
  boundaries, enum completeness, race conditions. Pass 2 (behavioral): business
  logic, edge cases, state management. Classifies findings as CRITICAL/AUTO-FIX/ASK.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /review

Two-pass adversarial review of a PR before landing.

## What This Skill Does

- Pass 1 (structural): SQL safety, LLM prompt injection boundaries, enum completeness, race conditions, error handling gaps
- Pass 2 (behavioral): business logic correctness, edge cases, state management, data flow integrity
- Classifies each finding: CRITICAL (must fix), INFORMATIONAL (note), AUTO-FIX (apply silently), ASK (needs developer input)
- AUTO-FIX items are applied without stopping the pipeline
- Persists review metadata for `/ship` verification gate

## Workflow

1. **Diff Load**: Read the PR diff. Identify files changed, net additions/deletions, and affected subsystems.
2. **Pass 1 — Structural**: Review for mechanical correctness:
   - SQL: parameterized queries, injection vectors, migration safety
   - LLM prompts: injection boundaries, output sanitization
   - Enums: exhaustive matching, missing cases
   - Concurrency: race conditions, deadlock potential, atomic operations
   - Error handling: swallowed errors, missing error types, recovery paths
3. **Pass 2 — Behavioral**: Review for logical correctness:
   - Business logic: requirement compliance, specification alignment
   - Edge cases: boundary values, empty inputs, overflow conditions
   - State management: consistency, stale state, update ordering
4. **Classification**: Label each finding as CRITICAL, INFORMATIONAL, AUTO-FIX, or ASK.
5. **Auto-Fix**: Apply AUTO-FIX items silently. Commit with clear description.
6. **Report**: Generate review report. Block on CRITICAL items. Surface ASK items for developer input.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Finding classification | TASTE | Agent classifies based on severity and fixability |
| AUTO-FIX application | MECHANICAL | Applied silently per classification |
| Whether to merge with CRITICAL items | USER_CHALLENGE | Human must resolve all CRITICAL items |
| ASK item resolution | USER_CHALLENGE | Developer provides input on ambiguous findings |

## Safety Mechanisms

- CRITICAL items are blocking — PR cannot land until resolved
- AUTO-FIX items are individually committed for easy revert
- Two-pass structure prevents structural issues from masking behavioral ones
- Review metadata persisted for downstream `/ship` verification

## Outputs & Persistence

- Review report: per-finding classification with evidence and location
- AUTO-FIX commits (if any)
- Review metadata: persisted for `/ship` gate verification
- Blocking items list: CRITICAL findings that must be resolved

## Invocation

```
/review <pr-number>
/review --pass1-only <pr-number>    # Structural review only
/review --no-autofix <pr-number>    # Skip AUTO-FIX application
```
