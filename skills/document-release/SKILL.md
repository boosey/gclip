---
name: document-release
description: >
  Auto-syncs documentation after shipping. Updates README, API docs, changelog,
  and migration guides by detecting doc staleness against code changes.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /document-release

Automatically updates documentation to reflect shipped code changes.

## What This Skill Does

- Detects documentation staleness by comparing code changes against existing docs
- Updates README, API documentation, changelog, and migration guides
- Generates migration guides for breaking changes
- Creates PRs for documentation updates rather than committing directly
- Tracks doc-to-code mapping to identify which docs need updating

## Workflow

1. **Change Detection**: Read git diff between previous release and current HEAD. Identify changed public APIs, configuration options, CLI flags, and behavioral changes.
2. **Staleness Analysis**: Map code changes to documentation files. Flag docs that reference changed APIs or behaviors but haven't been updated.
3. **Update Generation**: Generate updated documentation for each stale file. For breaking changes, generate migration guide with before/after examples.
4. **PR Creation**: Create a documentation PR with all updates. Include a summary of what changed and why each doc was updated.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which docs are stale | MECHANICAL | Determined by code-to-doc mapping and diff analysis |
| Changelog entry wording | TASTE | Auto-generated from commit messages, follows project style |
| Whether a change is "breaking" | TASTE | Inferred from API surface changes; borderline cases flagged |
| Documentation structure changes | USER_CHALLENGE | Adding new doc sections or reorganizing requires human approval |

## Safety Mechanisms

- Creates PR rather than direct commit — human reviews before merge
- Does not delete existing documentation; only updates or adds
- Migration guides require breaking change evidence from the diff
- Changelog entries are traceable to specific commits

## Outputs & Persistence

- Documentation PR with updated files
- Staleness report: which docs were out of date and why
- Migration guide (if breaking changes detected)
- Updated changelog entries

## Invocation

```
/document-release                   # Auto-detect and update stale docs
/document-release --since <tag>     # Docs since specific release tag
/document-release --dry-run         # Show what would be updated without changes
```
