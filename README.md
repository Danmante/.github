<div align="center">

<img width="140" height="140" alt="Danmante logo" src="https://github.com/user-attachments/assets/2e4b1bab-cd1a-4016-acaa-4753fc6672a4" />

# Danmante — `.github`

### Community health files, CI, and contribution workflow for the Danmante repository

</div>

---

## What's in here

```
.github/
├── workflows/
│   └── ci.yml                     → CI pipeline (lint, typecheck, tests, build, security scan)
├── ISSUE_TEMPLATE/
│   ├── bug_report.md              → Bug report template
│   └── feature_request.md         → Feature request template
├── PULL_REQUEST_TEMPLATE.md       → PR checklist
└── dependabot.yml                 → Automated dependency & GitHub Actions updates
```

---

## CI Pipeline (`workflows/ci.yml`)

Runs on every pull request and on push to `main`:

| Job | Steps |
|---|---|
| **build-test** | install → lint → typecheck → unit tests → integration tests → build |
| **security-scan** | dependency audit, secret scanning (TruffleHog) |
| **accessibility** | placeholder hook for axe-core/pa11y once a deployed preview exists |

Production deployment is expected to fail if critical tests fail, typecheck fails, the build fails, required secrets are missing, or migrations are invalid — see [`../PRODUCTION_READINESS_REPORT.md`](../PRODUCTION_READINESS_REPORT.md) for current CI execution status (this environment has no outbound network access, so the pipeline has been authored but not yet run end-to-end).

---

## Issue Templates

- **Bug report** — reproduction steps, expected behavior, and the affected area (`frontend`/`backend`/`clinical`/`pharmacy`/`security`/`payments`/`identity`/`fhir`/`jurisdiction`/`devops`).
- **Feature request** — problem statement, proposed solution, and a required flag for clinical/legal impact. Any feature with clinical or legal impact must be tagged `area:clinical` and routed through the review process in [`../CLINICAL_SAFETY.md`](../CLINICAL_SAFETY.md) before merge.

## Pull Request Template

Every PR checklist confirms: lint/typecheck/tests pass locally, no secrets committed, docs updated if behavior changed, and — critically — that any change touching clinical, pharmacy, verification, or payment code has been checked against [`../CLINICAL_SAFETY.md`](../CLINICAL_SAFETY.md) and [`../SECURITY.md`](../SECURITY.md).

## Dependabot

Weekly automated update checks for both npm dependencies and GitHub Actions versions, keeping the supply chain current without manual tracking.

---

## Labels used across issues & PRs

**Area:** `area:frontend` `area:backend` `area:clinical` `area:pharmacy` `area:security` `area:payments` `area:identity` `area:fhir` `area:jurisdiction` `area:devops`
**Priority:** `priority:critical` `priority:high` `priority:medium` `priority:low`
**Type:** `type:bug` `type:feature` `type:security` `type:clinical` `type:documentation`
**Status:** `status:blocked` `status:ready` `status:in-progress` `status:review`

---

For the full project overview, see the [main README](../README.md).
