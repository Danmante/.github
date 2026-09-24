<div align="center">

<p align="center">
        <img src="./docs/project-banner.svg" alt="Danmante project banner" width="960" />
</p>

<p align="center">
  <img src="../danmante-logo.png" alt="Danmante logo" width="140" />
</p>

**Healthcare access, connected.**

Connecting patients with verified nurses and certified pharmacies through secure, jurisdiction-aware digital healthcare workflows.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Status](https://img.shields.io/badge/status-early--stage%20foundation-orange)](./PRODUCTION_READINESS_REPORT.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Clinical Safety](https://img.shields.io/badge/clinical%20safety-fail--closed-red)](./CLINICAL_SAFETY.md)
[![Security Policy](https://img.shields.io/badge/security-policy-informational)](./SECURITY.md)

[Overview](#overview) · [Why Danmante](#why-danmante) · [Architecture](#architecture) · [Getting Started](#getting-started) · [Roadmap](#roadmap) · [Safety & Security](#clinical-safety--security) · [Contributing](#contributing) · [Docs](#documentation)

</div>

---

## Overview

Danmante is a global, open-source digital-health platform designed to connect **patients**, **licensed nurses**, **certified pharmacies**, **pharmacists**, and **healthcare organizations** — with the goal of expanding access to affordable, secure, professionally delivered healthcare support, particularly for vulnerable populations and communities experiencing healthcare-access shortages.

Danmante lets a patient:

1. Create an account and complete their profile
2. Select their country and jurisdiction
3. Find verified healthcare professionals available in that jurisdiction
4. Book an eligible online consultation and pay securely
5. Attend the consultation and receive documented clinical guidance within the professional's legal scope
6. Receive appropriate care-navigation or pharmacy information where legally permitted
7. Continue appropriate follow-up

Every one of those steps is gated by a **Jurisdiction & Clinical Rules Engine** that fails closed — nothing is assumed to be legal just because it's technically possible.

> **Danmante is not an emergency service** and does not replace hospitals, doctors, licensed clinical judgment, or any national healthcare system.

---

## Why Danmante

Healthcare access is uneven worldwide, and the rules governing who can treat whom, where, and how differ by country, state, province, and profession. Most "healthcare app" templates ignore this and hard-code a single country's assumptions. Danmante is built the opposite way:

- 🌍 **Jurisdiction-first, not jurisdiction-as-an-afterthought** — every clinical, prescribing, and pharmacy workflow checks the applicable region's rules before it runs, and blocks itself if those rules aren't established.
- 🩺 **Nurse-centered, scope-of-practice aware** — nurses are never presented as able to do more than their verified jurisdiction record allows.
- 💊 **Pharmacy-connected, not pharmacy-assumed** — medication workflows require jurisdiction eligibility, pharmacist review, and prescription rules to line up before anything is dispensed.
- 🤖 **AI as support, never as the clinician** — AI can summarize, translate, and structure intake, but it never diagnoses, prescribes, or overrides a licensed professional.
- 🔐 **Security and privacy by architecture** — RBAC/ABAC, object-level authorization, tamper-evident audit logs, encryption at rest and in transit.
- ⛓️ **Payments without compromising clinical data** — traditional card payments and WalletConnect/Celo crypto payments are supported, but medical records are **never** stored on-chain.
- 🌐 **Multilingual by design** — English, Haitian Creole, French, and Spanish from day one, with a clean path to add more.
- ♿ **Accessible by default** — built toward WCAG 2.2 AA.

---

## Architecture

Danmante is a monorepo with clear domain boundaries, so clinical, pharmacy, payment, and identity code can each be reviewed and audited independently.

```
danmante/
├── apps/            → role-aware frontends (web, patient, nurse, pharmacy, admin)
├── packages/        → shared domain logic (jurisdiction, auth, clinical, pharmacy,
│                      identity, payments, wallet, localization, notifications,
│                      audit, fhir, security, config, ui)
├── services/        → deployable backends (api, consultation, verification,
│                      pharmacy, payments, notifications, clinical-rules, identity)
├── database/        → schema, migrations, RLS/immutability policies, seed data
├── docs/            → architecture, clinical, compliance, security, privacy,
│                      api, fhir, jurisdiction, governance docs
├── infrastructure/  → docker, deployment, monitoring, backups
├── tests/           → unit, integration, e2e, security, clinical, accessibility
└── .github/         → CI workflows, issue/PR templates, dependabot
```

**Core data flow (clinical/prescribing path):**

```
Patient → Book consultation → Jurisdiction Engine check → Appointment → Encounter
        → (optional) Medication request → Jurisdiction Engine check → Pharmacy request
        → Pharmacist review → Fulfillment (only where legally permitted)
```

Clinical data, identity, payments, and blockchain transactions are kept in **separate, independently-failing subsystems** — if the payment rail or the blockchain is unavailable, the clinical record system keeps working, and vice versa.

Full breakdown: [`docs/architecture/ARCHITECTURE.md`](./docs/architecture/ARCHITECTURE.md)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend | Node.js, Fastify, REST + OpenAPI |
| Database | PostgreSQL, Redis (caching/rate-limiting) |
| Interoperability | FHIR-compatible data architecture |
| Telehealth | WebRTC (video/audio) |
| Auth | OAuth/OIDC, MFA |
| Payments | Card payment-provider abstraction, WalletConnect, Celo (CeloHT dApp) |
| Storage | S3-compatible encrypted object storage |
| Observability | OpenTelemetry |
| CI/CD | GitHub Actions, Turborepo, Docker |

Every dependency is chosen for a reason — not because it's trending.

---

## Getting Started

```bash
git clone https://github.com/<your-org>/danmante.git
cd danmante
pnpm install
cp .env.example .env
pnpm dev
```

**Requirements:** Node.js 20+, pnpm 9+, PostgreSQL 15+, Redis (optional).

See [`.env.example`](./.env.example) for the full configuration surface (database, auth, storage, payments, WalletConnect/Celo, telehealth, FHIR, observability). Danmante refuses to start in production if security-critical configuration is missing.

---

## Roadmap

- [x] **Phase 1 — Foundation:** monorepo, database schema, jurisdiction engine, API skeleton, CI, docs
- [ ] **Phase 2 — Identity & Verification:** registration flows, credential/license verification, admin review
- [ ] **Phase 3 — Consultation:** scheduling, secure video/audio, clinical documentation
- [ ] **Phase 4 — Pharmacy:** discovery, jurisdiction checks, referral & fulfillment workflow
- [ ] **Phase 5 — Payments:** card + WalletConnect/Celo integration, idempotent transaction ledger
- [ ] **Phase 6 — FHIR:** interoperability layer
- [ ] **Phase 7 — Security Hardening:** threat modeling, pen-test prep, dependency audits
- [ ] **Phase 8 — Production Readiness:** monitoring, backups, recovery drills, performance, accessibility

Current status in detail: [`PRODUCTION_READINESS_REPORT.md`](./PRODUCTION_READINESS_REPORT.md)

---

## Clinical Safety & Security

Danmante treats patient safety and security as non-negotiable, above feature velocity.

- **Fail-closed jurisdiction engine** — [`packages/jurisdiction`](./packages/jurisdiction) blocks any clinical, prescribing, or pharmacy workflow unless the applicable jurisdiction's rules are explicitly configured and active.
- **Verification is a process, not an upload** — nurses and pharmacies move through `pending → submitted → under_review → verified → expired/suspended/rejected/revoked`, with human review required before "verified" is shown.
- **Tamper-evident audit log** — every sensitive action is recorded in a hash-chained, append-only audit trail ([`packages/audit`](./packages/audit)).
- **Zero trust** — every sensitive authorization decision is enforced server-side; object-level authorization prevents one patient from ever reaching another's record via an ID change.
- **No PHI on-chain, ever** — blockchain is used strictly as a payment rail.

Read the full policies: [`CLINICAL_SAFETY.md`](./CLINICAL_SAFETY.md) · [`SECURITY.md`](./SECURITY.md) · [`docs/security/THREAT_MODEL.md`](./docs/security/THREAT_MODEL.md)

---

## Documentation

| Doc | Purpose |
|---|---|
| [`DANMANTE_CANONICAL_TRUTH.md`](./DANMANTE_CANONICAL_TRUTH.md) | Authoritative project facts |
| [`CLINICAL_SAFETY.md`](./CLINICAL_SAFETY.md) | Clinical safety boundaries |
| [`SECURITY.md`](./SECURITY.md) | Security policy & vulnerability reporting |
| [`PRIVACY.md`](./PRIVACY.md) / [`DATA_PROCESSING.md`](./DATA_PROCESSING.md) | Data handling & classification |
| [`docs/architecture/ARCHITECTURE.md`](./docs/architecture/ARCHITECTURE.md) | Full architecture breakdown |
| [`docs/architecture/DECISION_LOG.md`](./docs/architecture/DECISION_LOG.md) | Architecture decision records |
| [`docs/clinical/CLINICAL_WORKFLOW_MODEL.md`](./docs/clinical/CLINICAL_WORKFLOW_MODEL.md) | Clinical workflow model |
| [`docs/security/THREAT_MODEL.md`](./docs/security/THREAT_MODEL.md) | Threat model |
| [`docs/compliance/COMPLIANCE_OVERVIEW.md`](./docs/compliance/COMPLIANCE_OVERVIEW.md) | Jurisdiction-by-jurisdiction compliance status |
| [`API.md`](./API.md) | API reference |
| [`PRODUCTION_READINESS_REPORT.md`](./PRODUCTION_READINESS_REPORT.md) | What's implemented vs. planned |

---

## Contributing

Contributions are welcome — see [`CONTRIBUTING.md`](./CONTRIBUTING.md) for the workflow, labels, and code standards. Any change touching clinical, pharmacy, verification, or payment code must be read against [`CLINICAL_SAFETY.md`](./CLINICAL_SAFETY.md) and [`SECURITY.md`](./SECURITY.md) before merge.

Project governance and decision-making process: [`GOVERNANCE.md`](./GOVERNANCE.md). Community expectations: [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md).

---

## Founder

**Johnny Dubic** (Haiti) — Owner & Founder of Danmante.

> Ownership of Danmante does not grant clinical authority. Johnny Dubic is not represented as a clinician, CEO, or clinical decision-maker anywhere in this repository unless explicitly supported by future signed project documentation.

---

## License

Danmante is released under the [MIT License](./LICENSE). This license covers the software only — it is not a medical, legal, or regulatory certification of any kind.

<div align="center">

*Technology should expand access to healthcare — without pretending to replace healthcare professionals, emergency services, clinical judgment, or the laws that govern healthcare.*

</div>
