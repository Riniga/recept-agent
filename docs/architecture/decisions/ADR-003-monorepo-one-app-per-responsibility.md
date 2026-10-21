# ADR-003: One repository, one app per responsibility

**Status:** Accepted
**Date:** 2026-10-21

## Context

The project material (`docs/initial-idea/02-HLD.md`, `04-ARKITEKTURBESLUT.md`) recommends a
single, cohesive web application — a modular monolith — rather than several deployed
services. The owner has decided otherwise: separation of responsibilities is important,
and each responsibility should be its own app and its own deployed service. At the same
time, everything should live in one repository, not one repository per service.

The template already provides an `apps/` + `packages/` layout and a rule that code moves to
a shared package only when it is actually reused by at least two apps.

## Decision

- Every responsibility is a separate app under `apps/<app-name>/`, with its own source,
  tests and dependencies, and is deployed as its own service
  (see [ADR-004](ADR-004-deploy-every-app-to-one-azure-environment.md)).
- All apps live in this one repository and share its process, standards and CI.
- Shared code goes into `packages/<package-name>/` only when it is reused by at least two
  apps; that boundary decision is recorded as its own ADR.
- MVP-001 creates two apps: `apps/web` (user-facing, server-rendered HTML) and `apps/api`
  (the recipe API). Further apps are introduced by the MVP that needs them, not up front.

This is a deliberate deviation from the project material's monolith recommendation,
motivated by the owner's requirement for separated responsibilities.

## Consequences

**Benefits:**

- Clear ownership boundaries between user interface and recipe logic from day one.
- Each app can be deployed, scaled and replaced independently.
- One repository keeps process, CI and documentation in one place.

**Trade-offs:**

- More moving parts than a monolith: service-to-service calls, configuration of service
  URLs, separate deployments and health checks per app.
- Authentication between services and for the user must be designed across services
  (not yet decided — see `docs/architecture/overview.md`).
- The project material's "MCP in the same codebase as the recipe logic" must be
  reinterpreted: MCP tools must call the recipe API rather than share in-process code.
- Risk of premature splitting; mitigated by adding apps only when an MVP needs them.

## Alternatives considered

- **Modular monolith (the project material's recommendation)**: rejected by the owner —
  responsibilities should be separated into apps and services.
- **One repository per service**: rejected — splits process, CI and documentation for a
  small project with one maintainer.
