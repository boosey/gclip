---
name: learn
description: >
  Persistent institutional memory system. Discovers, stores, retrieves,
  and manages operational learnings with dual-write to Paperclip documents
  API and local .context/learnings.json cache.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: gstack learn skill
      license: MIT
      usage: vendored
  requires_api:
    - POST /api/documents
    - GET /api/documents
---

# Learn

Persistent institutional memory system forked from gstack. Discovers, stores, retrieves, and manages operational learnings across the agent organization. Modified from gstack to dual-write to both Paperclip's document API (source of truth) and a local JSON cache (fast reads).

## What This Skill Does

Manages the lifecycle of operational learnings — facts, patterns, and insights that agents discover during work. Each learning has a confidence level that can be promoted through repeated confirmation or pruned when evidence becomes stale.

### Learning Schema

```json
{
  "id": "learn-<ulid>",
  "agent_id": "staff-engineer",
  "skill": "review",
  "type": "pattern|anti-pattern|process|tool|domain",
  "key": "short identifier",
  "insight": "the actual learning content",
  "confidence": "VERIFIED|UNVERIFIED|TENTATIVE",
  "confidence_score": 0.85,
  "source": "commit|issue|heartbeat|retro|manual",
  "confirmations": 3,
  "first_seen": "2026-03-15T10:00:00Z",
  "last_confirmed": "2026-03-28T14:30:00Z",
  "paperclip_document_id": "doc-<id>",
  "tags": ["testing", "performance"]
}
```

## Workflow

1. **Capture** — Agent encounters an operational insight during work. Learning is created with initial confidence (UNVERIFIED for single observations, TENTATIVE for inferred patterns).
2. **Dual-Write** — POST /api/documents creates the Paperclip document (source of truth). The returned `document_id` is stored in the learning record. Append to `.context/learnings.json` for local fast reads.
3. **Retrieve** — On read, serve from `.context/learnings.json`. If local cache is missing or corrupt, rebuild from GET /api/documents.
4. **Confirm** — When a learning is re-observed, increment `confirmations` and update `last_confirmed`. At 3 confirmations, promote UNVERIFIED → TENTATIVE. At 5 confirmations, promote TENTATIVE → VERIFIED.
5. **Prune** — Detect stale learnings: files referenced in a learning that no longer exist, learnings older than 90 days with confidence_score below 0.5, or conflicting insights (prompt agent for resolution).

## Decision Classification

| Scenario | Classification | Rationale |
|---|---|---|
| Recording a new learning | MECHANICAL | Schema-driven, deterministic |
| Promoting confidence on confirmation | MECHANICAL | Threshold-based, no judgment |
| Resolving conflicting learnings | TASTE | Agent picks which insight holds, surfaces at gate |
| Pruning VERIFIED learning | USER_CHALLENGE | Requires human confirmation before deleting high-confidence data |

## Safety Mechanisms

- **Max 5 learnings per heartbeat** — prevents flooding the knowledge base during a single work cycle.
- **Confidence floor configurable per agent** — agents can set a minimum confidence for learnings they consume.
- **Never overwrites VERIFIED with lower confidence** — a VERIFIED learning can only be updated with new VERIFIED evidence. Lower-confidence contradictions are stored separately for human review.
- **Dual-write consistency** — Paperclip document API is the source of truth. Local cache is rebuilt from API on mismatch.
- **Prune is non-destructive by default** — stale learnings are flagged, not deleted, until confirmed.

## Outputs & Persistence

- Paperclip document → POST /api/documents (type: `learning`, source of truth)
- Local cache → `.context/learnings.json` (append-only during heartbeat, rebuilt from API on inconsistency)
- Prune report → stdout summary of stale/conflicting learnings

## Invocation

```
/learn                       # Show recent learnings for current agent
/learn add                   # Manually record a new learning
/learn search <query>        # Full-text search across all learnings
/learn prune                 # Detect and flag stale or conflicting learnings
/learn export                # Format learnings for inclusion in CLAUDE.md
/learn stats                 # Counts by confidence level, type, and agent
```
