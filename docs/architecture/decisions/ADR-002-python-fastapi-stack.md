# ADR-002: Use Python with FastAPI, Jinja2 and Pydantic, managed with Conda and pip-tools

**Status:** Accepted
**Date:** 2026-10-21

## Context

The project material (`docs/initial-idea/03-DLD.md`, `04-ARKITEKTURBESLUT.md`) recommends a
Python stack: FastAPI for web and backend, server-rendered HTML with Jinja2 (no separate
frontend build chain), Pydantic for validating recipe data and AI output, and SQLite (with
FTS5 when full-text search is needed) as a rebuildable local index. The template this
repository was created from is already set up for Python with Conda, pip-tools, Ruff and
pytest.

The owner confirmed the stack and asked for the latest stable Python version. Which exact
version that is — and whether conda-forge and the chosen libraries support it — has not
yet been verified.

## Decision

All apps are written in Python, using:

- **FastAPI** for HTTP services.
- **Jinja2** server-rendered templates for user-facing pages; plain JavaScript only where it
  adds clear value. No separate frontend framework or build chain.
- **Pydantic** for validated data models.
- **SQLite** (FTS5 when needed) for local status and search index, rebuildable from
  OneDrive — introduced when an MVP needs it.
- **Conda** for the environment, with **pip-tools** producing a hash-pinned
  `requirements-lock.txt` from `requirements.in`.
- **Ruff** (format + lint) and **pytest** (with coverage) as the quality tooling.

Python version: the latest stable CPython release supported by conda-forge and the chosen
libraries. The exact version is determined in MVP-001's investigation (§0 of its plan) and
then pinned in `environment.yml`, `pyproject.toml` and CI.

## Consequences

**Benefits:**

- One language across all apps; the template's tooling and CI apply directly.
- Server-rendered HTML keeps the UI simple and avoids a second toolchain.
- Pydantic gives one validated shape for AI output before it is shown or stored.

**Trade-offs:**

- Rich client-side interactivity is harder without a frontend framework.
- Conda plus pip-tools is heavier than a plain venv and needs care with platform-specific
  transitive packages (Windows locally, Linux in CI and Azure).
- SQLite is a local file; how it fits services hosted in Azure is not yet decided (see
  open questions in `docs/architecture/overview.md`).

## Alternatives considered

- **Separate React/Vite frontend with a JSON API**: rejected by the project material — adds
  a build chain and duplicated logic without a verified need.
- **venv or uv instead of Conda**: rejected by the owner; Conda with pip is the chosen
  environment tool.
- **A separate database or search service**: rejected until a verified need exists.
