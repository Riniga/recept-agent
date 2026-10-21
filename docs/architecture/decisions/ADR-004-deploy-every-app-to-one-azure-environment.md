# ADR-004: Deploy every app to one Azure environment on merge to `main`

**Status:** Accepted (the specific Azure hosting service is **Proposed**, decided in
MVP-001 §0)
**Date:** 2026-10-21

## Context

The owner requires that every MVP is deployed and fully testable both locally and online.
Initially there is one online environment, in Azure. The project material
(`docs/initial-idea/08-PLATTFORM-OCH-LEVERANS.md`) sets the frame: GitHub Actions for CI,
Azure as the cloud platform, the simplest Azure service that fits, managed app hosting
preferred over container platforms, scripts (PowerShell/Bash) before IaC tools, and
federated (OIDC) authentication from GitHub Actions to Azure instead of long-lived
secrets.

The owner makes all commits and pull requests and merges them. The AI action rules require
explicit human approval for production-writing actions.

## Decision

- There is **one** Azure environment. `main` is what runs there.
- CI runs on every pull request. After a pull request is merged to `main`, a GitHub Actions
  workflow deploys every app automatically, each as its own Azure service
  ([ADR-003](ADR-003-monorepo-one-app-per-responsibility.md)).
- **The human merge of the pull request is the approval gate** for deployment.
- GitHub Actions authenticates to Azure with OIDC federated credentials; no long-lived
  Azure secrets are stored.
- Each deployment ends with an automated smoke test against the deployed apps.
- Every app must also run locally with a documented command.
- Creating Azure resources is done by versioned scripts that the owner runs; it is not a
  side effect of CI.

Which Azure hosting service each app uses (e.g. App Service on Linux vs. Container Apps),
the region and the resource naming are **Proposed** — determined in MVP-001 §0 against the
"simplest managed service" guideline, then recorded in a follow-up ADR if the choice is
significant.

## Consequences

**Benefits:**

- Every MVP is demonstrable online as soon as it is merged.
- No production secrets for Azure in GitHub; access is scoped to this repo and branch.
- Deployment is traceable to a reviewed, merged pull request.

**Trade-offs:**

- With one environment, a merged defect is immediately live; mitigated by CI gates and
  the post-deploy smoke test, and acceptable because the environment holds demo data only.
- No staging environment: DAST against staging (methodology C1 SKA 3) cannot be met as
  written and must be tracked as a gap.
- Before real recipe data flows (R2/R3), the online environment needs access control.

## Alternatives considered

- **Manual approval in a GitHub Environment in addition to the merge**: not chosen — the
  owner's merge is already the explicit, in-the-moment approval.
- **Local-only demo until later**: rejected by the owner — every MVP is deployed.
- **Separate dev/test/prod environments**: rejected for now — no need is verified yet.
