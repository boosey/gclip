---
name: design-consultation
description: >
  Builds complete design systems: color palette, typography scale, spacing,
  motion guidelines, and component inventory. Outputs production-ready CSS tokens.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /design-consultation

Generates a complete design system with production-ready tokens and component specifications.

## What This Skill Does

- Generates a color palette with semantic naming (primary, secondary, neutral, status colors)
- Defines a typography scale with font stacks, sizes, weights, and line heights
- Creates a spacing system (4px/8px base grid) with named tokens
- Produces motion and animation guidelines (duration, easing, purpose-based presets)
- Builds a component inventory based on the project's UI needs
- Outputs production-ready CSS custom properties and/or design tokens

## Workflow

1. **Discovery**: Analyze the project's existing UI (if any), brand requirements, and target platforms. Identify constraints (accessibility requirements, performance budgets).
2. **Foundation**: Generate color palette, typography scale, and spacing system. Ensure WCAG 2.1 AA contrast ratios for all text/background combinations.
3. **Motion**: Define animation guidelines: entrance/exit transitions, micro-interactions, loading states. Respect `prefers-reduced-motion`.
4. **Components**: Build component inventory from project needs. Define component anatomy, states, and variants.
5. **Output**: Export as CSS custom properties, JSON design tokens, or both. Include usage documentation for each token.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Base grid unit (4px vs 8px) | TASTE | 8px default, auto-decided via pragmatic principle |
| Color palette generation | TASTE | Derived from brand input or sensible defaults |
| Accessibility minimum (AA vs AAA) | TASTE | AA minimum, AAA recommended where feasible |
| Brand direction and identity | USER_CHALLENGE | Human defines brand; agent implements system |

## Safety Mechanisms

- All color combinations validated against WCAG 2.1 AA contrast requirements
- Motion guidelines include `prefers-reduced-motion` fallbacks
- Component inventory reviewed before token generation to prevent unused tokens
- No external dependencies; outputs are standalone CSS/JSON

## Outputs & Persistence

- Design tokens: CSS custom properties file and/or JSON token file
- Typography scale documentation
- Color palette with contrast ratio matrix
- Component inventory with state definitions
- Motion guidelines with code examples

## Invocation

```
/design-consultation                    # Full design system generation
/design-consultation --colors-only      # Color palette only
/design-consultation --from <brand-doc> # Generate from brand guidelines
```
