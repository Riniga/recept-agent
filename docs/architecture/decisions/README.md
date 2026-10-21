# ADR

Architecture Decision Records.

## Namnstandard

`ADR-001-kort-beslut.md`

Nygard-format: titel, status, kontext, beslut, konsekvenser. En godkänd ADR redigeras
aldrig i efterhand — om beslutet ändras skrivs en ny ADR som ersätter den, med en länk
mellan dem. Se [`docs/standards/documentation.md`](../../standards/documentation.md) and
`ADR-TEMPLATE.md` in this directory for the exact section structure.

## Index

| ADR | Beslut |
|-----|--------|
| [ADR-001](ADR-001-follow-skanska-development-methodology.md) | Follow Skanskas utvecklingsmetodik — Accepted |
| [ADR-002](ADR-002-python-fastapi-stack.md) | Python with FastAPI, Jinja2, Pydantic; Conda + pip-tools — Accepted |
| [ADR-003](ADR-003-monorepo-one-app-per-responsibility.md) | One repository, one app per responsibility — Accepted |
| [ADR-004](ADR-004-deploy-every-app-to-one-azure-environment.md) | Deploy every app to one Azure environment on merge to `main` — Accepted (hosting service Proposed) |
