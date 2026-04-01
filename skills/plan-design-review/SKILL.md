---
name: plan-design-review
description: >
  Pre-implementation design audit scoring 7 dimensions (0-10). Identifies AI
  design slop. CSS-first recommendations. Runs on plans, not live code.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /plan-design-review

Audits a design plan across 7 dimensions before implementation begins.

## What This Skill Does

- Rates a design plan across 7 dimensions on a 0-10 scale
- Runs pre-implementation — evaluates plans, mockups, and specifications (not live code)
- Identifies AI design slop patterns (generic gradients, stock layouts, placeholder aesthetics)
- Provides CSS-first recommendations that avoid unnecessary JavaScript
- Produces a design audit with specific, actionable fixes

## Workflow

1. **Plan Intake**: Read the design plan, mockups, or wireframes. Identify the UI surfaces being designed.
2. **7-Dimension Rating**: Score each dimension 0-10 with specific justification:
   - Visual hierarchy: information flow, focal points, scanning patterns
   - Typography: font selection, scale, readability, rhythm
   - Color system: palette coherence, contrast, semantic usage
   - Spacing & layout: grid consistency, whitespace, alignment
   - Interaction design: affordances, feedback, state transitions
   - Accessibility: WCAG compliance, keyboard navigation, screen reader support
   - Responsive behavior: breakpoint strategy, content reflow, touch targets
3. **Slop Detection**: Scan for AI-generated design patterns across 10 categories. Flag each instance.
4. **Recommendations**: For each dimension scoring below 7, provide specific CSS-first fixes. Avoid JavaScript unless CSS cannot achieve the result.
5. **Report**: Generate audit with scores, slop findings, and prioritized recommendations.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Dimension scores | TASTE | Agent scores with evidence; human can override |
| Slop classification | TASTE | Based on 10-category detection patterns |
| CSS vs JS recommendation | TASTE | CSS-first preference, auto-decided |
| Design direction pivots | USER_CHALLENGE | Fundamental aesthetic changes require human input |

## Safety Mechanisms

- Pre-implementation only — does not modify any code or assets
- Slop detection prevents generic AI aesthetics from reaching production
- Scores require specific justification (no unsupported ratings)
- CSS-first preference reduces implementation complexity

## Outputs & Persistence

- 7-dimension score card with per-dimension justification
- Slop detection report with flagged patterns
- Prioritized recommendations (CSS-first)
- Overall design readiness assessment

## Invocation

```
/plan-design-review <plan>
/plan-design-review --dimension <name> <plan>   # Score single dimension
```
