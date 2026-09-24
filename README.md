<p align="center">
  <img src="./profile/docs/project-banner.svg" alt="Danmante project banner" width="960" />
</p>

# Danmante

A professional, safety-first digital health foundation for secure patient access, jurisdiction-aware clinical workflows, and trusted pharmacy coordination.

## Purpose of this repository

This repository serves as the GitHub foundation for the Danmante project. It centralizes governance, contribution standards, issue templates, CI workflow setup, and the public-facing documentation needed to support a credible open-source digital health initiative.

## Repository structure

```text
.
├── .github/                     # GitHub configuration and automation
│   ├── ISSUE_TEMPLATE/          # Bug and feature request templates
│   ├── workflows/              # CI/CD automation
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── dependabot.yml
├── profile/                    # Public profile and branding assets
│   ├── README.md
│   └── docs/
├── docs/                       # Project documentation index and concept docs
│   ├── README.md
│   ├── architecture/
│   ├── clinical/
│   ├── security/
│   └── compliance/
├── .env.example                # Example environment configuration
├── .gitignore
├── API.md
├── CLINICAL_SAFETY.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── DANMANTE_CANONICAL_TRUTH.md
├── DATA_PROCESSING.md
├── GOVERNANCE.md
├── LICENSE
├── PRIVACY.md
├── PRODUCTION_READINESS_REPORT.md
├── SECURITY.md
├── README.md
└── danmante-logo.png
```

## Core standards

- Safety-first design and fail-closed operational logic
- Clear contribution rules and review gates
- Security-first vulnerability handling
- Clinical and jurisdiction-aware decision processes
- Documentation that is maintainable and reviewable

## Key documentation

- [Project profile](./profile/README.md)
- [Contributing guide](./CONTRIBUTING.md)
- [Security policy](./SECURITY.md)
- [Clinical safety policy](./CLINICAL_SAFETY.md)
- [Governance](./GOVERNANCE.md)
- [Production readiness report](./PRODUCTION_READINESS_REPORT.md)
- [Documentation index](./docs/README.md)

## Workflow and automation

This repository includes GitHub automation for:

- CI validation
- pull request checklists
- issue intake
- dependency updates

See the [workflow configuration](./workflows/ci.yml) and the issue templates in [ISSUE_TEMPLATE](./ISSUE_TEMPLATE).

## Contribution model

We welcome collaboration that is well-scoped, documented, and respectful. For safety-sensitive work, follow the review standards defined in [CLINICAL_SAFETY.md](./CLINICAL_SAFETY.md) and [SECURITY.md](./SECURITY.md).

## License

This project is licensed under the [MIT License](./LICENSE).
