---
name: gclip
description: Hybrid agent company merging Paperclip orchestration with gstack cognitive methodology
version: 0.1.0
agents: 9
skills: 37
license: MIT
sources:
  - kind: fork
    repo: garrytan/gstack
    license: MIT
    usage: cognitive methodology, skill definitions, workflow phases
  - kind: compatible
    repo: paperclipai/paperclip
    license: MIT
    usage: orchestration infrastructure, adapter system, board governance
---

# gclip

A hybrid agent company definition that runs on [Paperclip](https://github.com/paperclipai/paperclip) infrastructure with [gstack](https://github.com/garrytan/gstack) cognitive methodology.

## What this is

gclip is a superset: it takes Paperclip's multi-agent orchestration (heartbeats, budgets, board governance, adapter abstraction) and injects gstack's opinionated workflow rigor (cognitive gearing, decision triage, phase enforcement, anti-sycophancy, operational limits).

It adds two novel agents not present in either parent project:
- **Evaluator** — observes all agent performance via Paperclip telemetry and proposes improvements to the board
- **Learning Curator** — maintains institutional memory, applies board-approved changes, prunes stale knowledge

## Import

```bash
paperclip company import --from ./gclip
```

## Org Chart

```
              BOARD (Human)
                  |
                  v
                CEO --------------- Evaluator
              (vision)           (self-improvement)
                |                       |
                v                       v
               CTO              Learning Curator
            (tech lead)         (memory manager)
         /    |    \    \    \
       Staff Release QA  CSO  UX
       Eng   Eng    Eng  (sec) Designer
```
