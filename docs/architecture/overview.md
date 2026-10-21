# Architecture Overview

<!--
Keep this file current: update it in the same PR as any change to workspace structure,
major components, dependencies, or the build/deploy process (see
docs/standards/documentation.md).
-->

## Project Overview

ReceptAgent turns photographed or scanned paper recipes into a reviewed, searchable
recipe library in the user's private OneDrive, with an AI assistant that answers questions
about the collection. See [`docs/vision.md`](../vision.md) and the original project
material in [`docs/initial-idea/`](../initial-idea/).

**Current state: there is no application code yet.** The repository contains the
development process, standards, methodology, ADRs and the plan for the first MVP.

## Current Workspace Structure

```text
.github/             CI workflow (template, placeholders not yet adapted), manual DAST
                     workflow, Dependabot, PR template, Copilot pointer
docs/
    initial-idea/    Original project material (historical input, not edited)
    architecture/    This overview, current state, ADRs
    mvp/, plans/     MVPs and implementation plans
    development/     Setup, environment, tools, process
    standards/       Coding, testing, git, documentation, dependencies
    methodology/     Organisation methodology (not edited by this project)
    methodology-compliance/  This project's standing against the methodology
scripts/             scan_instruction_files.py (instruction-file Unicode scan)
environment.yml, requirements.in, pyproject.toml, pytest.ini, .pre-commit-config.yaml
                     Python tooling from the template — not yet adapted (MVP-001)
```

There is no `apps/` or `packages/` directory yet.

## Major Components

None implemented. Planned components are listed under
[Planned Evolution](#planned-evolution-of-the-workspace).

## Existing Dependencies

### Runtime

None yet.

### Development Environment

Conda (`environment.yml`) with a pip-tools lock compiled from `requirements.in`
([ADR-002](decisions/ADR-002-python-fastapi-stack.md)). The lock file has not been
generated yet.

### Build

No build step. Apps will be installed in editable mode per app.

### AI Assistants

Claude Code and GitHub Copilot are both supported, steered by [`AGENTS.md`](../../AGENTS.md)
(canonical) with thin pointers in `CLAUDE.md` and `.github/copilot-instructions.md`.

## Build and Development Process

### Environment Setup

See [`docs/development/setup.md`](../development/setup.md) and
[`environment.md`](../development/environment.md). Finalised in MVP-001 phase 1.

### Running Tests

`pytest` from the repo root, once apps exist (MVP-001).

### Development Workflow

`Roadmap → MVP → Plan → Implementation → Test → Pull Request` — see
[`docs/development/methodology.md`](../development/methodology.md).

### Git Workflow

Feature branches off `main`, pull request to `main`; the owner makes all commits and
merges. See [`docs/standards/git.md`](../standards/git.md).

### CI/CD

CI (GitHub Actions) is present as a template with placeholders; it is adapted in MVP-001.
Deployment does not exist yet.

### Deployment

None yet. Planned: deploy every app on merge to `main` to one Azure environment
([ADR-004](decisions/ADR-004-deploy-every-app-to-one-azure-environment.md)).

## Architectural Observations

- The project material recommends a modular monolith; the owner chose one app per
  responsibility, each deployed separately, in one repository
  ([ADR-003](decisions/ADR-003-monorepo-one-app-per-responsibility.md)). Consequences that
  follow from the material's own principles still apply: OneDrive is the source of truth,
  any index is rebuildable, MCP exposes only typed recipe operations, and AI output is
  validated and approved by a human before it is saved.
- Shared code moves to `packages/` only when at least two apps reuse it.

## Planned Evolution of the Workspace

> Everything in this section is **planned**, not existing.

- **MVP-001 (planned):**
  - `apps/api/` — recipe API service (FastAPI). Starts with a health endpoint only.
  - `apps/web/` — user-facing app (FastAPI + Jinja2, server-rendered HTML). Starts with a
    start page that reports api status.
  - CI with quality gates, and a deploy workflow to Azure.
- **Later (candidate apps, decided by the MVP that needs them):** responsibilities named in
  the project material — AI extraction, OneDrive storage and indexing, the AI assistant
  and its MCP tools — may become separate apps or stay inside `api`. Not decided.
- Planned stack: Python, FastAPI, Jinja2, Pydantic, SQLite/FTS5 when needed
  ([ADR-002](decisions/ADR-002-python-fastapi-stack.md)).
- Planned integrations: Microsoft Graph for a **private** OneDrive (personal Microsoft
  account), one multimodal AI model (provider Unknown – to be decided, likely configured
  via Azure AI Foundry).

## Open Questions and Areas Not Yet Implemented

- Python version: latest stable — exact version Unknown – to be decided (MVP-001 §0).
- Azure hosting service, region, subscription and resource naming: Unknown – to be decided
  (MVP-001 §0).
- AI model and provider for image interpretation and chat: Unknown – to be decided (MVP-002).
- Persistent recipe file format (Markdown, JSON, other): Unknown – to be decided.
- Which test images get human-reviewed expected results, and who approves them: Unknown.
- MCP: only the project's own assistant, or also external AI clients? MCP transport? Unknown.
- How SQLite as a rebuildable index fits services hosted in Azure: Unknown – decided in R3.
- User authentication for the online environment (personal Microsoft account) and
  service-to-service authentication: Unknown – required before real recipe data flows.
- Retention and deletion of original images: Unknown.
- User-initiated re-read vs. automatic sync of OneDrive: Unknown.
