# Plan: MVP-001 – Walking skeleton

Reference: [`docs/mvp/MVP-001-walking-skeleton.md`](../mvp/MVP-001-walking-skeleton.md)

**Status:** Not started — awaiting plan review.

Work happens on one feature branch (`feature/mvp-001-walking-skeleton`) and ends in one
pull request. Each phase below is one commit, made by the owner after a short report.

## 0. Investigation

Nothing below is locked until it has been checked for real. Record the findings here (date,
what was checked, result) before the phase that depends on them.

| # | Question | How to check | Needed before |
|---|----------|--------------|---------------|
| 0.1 | Which Python version is the latest stable release that conda-forge, FastAPI, Jinja2, Pydantic, Uvicorn, Ruff and Semgrep all support? | `conda search -c conda-forge python`; install the candidate env and import/run each tool | Phase 1 |
| 0.2 | Does the chosen Python version have a supported runtime in the candidate Azure hosting service? | Azure CLI / portal runtime list for the service | Phase 1, 6 |
| 0.3 | Which Azure hosting service is the simplest fit for two small FastAPI apps deployed separately: App Service (Linux) or Container Apps? Compare startup command support, cost of an idle demo, OIDC deploy action support, health-check support. | Deploy a throwaway hello-world to the candidate; record the result | Phase 6 |
| 0.4 | Which Azure subscription, region and resource naming to use? | Ask the owner; record the answer | Phase 6 |
| 0.5 | How does `web` find `api` locally and in Azure (config key, URL format), and does service-to-service traffic need any restriction in this MVP? | Decide from 0.3 result; keep to one env var | Phase 3 |
| 0.6 | Coverage baseline: what is the real coverage of the two apps once their tests exist? | `pytest --cov` after phase 3 | Phase 5 |
| 0.7 | Do Semgrep's `p/security-audit` + `p/owasp-top-ten` rulesets give a useful signal on this code without noise? | Run locally against `apps/` | Phase 5 |
| 0.8 | Is the repository public or private, and are GitHub rulesets / private vulnerability reporting / Advanced Security available on its plan? | Repository settings, `gh api repos/Riniga/recept-agent` | Phase 7 |
| 0.9 | Do Windows (local) and Linux (CI/Azure) resolve the same `requirements-lock.txt`? | Compile locally, compare with CI's drift check | Phase 5 |

Findings: *(none yet)*

## 1. Goal

Two minimal apps, `apps/api` and `apps/web`, that run with one command locally and as two
separate Azure services after a merge to `main`, behind blocking CI quality gates, with the
first methodology assessment recorded.

## 2. Scope boundary

- **In:** `environment.yml`, `requirements.in`, `requirements-lock.txt`, `pyproject.toml`,
  `pytest.ini`, `.pre-commit-config.yaml`, `.github/workflows/ci.yml`, a new deploy
  workflow, `.github/dependabot.yml`, `.env.example`, `apps/api/`, `apps/web/`, a local-run
  script and Azure provisioning script(s) under `scripts/`, `docs/development/*`,
  `docs/methodology-compliance/*`, `README.md`, `docs/architecture/*`.
- **Out:** recipe models, AI integration, OneDrive/Graph, authentication, SQLite, MCP; a
  shared `packages/` package (nothing is reused by two apps yet); IaC tools; extra
  environments.

## 3. Chapters addressed

- B2 Kodstandard & stil (Ruff), B5 Dokumentation av kod (README, ADRs)
- C1 Secure coding (SAST), C2 Beroendehantering (lock, licences, Dependabot),
  C4 Hantering av hemligheter (detect-secrets, OIDC)
- D1 Testning (coverage floor, AI-TDD), D2 Kodgranskning (branch protection),
  D3 Definition of Done
- E1 Versionshantering, E2 CI/CD, E3 Miljöhantering, E4 Observability (health checks)
- F1–F3 AI collaboration (instruction files, scan)
- Assessment of all areas A–F

## 4. TODOs

### Phase 1 – Environment and tooling

- [ ] 1.1 Record findings 0.1–0.2. Pin the chosen Python version in `environment.yml`,
      `pyproject.toml` (`target-version`) and CI.
- [ ] 1.2 Name the Conda env `recept-agent`; add the runtime dependencies (FastAPI,
      Uvicorn, Jinja2, an HTTP client for web → api) and test dependencies (pytest-cov)
      to `requirements.in`, each justified in a comment; compile `requirements-lock.txt`.
- [ ] 1.3 Point `pyproject.toml` (Ruff `src`, isort `known-first-party`, coverage
      `source`) and `pytest.ini` (`testpaths`) at `apps/api` and `apps/web`.
- [ ] 1.4 Update `docs/development/setup.md`, `environment.md`, `tools.md` with the real
      version and env name.

Acceptance: `conda env create -f environment.yml` succeeds; `ruff check .` and
`pre-commit run --all-files` pass.

Commit: `Set up Conda environment and tooling for apps/web and apps/api`

### Phase 2 – `apps/api` skeleton (tests first)

- [ ] 2.1 Write tests first, reviewed by the owner before implementation: `GET /health`
      returns 200 with a body identifying the service.
- [ ] 2.2 Implement the minimal FastAPI app as an installable package
      (`pip install -e apps/api`).

Acceptance: tests written in 2.1 pass; nothing else is implemented.

Commit: `Add minimal api app with health endpoint`

