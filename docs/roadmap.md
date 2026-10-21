# Roadmap

This roadmap describes the major development stages and capabilities planned for
ReceptAgent.

Each roadmap area is implemented through one or more MVPs. Once an MVP has been created, it
becomes the source of truth for that work — this file should stay a lightweight index, not
a duplicate of MVP content. See `docs/development/methodology.md` "Roadmap".

---

## Current Status

The project has been initialised from the template: vision, roadmap, initial ADRs and
MVP-001 with its plan are written. There is no application code yet. Current focus:
[MVP-001 – Walking skeleton](mvp/MVP-001-walking-skeleton.md).

---

## R1 – Foundation and delivery chain (Ongoing)

Establishes the repository, the chosen stack, CI with the methodology's quality gates,
deployment of every app to the Azure environment, and the first methodology assessment.

* [MVP-001 – Walking skeleton](mvp/MVP-001-walking-skeleton.md) — two minimal apps
  (`web`, `api`) that run locally and in Azure, behind CI quality gates.

## R2 – Recipe intake and review (Planned)

Establishes upload of recipe images/PDFs, AI interpretation with uncertainty marking, and
review, correction and explicit approval by the user.

* MVP-002 – From recipe image to reviewed interpretation — upload a recipe image or PDF,
  see the AI's interpretation with uncertain fields marked, correct it and approve it.

## R3 – Recipe library in OneDrive (Planned)

Establishes sign-in with a personal Microsoft account, saving approved recipes and
originals in OneDrive `/Recept/`, search, re-reading of OneDrive changes and visible
conflict handling.

## R4 – AI assistant and MCP (Planned)

Establishes questions about the collection answered with source references via typed MCP
tools, and recipe variants that are saved only after separate user approval.

## Backlog of ideas to be implemented / fixed

* Out of the current target picture (from `docs/initial-idea/`): family sharing, mobile
  app, shopping list, external recipe sites, nutrition analysis, semantic vector search.

---

## Maintaining the Roadmap

- Update "Current Status" whenever a roadmap area's status changes.
- Add a new `## R<n> – <Area>` section when a genuinely new phase starts; don't retrofit
  history into it.
- Keep MVP links current — an MVP is the source of truth for its own scope, this file just
  points at it.
