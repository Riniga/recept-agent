# MVP-001 – Walking skeleton

## Purpose

Put the whole delivery chain in place — repository, stack, two minimal apps, CI with the
methodology's quality gates, and deployment to Azure — before any recipe functionality is
built. The gates must exist before AI tools start writing production code, so that
everything that follows is checked from its first line.

## Goals

- Code goes from a pull request through blocking quality gates to a running service in
  Azure, with the owner's merge as the only manual step.
- Both apps run locally with a documented command and online in Azure, and their
  connection (web → api) is proven in both places.
- The project's standing against Skanskas utvecklingsmetodik is assessed, and the gaps are
  visible in one place.

## Context

The repository has been initialised from the template: vision, roadmap and
ADR-001–ADR-004 exist; there is no application code. The template's Python tooling
(`pyproject.toml`, `pytest.ini`, `environment.yml`, `requirements.in`, pre-commit,
`.github/workflows/ci.yml`) still contains placeholders. See
[`docs/architecture/overview.md`](../architecture/overview.md) and
[ADR-002](../architecture/decisions/ADR-002-python-fastapi-stack.md),
[ADR-003](../architecture/decisions/ADR-003-monorepo-one-app-per-responsibility.md),
[ADR-004](../architecture/decisions/ADR-004-deploy-every-app-to-one-azure-environment.md).

## Scope

- Conda environment with the latest stable Python supported by the stack, and a
  hash-pinned dependency lock.
- `apps/api`: a minimal FastAPI service with a health endpoint.
- `apps/web`: a minimal FastAPI + Jinja2 app that renders a start page and shows whether
  `api` is reachable — with a visible error, not a success-looking default, when it is not.
- Tests for both apps, written before the implementation (AI-TDD), and a coverage floor
  set from a measured baseline.
- Local run of both apps with one documented command.
- CI on pull request: Ruff (format + lint), tests with coverage, secret scan
  (detect-secrets), dependency checks (lock drift, licence scan, Dependabot), SAST and
  instruction-file scan — all blocking.
- Deployment on merge to `main`: each app as its own Azure service, GitHub → Azure via
  OIDC, followed by a smoke test of both apps and of web → api.
- Versioned scripts for creating the Azure resources, run by the owner.
- Branch protection on `main` with the CI checks required.
- First methodology assessment (areas A–F), gap register, interpretations and exceptions
  in `docs/methodology-compliance/`.

## Out of Scope

- Any recipe functionality: upload, AI interpretation, OneDrive, search, assistant, MCP
  (R2–R4).
- User authentication — the online apps hold no data in this MVP; access control is
  required before real recipe data flows (R2/R3).
- A choice of AI model or provider (MVP-002).
- More than one Azure environment, custom domains, IaC tooling (Bicep/Terraform),
  containers unless the hosting investigation requires them.
- DAST as a gate — the existing manual `dast.yml` may be run against the environment, but
  the staging requirement stays a tracked gap.

## Acceptance Criteria

- `conda env create -f environment.yml` and the documented local-run command start both
  apps; the web start page shows that `api` is reachable, and shows a clear error when
  `api` is stopped.
- `pytest` passes locally and in CI; the coverage floor in `pyproject.toml` is set just
  below a measured baseline, recorded in `interpretations.md`.
- A pull request triggers all CI checks listed in Scope; each one is blocking and required
  by branch protection on `main`.
- A merge to `main` deploys both apps as separate Azure services without long-lived Azure
  secrets in GitHub, and the post-deploy smoke test passes against the online URLs.
- The online web start page shows that the online `api` is reachable.
- `docs/methodology-compliance/` contains an assessment per area A–F, a populated gap
  register, interpretations and the single-maintainer exception.
- No secrets are committed; `.env.example` lists every key the apps read.
- `README.md`, `docs/architecture/overview.md` and `current-state.md` describe what
  actually exists.

## Outcome at close (YYYY-MM-DD)

<!-- Fill in when the MVP is closed — see docs/mvp/TEMPLATE.md. -->
