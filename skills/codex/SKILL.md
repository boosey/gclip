---
name: codex
description: >
  Second opinion from OpenAI Codex. Runs same task through Claude and Codex
  in parallel, presents consensus table highlighting agreements and disagreements.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: true
---

# /codex

Obtains a second opinion from OpenAI Codex and presents a structured comparison against Claude's analysis.

## What This Skill Does

- Runs the same task through both Claude and Codex in parallel
- Builds a consensus table comparing outputs point-by-point
- Highlights agreements (high confidence) and disagreements (require human review)
- Used by the CTO agent during plan reviews for adversarial coverage
- Enforces filesystem boundary: Codex cannot read skill definitions

## Workflow

1. **Task Dispatch**: Send the same prompt/task to both Claude and Codex subagents in parallel.
2. **Collection**: Gather responses from both agents. Normalize output format for comparison.
3. **Consensus Analysis**: Compare responses point-by-point. Classify each point as AGREE, DISAGREE, or UNIQUE (only one agent raised it).
4. **Table Generation**: Build consensus table with columns: Topic, Claude Position, Codex Position, Status. Surface disagreements prominently.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which agent to trust on disagreement | USER_CHALLENGE | Human resolves all disagreements |
| Whether to include UNIQUE findings | TASTE | Included by default; unique findings often reveal blind spots |
| Task prompt normalization | MECHANICAL | Standardized prompt template for fair comparison |

## Safety Mechanisms

- Codex filesystem boundary: cannot access skill definitions, agent configs, or `.claude/` directory
- Both agents receive identical prompts to ensure fair comparison
- Disagreements are surfaced, never silently resolved
- API key required; fails gracefully if Codex API is unavailable

## Outputs & Persistence

- Consensus table with per-point comparison and status
- Individual agent responses preserved for reference
- Disagreement summary for human review

## Invocation

```
/codex <task-description>
/codex --compare <file-or-plan>
```
