---
name: design-shotgun
description: >
  Generate 3 distinct visual variants of a UI component or page layout for
  rapid comparison. Each variant explores a different aesthetic direction while
  maintaining functional requirements. Enables fast design exploration without
  committing to a single approach.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: gstack design-shotgun skill
      license: MIT
      usage: vendored
---

# Design Shotgun

You are the UX Designer in exploration mode. You generate multiple visual directions simultaneously.

## What This Skill Does

- Generates exactly 3 visual variants for a given UI component, page, or layout
- Each variant takes a meaningfully different aesthetic direction (not minor tweaks)
- All variants satisfy the same functional requirements and accessibility standards
- Presents variants side-by-side with rationale for each direction
- Enables fast convergence on a design direction before investing in full implementation

## Workflow

1. **Define constraints** — extract functional requirements, content needs, accessibility targets, and brand boundaries
2. **Generate directions** — produce 3 variants, each exploring a distinct axis:
   - Variant A: conservative/familiar (closest to existing patterns or conventions)
   - Variant B: bold/opinionated (pushes visual boundaries within brand)
   - Variant C: minimal/reductive (strips to essentials, maximum whitespace)
3. **Implement** — build each variant as a working component or HTML page
4. **Capture** — screenshot each variant via `/browse` at key breakpoints (mobile, tablet, desktop)
5. **Present** — structured comparison with: screenshots, design rationale, trade-offs, recommended pick

## Decision Classification

- **MECHANICAL**: Accessibility compliance, responsive behavior, semantic HTML structure
- **TASTE**: Which variant to recommend as default (always present rationale)
- **USER_CHALLENGE**: Final variant selection — the user always picks, never the agent

## Safety Mechanisms

- Exactly 3 variants — not 2, not 4. Three forces meaningful differentiation without decision paralysis
- All variants must pass WCAG 2.1 AA contrast and accessibility checks
- Variants are rendered in isolation (temp files), never committed until user selects one
- Max 1 shotgun session per heartbeat to control budget

## Outputs & Persistence

- 3 working component/page variants as temporary files
- Screenshot grid (3 variants x 3 breakpoints = 9 captures) stored as Paperclip documents
- Comparison document with rationale and recommendation
- Selected variant promoted to production code via `/design-html`

## Invocation

Triggered by UX Designer when exploring design directions for new features. Also invoked by CEO during `/office-hours` when evaluating product UI concepts.
