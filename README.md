# ReceptAgent

Turns photographed or scanned paper recipes into a reviewed, searchable recipe library in
the user's private OneDrive, with an AI assistant that answers questions about the
collection. Also a demonstration of AI-assisted development with Claude Code and GitHub
Copilot.

**Owner:** Rickard Nisses-Gagnér (product, engineering, security) · **Status:** initialised —
no application code yet; [MVP-001](docs/mvp/MVP-001-walking-skeleton.md) is next.

The workspace is designed to grow over time by adding independent applications that share
a common architecture, development process, coding standards, and reusable components.

## Vision

Paper recipes become a library the user owns and can ask questions about — with every AI
interpretation reviewed and approved by the user. See [`docs/vision.md`](docs/vision.md).

## Workspace Structure

```text
apps/                  (planned, MVP-001 — one app per responsibility, each its own Azure service)
    web/               User-facing app, server-rendered HTML
    api/               Recipe API

packages/              (none yet — only for code reused by at least two apps)

docs/
    Architecture, development process, standards, roadmap, MVPs and implementation plans
```

> **Note**
>
> The workspace is currently being established. Additional applications, shared packages
> and documentation will be introduced incrementally as the project evolves.

## Development Process

Every change follows the same lightweight process:

```text
Roadmap
    ↓
MVP
    ↓
Implementation Plan
    ↓
Implementation
    ↓
Test
    ↓
Pull Request
```

See [`docs/development/methodology.md`](docs/development/methodology.md) for the full
process, and [`AGENTS.md`](AGENTS.md) for how AI tools work within it.

## Getting Started

1. Read [`AGENTS.md`](AGENTS.md) — the canonical instruction file for how work happens
   here (human or AI).
2. Follow [`docs/development/setup.md`](docs/development/setup.md) and
   [`environment.md`](docs/development/environment.md) to set up your local environment.
3. Read [`docs/standards/`](docs/standards/) — coding, testing, git, documentation,
   dependencies.
4. If starting a brand-new project from this template, put the project material in
   `docs/initial-idea/` and run
   [`docs/claude-prompts/initialize-project.md`](docs/claude-prompts/initialize-project.md).

## Required GitHub Actions secrets

None yet. The Azure deploy workflow (MVP-001 phase 6) will use OIDC federation; the exact
secret/variable names are documented here when it is added.

| Secret | Purpose |
|--------|---------|
| — | — |

## Documentation

| Area | Location |
|------|----------|
| Architecture | [`docs/architecture/`](docs/architecture/) |
| Development process | [`docs/development/methodology.md`](docs/development/methodology.md) |
| Standards | [`docs/standards/`](docs/standards/) |
| Organisation methodology | [`docs/methodology/`](docs/methodology/) |
| Methodology compliance (internal) | [`docs/methodology-compliance/`](docs/methodology-compliance/) |
| Roadmap | [`docs/roadmap.md`](docs/roadmap.md) |
| MVPs | [`docs/mvp/`](docs/mvp/) |
| Implementation plans | [`docs/plans/`](docs/plans/) |