### Phase 3 – `apps/web` skeleton (tests first)

- [ ] 3.1 Record finding 0.5. Write tests first, reviewed by the owner: `GET /` renders a
      ReceptAgent page showing api status from a mocked api; when api is unreachable the
      page shows a clear error (not a success); `GET /health` returns 200.
- [ ] 3.2 Implement with FastAPI + Jinja2; api base URL from one environment variable,
      added to `.env.example` without a value.

Acceptance: tests pass offline (api mocked); no real network calls in tests.

Commit: `Add minimal web app that reports api status`

### Phase 4 – Local run

- [ ] 4.1 Add a script under `scripts/` that starts both apps locally with the right
      configuration; document it in `README.md`, `AGENTS.md` "Local commands" and
      `docs/development/setup.md`.
- [ ] 4.2 Verify manually: start both → page shows api reachable; stop api → page shows
      the error.

Commit: `Add local run script for web and api`

### Phase 5 – CI quality gates

- [ ] 5.1 Record findings 0.6, 0.7, 0.9.
- [ ] 5.2 Adapt `ci.yml`: env name, one install step per app, Semgrep scoped to `apps/`,
      licence-scan `--ignore-packages` with the two app package names, test job with
      coverage.
- [ ] 5.3 Set `fail_under` just below the measured baseline; record measurement and
      reasoning in `interpretations.md`.
- [ ] 5.4 Confirm `dependabot.yml` covers pip and GitHub Actions.

Acceptance: all CI jobs pass on the pull request; deliberately breaking a test or adding a
fake secret makes the corresponding job fail (checked locally or in a throwaway commit,
then reverted).

Commit: `Wire CI quality gates for web and api`

### Phase 6 – Azure deployment

- [ ] 6.1 Record findings 0.2–0.4 and the hosting choice; write a follow-up ADR if the
      choice is significant.
- [ ] 6.2 Add idempotent provisioning script(s) under `scripts/` that create the resources
      for both apps and the OIDC federated credential (subject: `repo:Riniga/recept-agent:ref:refs/heads/main`).
      The owner runs them; this is a production-writing action and is never run by CI or
      an AI tool.
- [ ] 6.3 Add a deploy workflow triggered on push to `main`: one job per app, then a smoke
      test job calling each `/health` and the web start page (asserting api reachable).
- [ ] 6.4 Document required GitHub secrets/variables in `README.md`; update
      `docs/development/secrets-rotation.md` with the real names.

Acceptance: after merge, both apps run as separate Azure services and the smoke test passes.
(Verified after the PR is merged; recorded in "Outcome at close".)

Commit: `Add Azure provisioning script and deploy workflow`

### Phase 7 – Repository settings

- [ ] 7.1 Record finding 0.8. Owner configures branch protection / ruleset on `main`:
      pull request required, all CI checks required, no force-push; document the applied
      settings in `docs/development/repo-settings.md`.
- [ ] 7.2 Record exception EX-001 (single maintainer, 0 required approvals) in
      `exceptions.md` and as a gap-register row.
- [ ] 7.3 Enable private vulnerability reporting if available; update `SECURITY.md`.

Commit: `Document repository protection and single-maintainer exception`

### Phase 8 – Methodology baseline

- [ ] 8.1 Write `a-grundforutsattningar.md` … `f-ai-samarbete.md` from `_template.md`.
- [ ] 8.2 Populate `gap-register.md` and `interpretations.md` (formatter/linter, SAST tool,
      secret scanning, coverage, branch model, deploy-on-merge, AI tools: Claude Code and
      GitHub Copilot, AI-assistance marking in PRs).
- [ ] 8.3 Link ADR-001 and this MVP from `docs/methodology-compliance/README.md`.

Commit: `Add initial methodology compliance assessment`

### Phase 9 – MVP review

- [ ] 9.1 Update `README.md`, `docs/architecture/overview.md`, `current-state.md` (apps
      implemented, test counts), `docs/roadmap.md`.
- [ ] 9.2 Verify every acceptance criterion locally and in CI; prepare PR title and text.
- [ ] 9.3 After merge and deploy: verify online criteria, fill in "Outcome at close".

Commit: `Update architecture docs and close MVP-001 review`

## 5. Risks / open questions

- **Latest Python vs. ecosystem support** (0.1–0.2): the newest release may not yet be
  supported by every tool or by the Azure runtime; fall back to the newest version that
  is, and record why.
- **Hosting choice** (0.3): two separately deployed services cost more and need more
  configuration than one; keep to the cheapest tier that supports the demo.
- **Public endpoints without authentication**: acceptable only while the apps hold no data.
  Must be closed before R2 handles real recipe images.
- **Security impact (STRIDE, short)** — new external-facing surface (two public
  endpoints) and a new trust boundary (GitHub Actions → Azure):
  - *Spoofing:* deploy identity via OIDC scoped to `main` of this repo only.
  - *Tampering:* deploy only from merged, CI-gated `main`.
  - *Repudiation:* every deploy traceable to a merged PR and workflow run.
  - *Information disclosure:* no data, no secrets in the apps; health endpoints expose no
    version or environment detail beyond service name.
  - *Denial of service:* accepted for a demo; no SLA.
  - *Elevation of privilege:* federated identity gets the minimum role on the resource
    group only.
- **Single environment**: a defect merged to `main` is live immediately; mitigated by CI
  gates and the smoke test.
