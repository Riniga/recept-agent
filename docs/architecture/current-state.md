# Current Architecture State

Short reference for the current state of the workspace. For full context see
[`overview.md`](overview.md). Keep this current — it's the quick-scan version; update it
in the same PR as any change to an app's status, test count, or key capabilities.

## Applications

| App | Status | Tests | Key capabilities |
|-----|--------|-------|-------------------|
| `api` | Planned (MVP-001) | 0 | health endpoint |
| `web` | Planned (MVP-001) | 0 | start page, api status, health endpoint |

## Shared packages

None. A package is created only when code is reused by at least two apps
([ADR-003](decisions/ADR-003-monorepo-one-app-per-responsibility.md)).

## Conventions

See [`docs/standards/`](../standards/). Project-specific: one app per responsibility, each
deployed as its own Azure service on merge to `main`
([ADR-003](decisions/ADR-003-monorepo-one-app-per-responsibility.md),
[ADR-004](decisions/ADR-004-deploy-every-app-to-one-azure-environment.md)).

## Dependencies

None installed yet. Planned: FastAPI, Jinja2, Pydantic, Uvicorn
([ADR-002](decisions/ADR-002-python-fastapi-stack.md)).

## Test counts

0 — no code yet.

## Methodology compliance

See [`docs/methodology-compliance/`](../methodology-compliance/) — first assessment is part
of MVP-001.
