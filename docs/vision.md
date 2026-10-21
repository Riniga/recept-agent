# Vision

## Purpose

ReceptAgent turns paper recipes — handwritten notes, clippings, printouts — into a
reviewed, searchable recipe library stored in the user's own OneDrive, and lets the user
ask an AI assistant about that collection.

ReceptAgent is also a demonstration of AI-assisted development with Claude Code and GitHub
Copilot, following [Skanskas utvecklingsmetodik](methodology/index.md).

The original project material is kept unchanged as historical input in
[`docs/initial-idea/`](initial-idea/).

---

## Vision

A user can photograph or scan a recipe, have it interpreted by AI, correct it, and keep it
in a library they own and can read outside ReceptAgent — then find it again and ask
questions about their collection, with answers grounded in their own recipes.

The project should support:

* Interpreting recipe images and PDFs into structured recipe fields, marking uncertain or
  unreadable values instead of guessing them.
* Reviewing, correcting and explicitly approving an interpretation before anything is
  saved.
* Saving approved recipes and their originals in the user's private OneDrive under
  `/Recept/`, in an open format that is readable outside the application.
* Finding, viewing, editing and deleting recipes, and re-reading changes made directly in
  OneDrive — with conflicts shown, never silently overwritten.
* Asking an AI assistant about the collection, with source references, and requesting
  variants (e.g. a different number of servings) that are saved only after explicit
  approval.

---

## Principles

* **One MVP at a time.** Every MVP is deployed and fully testable both locally and in the
  Azure environment.
* **Separation of responsibilities.** Each responsibility is its own app under `apps/`,
  deployed as its own service — all in this one repository
  ([ADR-003](architecture/decisions/ADR-003-monorepo-one-app-per-responsibility.md)).
* **The human approves.** AI output is a proposal until a person approves it. Recipe
  content is treated as data, never as instructions to an AI.
* **OneDrive is the source of truth** for the user's recipes and originals; any local
  index can be rebuilt from it.
* **Failures are visible.** Errors from AI, Microsoft Graph, storage or indexing are shown
  clearly and never reported as success.
* **Simplest thing that meets a verified need.** No queues, extra providers or extra
  infrastructure without a concrete requirement.
* **Quality gates first.** CI with lint, tests, secret scanning and dependency scanning is
  in place before AI tools write production code.
* **AI-assisted development is a first-class workflow**, under the methodology's rules for
  human review and accountability.
