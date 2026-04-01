---
name: setup-deploy
description: >
  Configures deployment pipelines. Detects project type and hosting platform.
  Generates CI config, environment variables, secrets, and health check endpoints.
metadata:
  sources:
    - kind: fork
      repo: garrytan/gstack
      attribution: Garry Tan
      license: MIT
      usage: Ported to Paperclip heartbeat execution model
  requires_api: false
---

# /setup-deploy

Configures deployment infrastructure for a project by detecting its stack and generating pipeline configuration.

## What This Skill Does

- Detects project type: Node.js, Python, Go, Rust, Ruby, etc.
- Identifies hosting platform: Vercel, Fly.io, AWS, GCP, Railway, etc.
- Generates or updates CI configuration (GitHub Actions, GitLab CI, etc.)
- Configures environment variables and secrets management
- Sets up health check endpoints for post-deploy verification
- Validates the full deploy configuration before first deployment

## Workflow

1. **Detection**: Analyze project files to determine language/framework (package.json, pyproject.toml, go.mod, etc.) and hosting platform (from existing config or user input).
2. **CI Configuration**: Generate or update CI pipeline config. Include: build, test, lint, deploy stages. Configure caching for dependencies.
3. **Environment Setup**: Identify required environment variables. Configure secrets management (GitHub Secrets, Vault, etc.). Generate `.env.example` for documentation.
4. **Health Checks**: Create or configure health check endpoints. Set up expected response validation for `/land-and-deploy` integration.
5. **Validation**: Dry-run the pipeline configuration. Verify all secrets are configured. Confirm health check endpoints respond correctly.

## Decision Classification

| Decision | Category | Rationale |
|---|---|---|
| Project type detection | MECHANICAL | Derived from project files |
| CI tool selection | TASTE | Matches existing project setup; defaults to GitHub Actions |
| Hosting platform selection | USER_CHALLENGE | Human chooses where to deploy |
| Secrets configuration | USER_CHALLENGE | Human must provide secret values |

## Safety Mechanisms

- Validation step before first deployment prevents broken pipelines
- Secrets are never written to plain-text files or committed
- Generates `.env.example` without actual values
- Health checks configured for `/land-and-deploy` and `/canary` integration

## Outputs & Persistence

- CI pipeline configuration file (e.g., `.github/workflows/deploy.yml`)
- Environment variable documentation (`.env.example`)
- Health check endpoint configuration
- Validation report: all checks pass/fail

## Invocation

```
/setup-deploy                           # Auto-detect and configure
/setup-deploy --platform <name>         # Specify hosting platform
/setup-deploy --validate                # Validate existing config only
```
