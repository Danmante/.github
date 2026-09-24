# Clinical Safety Policy

Danmante is a digital health platform and must be designed with clinical safety as a primary concern. This document outlines the expected safety boundaries for software changes, workflows, and product decisions.

## Principles

- Fail closed when jurisdiction or clinical rules are uncertain.
- Never let the software assume legal or clinical authority without explicit validation.
- Treat all healthcare workflows as scope-limited and review-dependent.
- Require review for any change to clinical logic, pharmacy workflows, or jurisdiction checks.

## Required review

Changes affecting clinical decision support, prescriber scope, pharmacy rules, verification workflows, or patient safety logic require a documented review path and appropriate approvals before merge.

## Limitations

This repository does not replace licensure, regulated practice, or legal counsel. It exists to support a safer and more auditable digital-health architecture.
