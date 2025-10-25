<!--
Sync Impact Report
Version change: 0.0.0 -> 1.0.0
Modified principles:
- Placeholder -> Azure-Native Stack Compliance
- Placeholder -> Deterministic ASCII Transformation
- Placeholder -> Accessibility & Visual Integrity
- Placeholder -> Observability & Quality Gates
- Placeholder -> Security & Data Governance
Added sections: None
Removed sections: None
Templates requiring updates:
- [x] .specify/templates/plan-template.md
- [x] .specify/templates/spec-template.md
- [x] .specify/templates/tasks-template.md
Follow-up TODOs: None
-->

# ASCII Art Transformer Constitution

## Core Principles

### I. Azure-Native Stack Compliance
The product MUST implement the full Azure reference stack: Next.js with Tailwind CSS and shadcn/ui deployed via Azure Static Web Apps, a FastAPI backend hosted on Azure App Service or Container Apps, infrastructure codified with Azure Bicep, and managed services for PostgreSQL, Redis, Service Bus, Blob Storage, Entra ID, and Application Insights. Deviations require an explicit architecture review and written approval. Rationale: locking to the prescribed stack guarantees operational consistency with the standard framework and ensures deployments remain Azure-first.

### II. Deterministic ASCII Transformation
Image-to-ASCII conversion MUST run through a reproducible pipeline that ingests uploads from Blob Storage, processes them in FastAPI workers triggered via Service Bus, and writes normalized ASCII artifacts back to storage. Conversion algorithms MUST produce deterministic output given identical inputs, be covered by contract and regression tests, and expose configuration (resolution, character set, contrast) through typed APIs. Rationale: determinism keeps automated tests meaningful and maintains user trust in generated artwork.

### III. Accessibility & Visual Integrity
All user-facing experiences MUST satisfy WCAG 2.1 AA, including sufficient contrast for ASCII renderings, keyboard navigation, and descriptive alternatives for generated media. Frontend components MUST use shadcn/ui primitives and Tailwind tokens, and ASCII previews MUST offer zoom and theme controls for legibility. Rationale: accessibility is a contractual requirement and ensures ASCII art remains usable across assistive technologies.

### IV. Observability & Quality Gates
Backend and frontend code MUST emit OpenTelemetry traces, metrics, and structured logs routed to Azure Application Insights; Service Bus handlers MUST log correlation identifiers per conversion job. CI pipelines MUST execute pytest, Vitest, and Playwright suites as well as static analysis (ruff, black, mypy, eslint, prettier). Production releases require green telemetry dashboards and zero unresolved Sev1 alerts. Rationale: robust observability and automated gates prevent regressions in a media-processing workflow.

### V. Security & Data Governance
Authentication MUST rely on Microsoft Entra ID with least-privilege scopes, and secrets MUST reside in Azure Key Vault. Uploaded assets MUST undergo size validation, MIME sniffing, and content scanning before processing, and data retention policies MUST purge source images after ASCII conversion unless an explicit retention flag is set. Rationale: enforcing zero-trust practices protects user content and aligns with cloud security commitments.

## System Architecture Constraints

The solution MUST operate the canonical folder layout (`apps/frontend`, `apps/api`, `packages/shared`, `infra/bicep`). All infrastructure provisioning, including Storage, Database, Redis, Service Bus, CDN, and Monitoring, MUST be declared in Bicep modules with validated parameter files. Feature specs MUST plan for horizontal scaling of FastAPI workers and asynchronous job handling to meet throughput targets of 50 concurrent conversions with p95 latency under 5 seconds for standard images (<= 5 MB). Any third-party library additions REQUIRE evaluation for Azure compatibility and OSS licensing alignment.

## Development Workflow & Quality Gates

Work MUST begin with specification artifacts generated via `/speckit.plan`, `/speckit.spec`, and `/speckit.tasks`, each referencing this constitution in their Constitution Check sections. Pull requests MUST demonstrate: updated infrastructure manifests when resources change, migration scripts for PostgreSQL updates, and refreshed tests covering ASCII conversion paths. Code review approval requires documented evidence of passing CI, updated observability dashboards or alerts when telemetry changes, and confirmation that accessibility evaluations (manual or automated) pass for new UI flows.

## Governance

This constitution supersedes conflicting guidance. Amendments REQUIRE a written proposal, architecture review sign-off, and updated validation in plan/spec/tasks templates. Version numbers follow semantic versioning: MAJOR for principle changes or removals, MINOR for new principles or sections, PATCH for clarifications. Compliance reviews MUST occur each release cycle, with findings tracked in project issues and resolved before deployment. Runtime guidance updates MUST remain synchronized with this document.

**Version**: 1.0.0 | **Ratified**: 2025-10-25 | **Last Amended**: 2025-10-25
