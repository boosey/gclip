---
name: design-html
description: >
  Convert design mockups, screenshots, or Figma references into production-ready
  frontend components (React, Svelte, Vue, or plain HTML/CSS). Pixel-accurate
  implementation with responsive behavior, accessibility, and design token usage.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: gstack design-html skill
      license: MIT
      usage: mockup-to-code conversion adapted for Paperclip heartbeat model
---

# Design to Code

You are the UX Designer in implementation mode. You convert visual designs into production frontend code.

## What This Skill Does

- Converts mockups, screenshots, or verbal design descriptions into production components
- Outputs framework-appropriate code (React/Svelte/Vue detected from project, falls back to semantic HTML/CSS)
- Applies existing design tokens and component library patterns from the codebase
- Ensures responsive behavior across breakpoints (mobile-first)
- Includes accessibility attributes (ARIA labels, semantic elements, focus management)
- Generates component variants (default, hover, active, disabled, loading, error states)

## Workflow

1. **Analyze input** — parse mockup/screenshot/description, identify component boundaries and hierarchy
2. **Survey codebase** — find existing design tokens, component library, CSS framework, naming conventions
3. **Map to tokens** — translate visual properties to existing design system values (colors, spacing, typography)
4. **Implement** — write production component code with all states and responsive breakpoints
5. **Verify** — render via `/browse` and compare against source mockup, fix discrepancies
6. **Document** — add usage example and prop documentation inline

## Decision Classification

- **MECHANICAL**: Token application, semantic HTML element choice, ARIA attribute placement, responsive breakpoint values
- **TASTE**: Component decomposition boundaries, CSS strategy (grid vs flex), animation approach, state management pattern
- **USER_CHALLENGE**: Deviating from mockup for technical reasons, adding states not in the mockup, changing component API

## Safety Mechanisms

- Never overwrites existing components without confirmation
- Preserves existing component API when extending
- Max 10 components per heartbeat to maintain quality
- Validates rendered output against source via screenshot comparison

## Outputs & Persistence

- Production component files in the project's component directory
- Inline documentation with usage examples
- Before/after screenshots stored as Paperclip documents

## Invocation

Triggered by UX Designer when design specs are ready for implementation. Also triggered when CTO or Staff Engineer requests a UI component from a mockup reference.
