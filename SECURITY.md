# Security

## Reporting a vulnerability

If you believe you have found a security vulnerability in this project, **do not open a
public issue or pull request.** Report it directly to the project owner, Rickard Nisses-Gagnér, via GitHub's private
vulnerability reporting for this repository (Security → Report a vulnerability) once it
is enabled (MVP-001 phase 7).

<!-- If your organisation has a central security function and coordinated vulnerability
     disclosure (CVD) process, point at it here rather than inventing a parallel one — see
     the worked example this file was drawn from, which withheld the real contact details
     until its formal governance process completed, and tracked that explicitly as a gap. -->

## Coordinated vulnerability disclosure

<!-- If CVD is owned centrally by your organisation's security function, say so and link
     the relevant methodology chapter or policy instead of authoring your own. -->

This repository does not run a separate disclosure process; it follows the organisation's
central one.

## `security.txt`

Not published yet. Tracked as a gap in the first methodology assessment (MVP-001).

## Secrets and configuration

- Secrets are never committed. `.env` is git-ignored; `.env.example` (repo root) lists the
  required keys with no values.
- Deployment secrets are stored as CI/CD platform secrets (see `README.md`).
- If a secret is ever exposed, it is treated as compromised: rotate it immediately, then
  clean history. See
  [`docs/development/secrets-rotation.md`](docs/development/secrets-rotation.md) and
  [`docs/methodology/c-sakerhet/hantering-av-hemligheter.md`](docs/methodology/c-sakerhet/hantering-av-hemligheter.md).
