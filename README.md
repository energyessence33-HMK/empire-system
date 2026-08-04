# Kingdom Executor OS

Kingdom Executor OS is a TypeScript-first operating system for executive governance, brand operations, knowledge workflows, and AI-assisted execution.

This repository is intentionally bootstrapped documentation-first. Before significant application code is added, the project records the governing doctrine, product requirements, architecture, engineering standards, brand manuals, AI executive roles, workflow library, and deployment runbooks that future implementation must trace back to.

## Repository Structure

```text
docs/
  01-Constitution/
  02-PRD/
  03-System-Architecture/
  04-Engineering-Handbook/
  05-Brands/
  06-Executive-Council/
  07-Workflow-Library/
  08-Operations/
apps/
packages/
tests/
scripts/
artifacts/
```

## Current Build State

The current milestone is **Volume I — Kingdom Executor Constitution v1.0**. Application scaffolding should remain minimal until Volume II and Volume III define approved feature scope and system boundaries.

## Development Principles

- TypeScript first.
- React Native + Expo for mobile applications.
- Supabase for backend services.
- No hardcoded secrets; use environment variables.
- Modular architecture with reusable packages.
- Tests for every new feature.
- GitHub Actions and Expo EAS compatible deployment paths.

## Validation

Until application code exists, validation focuses on documentation completeness and repository hygiene:

```bash
find docs -maxdepth 2 -type f | sort
```
