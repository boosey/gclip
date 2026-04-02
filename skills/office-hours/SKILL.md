---
name: office-hours
description: >
  Problem framing and ideation sessions. Two modes: Startup (diagnostic forcing
  questions for founders) and Builder (generative brainstorming). Anti-sycophantic.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /office-hours

Structured problem framing and ideation sessions that end with concrete assignments.

## What This Skill Does

- Two modes: Startup (diagnostic) and Builder (generative brainstorming)
- Startup mode: 6 forcing diagnostic questions that expose hidden assumptions
- Builder mode: generative brainstorming with mandatory alternative generation
- One question at a time — no question dumps
- Every session ends with a concrete assignment (not vague next steps)
- Anti-sycophantic: takes a position and states what evidence would change it

## Workflow

1. **Context**: Understand what the user is working on and what problem they're facing. Determine Startup vs Builder mode.
2. **Diagnostic** (Startup mode): Ask 6 forcing questions sequentially. Each question targets a common founder blind spot: market size, distribution, defensibility, unit economics, timeline, and team capability. One question at a time; wait for response before proceeding.
3. **Landscape Awareness**: Identify existing solutions, competitors, and adjacent approaches the user may not have considered.
4. **Premise Challenge**: Identify the core premise and challenge it directly. State what evidence would validate or invalidate it.
5. **Alternatives**: Generate 2-3 concrete alternative approaches. These are mandatory — every session must surface alternatives even if the original approach is strong.
6. **Design Doc**: If the session produces a viable direction, outline a design document structure.
7. **Handoff**: End with a specific, concrete assignment with clear deliverables and scope.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Mode selection (Startup vs Builder) | TASTE | Inferred from context; user can override |
| Question ordering | TASTE | Adapted to the specific problem |
| Alternative generation | MECHANICAL | Mandatory 2-3 alternatives, no exceptions |
| Which direction to pursue | USER_CHALLENGE | Human decides; agent frames the options |

## Safety Mechanisms

- Anti-sycophancy enforced: agent must take a position and state the counter-evidence
- One question at a time prevents overwhelming the user
- Mandatory alternatives prevent premature commitment to first idea
- Session always ends with concrete assignment, not open-ended discussion

## Outputs & Persistence

- Session transcript with questions and responses
- Premise challenge with evidence requirements
- Alternative approaches (2-3 minimum)
- Concrete assignment with deliverables

## Invocation

```
/office-hours                   # Auto-detect mode
/office-hours --startup         # Founder diagnostic mode
/office-hours --builder         # Generative brainstorming mode
```
