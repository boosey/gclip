---
name: cso-audit
description: >
  14-phase security audit covering OWASP Top 10 and STRIDE threat modeling.
  Two modes: Daily (8/10 confidence gate) and Comprehensive (2/10 confidence).
  Every finding requires exploit scenario, evidence, and remediation roadmap.
metadata:
  sources:
    - kind: github-file
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: vendored
  requires_api: false
---

# /cso-audit

Runs a systematic 14-phase security audit with evidence-based findings and false positive filtering.

## What This Skill Does

- Executes 14 security audit phases covering the full application attack surface
- Two modes: Daily (high-confidence only) and Comprehensive (includes tentative findings)
- Every finding requires: exploit scenario, code trace evidence, confidence level, remediation roadmap
- Auto-filters ~21 categories of known false positives
- Covers: OWASP Top 10 vulnerability classes and STRIDE threat modeling

## Workflow

1. **Authentication & Authorization**: Review auth flows, session management, token handling.
2. **Injection**: SQL, NoSQL, command, LDAP, XPath injection vectors.
3. **Cross-Site Scripting (XSS)**: Stored, reflected, DOM-based XSS.
4. **Cross-Site Request Forgery (CSRF)**: Token validation, SameSite, origin checks.
5. **Cryptography**: Key management, algorithm selection, entropy, TLS configuration.
6. **Access Control**: RBAC/ABAC enforcement, privilege escalation paths, IDOR.
7. **Security Configuration**: Default credentials, debug modes, header policies, CORS.
8. **Dependencies**: Known CVEs in dependencies, outdated packages, supply chain risk.
9. **Data Exposure**: PII leaks, verbose errors, log sanitization, API response filtering.
10. **Logging & Monitoring**: Audit trail completeness, tamper resistance, alert coverage.
11. **API Security**: Rate limiting, input validation, schema enforcement, versioning.
12. **Business Logic**: Workflow bypass, race conditions, state manipulation.
13. **Infrastructure**: Container config, network segmentation, secrets management.
14. **Supply Chain**: Build pipeline integrity, dependency provenance, artifact signing.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Finding confidence threshold | TASTE | Daily: 8/10 (zero noise), Comprehensive: 2/10 (includes TENTATIVE) |
| False positive classification | MECHANICAL | 21 known false positive categories auto-filtered |
| Remediation priority ordering | TASTE | Ordered by exploitability and blast radius |
| Whether to remediate a finding | USER_CHALLENGE | Human decides on all remediation actions |

## Safety Mechanisms

- Read-only audit — does not modify application code
- False positive filter prevents alert fatigue (~21 categories)
- Every finding requires reproducible evidence (code trace, not speculation)
- Confidence gating prevents low-quality findings in Daily mode

## Outputs & Persistence

- Audit report: per-phase findings with severity, confidence, evidence, and remediation roadmap
- Executive summary: critical/high/medium/low counts with trend comparison
- Finding database: stored for trend analysis across audits

## Invocation

```
/cso-audit                  # Daily mode (8/10 confidence gate)
/cso-audit --comprehensive  # Full audit (2/10 confidence, includes TENTATIVE)
/cso-audit --phase <n>      # Run specific phase only
```
