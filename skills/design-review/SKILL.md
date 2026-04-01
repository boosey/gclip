---
name: design-review
description: >
  Live site visual audit with two modes: plan review (7-dimension rating) and
  live review (80-item checklist with CSS-first fixes). Includes AI slop detection.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /design-review

Conducts a visual audit of a live site or implementation, scoring across design dimensions and applying CSS-first fixes.

## What This Skill Does

- Two modes: plan review (pre-implementation) and live review (post-implementation)
- Plan review: rates across 7 design dimensions (0-10 scale each)
- Live review: runs an 80-item checklist covering layout, typography, color, spacing, interaction, accessibility, responsive behavior, and performance
- 10-category AI slop detection (generic gradients, placeholder copy, stock patterns, etc.)
- CSS-first fix preference: avoids JavaScript when CSS achieves the same result
- Max 30 fixes per session to prevent scope creep

## Workflow

1. **Screenshot Capture**: Take full-page and viewport screenshots of target pages using `/browse`.
2. **Dimension Scoring** (plan mode): Rate each of the 7 dimensions (visual hierarchy, typography, color system, spacing/layout, interaction design, accessibility, responsive behavior) on a 0-10 scale with specific justification.
3. **Checklist Audit** (live mode): Run the 80-item checklist. For each failing item, classify as CSS-fixable or requires-JS. Apply CSS-first fixes up to the 30-fix cap.
4. **Slop Detection**: Scan for the 10 categories of AI-generated design patterns. Flag each instance with specific remediation.
5. **Report**: Generate audit report with scores, fixes applied, and remaining recommendations.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Dimension scores | TASTE | Agent scores with justification; human can override |
| CSS vs JS fix approach | TASTE | CSS-first preference, auto-decided |
| Whether to apply a fix | TASTE | Applied automatically up to 30-fix cap for CSS-only fixes |
| Design direction changes | USER_CHALLENGE | Aesthetic choices beyond fix scope require human input |

## Safety Mechanisms

- Max 30 fixes per session prevents runaway modifications
- CSS-first preference minimizes behavioral side effects
- Screenshot verification after each fix batch
- Slop detection prevents propagation of generic AI aesthetics

## Outputs & Persistence

- Dimension scores (7 categories, 0-10 each) with justifications
- Checklist results: pass/fail per item with fix details
- Slop detection report with flagged instances
- Before/after screenshots for applied fixes

## Invocation

```
/design-review <url>            # Live review with fixes
/design-review --plan <plan>    # Plan mode (scoring only, no fixes)
/design-review --no-fix <url>   # Live review without applying fixes
```
