---
name: ship
description: >
  Pre-merge orchestration gate. Sequence: merge base, test, review, version bump,
  changelog, bisectable commits, PR creation. Non-interactive for MECHANICAL items.
  Stops only for conflicts, failures, ASK items, and coverage gates.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /ship

Orchestrates the full pre-merge pipeline from tests through PR creation.

## What This Skill Does

- Runs the complete pre-merge sequence: merge base branch, run tests, run `/review`, version bump, changelog update, bisectable commit creation, push, PR creation
- Non-interactive for MECHANICAL items — proceeds automatically
- Stops only for: merge conflicts, test failures, ASK items from `/review`, version bump decisions, coverage gates, incomplete plan items
- Ensures every commit in the PR is bisectable (each commit builds and passes tests)
- Creates a PR with all review metadata attached

## Workflow

1. **Merge Base**: Merge the base branch into the feature branch. Stop on merge conflicts.
2. **Test**: Run the full test suite. Stop on test failures.
3. **Review**: Run `/review` on the diff. Apply AUTO-FIX items. Stop on CRITICAL or ASK items.
4. **Version Bump**: Determine version bump type (patch/minor/major) from change analysis. Stop for human confirmation on minor/major bumps.
5. **Changelog**: Update changelog from commit history. Auto-format entries.
6. **Bisectable Commits**: Reorder/squash commits to ensure each one independently builds and passes tests.
7. **Push**: Push the branch to remote.
8. **PR Creation**: Create a PR with title, description, review metadata, and changelog excerpt.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| AUTO-FIX application | MECHANICAL | Applied silently from `/review` |
| Changelog formatting | MECHANICAL | Follows project conventions |
| Version bump type | TASTE | Auto-determined for patch; human confirms minor/major |
| Merge conflict resolution | USER_CHALLENGE | Human resolves conflicts |
| CRITICAL review findings | USER_CHALLENGE | Must be resolved before proceeding |

## Safety Mechanisms

- Hard stops on: merge conflicts, test failures, CRITICAL review items, coverage regressions
- Version bump requires human confirmation for minor and major bumps
- Bisectable commits ensure every commit in the PR is independently valid
- Review metadata attached to PR for downstream verification

## Outputs & Persistence

- PR with all review metadata, changelog, and version bump
- Bisectable commit history
- Review metadata persisted on the PR for `/land-and-deploy`
- Ship log: timestamps and outcomes for each pipeline stage

## Invocation

```
/ship                       # Full pipeline
/ship --no-version-bump     # Skip version bump step
/ship --draft               # Create draft PR
```
