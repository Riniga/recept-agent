# ADR-001: Follow Skanskas utvecklingsmetodik

**Status:** Accepted
**Date:** 2026-10-21

## Context

ReceptAgent is a demonstration of AI-assisted development. Its value as a demo depends on
showing a credible, repeatable way of working: quality gates, human review, secret
handling and clear rules for what AI tools may do on their own. The repository was created
from a template that already contains the organisation's methodology
([`docs/methodology/`](../../methodology/index.md), areas A–F, 21 chapters) and a
structure for recording compliance ([`docs/methodology-compliance/`](../../methodology-compliance/)).

The project has a single maintainer (Rickard Nisses-Gagnér), who is product owner,
technical lead and security contact. Some methodology requirements — notably review by
someone other than the author — cannot be met by one person.

## Decision

The project follows Skanskas utvecklingsmetodik in full. The project's standing against
it, the project-level interpretations and any exceptions are recorded in
`docs/methodology-compliance/`; the first assessment is part of
[MVP-001](../../mvp/MVP-001-walking-skeleton.md).

Requirements that a single maintainer cannot meet are recorded as time-boxed exceptions in
`exceptions.md` and as open rows in `gap-register.md` — not silently skipped.

## Consequences

**Benefits:**

- The demo shows the methodology working on a real project, end to end.
- AI tools (Claude Code, GitHub Copilot) operate under explicit, written rules.
- Gaps are visible and tracked rather than implied.

**Trade-offs:**

- More process than a solo demo strictly needs: assessments, a gap register and
  interpretations must be kept current.
- Single-maintainer review exceptions remain open until a second reviewer exists.
- `docs/methodology/` is the organisation's text and is not edited here; project-specific
  choices must live in `docs/methodology-compliance/` instead.

## Alternatives considered

- **Use the methodology as loose reference only**: rejected — the point of the demo is to
  show the methodology applied, not described.
- **Adopt a subset of chapters**: rejected — deciding which chapters "don't apply" up front
  hides gaps; the assessment records `not applicable yet` explicitly instead.
