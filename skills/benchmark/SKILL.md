---
name: benchmark
description: >
  Measures performance baselines including Core Web Vitals, page load times,
  resource sizes, and bundle analysis. Detects regressions against stored baselines.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /benchmark

Captures performance baselines and detects regressions by comparing against previous measurements.

## What This Skill Does

- Measures Core Web Vitals: Largest Contentful Paint (LCP), First Input Delay (FID), Cumulative Layout Shift (CLS)
- Records page load time, time to interactive, and total resource sizes
- Runs bundle analysis to identify oversized chunks and tree-shaking failures
- Stores baselines for future comparison
- Flags regressions when metrics exceed previous baseline thresholds

## Workflow

1. **Setup**: Launch headless browser session. Identify target URLs from project config or arguments.
2. **Measurement**: For each target URL, collect Core Web Vitals, load timing, resource waterfall, and total transfer size. Run bundle analysis on built assets.
3. **Comparison**: Load previous baseline (if exists). Compare each metric against stored values. Flag regressions exceeding configurable thresholds (default: 10% degradation).
4. **Report**: Generate benchmark report with per-metric results, trend arrows, and regression alerts. Store new baseline for future runs.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Which metrics to collect | MECHANICAL | Fixed set: Core Web Vitals + load timing + bundle size |
| Regression threshold | TASTE | Default 10%, adjustable per project |
| Whether to block deploy on regression | USER_CHALLENGE | Human decides severity of performance impact |

## Safety Mechanisms

- Read-only operation — does not modify application code or assets
- Headless browser auto-terminates after measurement completes
- Baselines are append-only; previous baselines are never overwritten

## Outputs & Persistence

- Benchmark report with per-metric values and trend indicators
- Stored baseline file (JSON) for future comparison
- Regression alerts with specific metric and percentage change

## Invocation

```
/benchmark [url...]
/benchmark --compare <baseline-id>
```
