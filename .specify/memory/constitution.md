<!--
Sync Impact Report
Version change: 1.0.0 -> 2.0.0
Modified principles:
- Azure-Native Stack Compliance -> Portable Web Stack Discipline
- Deterministic ASCII Transformation (content updated)
- Accessibility & Visual Integrity (content updated)
- Observability & Quality Gates (content updated)
- Security & Data Governance (content updated)
Added sections: None
Removed sections: None
Templates requiring updates:
- [x] .specify/templates/plan-template.md
- [x] .specify/templates/spec-template.md
- [x] .specify/templates/tasks-template.md
- [x] spec/SPEC.md
- [x] .github/workflows/validate-stack.yml
- [x] .github/PULL_REQUEST_TEMPLATE.md
Follow-up TODOs: None
-->

# ASCII Art Transformer Constitution

## Core Principles

### I. Portable Web Stack Discipline
The product MUST keep a reference implementation with Next.js (Tailwind CSS + shadcn/ui) in `apps/frontend` and FastAPI services in `apps/api`, each packaged as OCI-compliant containers and runnable via Docker Compose or Kubernetes. Infrastructure descriptions MUST remain provider-agnostic (e.g., Terraform, Pulumi, Crossplane) and MUST NOT hard-code Azure-only managed services. Introducing a cloud-specific primitive requires a documented portability fallback and architecture review approval. Rationale: enforcing a portable baseline prevents accidental cloud lock-in while preserving a consistent developer experience.

### II. Deterministic ASCII Transformation
Image-to-ASCII conversion MUST run through a reproducible pipeline that accepts uploads from an object storage interface (MinIO, S3, local filesystem) routed through a message queue or scheduler that is vendor-neutral (Redis Streams, RabbitMQ, Celery). Workers MUST produce deterministic output given identical inputs, expose configuration (resolution, character set, contrast) through typed APIs, and be covered by contract and regression tests. Rationale: determinism keeps automated tests meaningful and maintains user trust in generated artwork regardless of the hosting provider.

### III. Accessibility & Visual Integrity
All user-facing experiences MUST satisfy WCAG 2.1 AA, including sufficient contrast for ASCII renderings, keyboard navigation, and descriptive alternatives for generated media. Frontend components MUST use shadcn/ui primitives and Tailwind tokens (or documented accessible equivalents) and ASCII previews MUST offer zoom and theme controls for legibility across light and dark themes. Rationale: accessibility is a contractual requirement and ensures ASCII art remains usable across assistive technologies.

### IV. Observability & Quality Gates
Backend and frontend code MUST emit OpenTelemetry traces, metrics, and structured logs exported via OTLP to the project's observability stack (e.g., Tempo, Jaeger, Honeycomb). Queue handlers MUST preserve correlation identifiers for each conversion job. CI pipelines MUST execute pytest, Vitest, and Playwright suites as well as static analysis (ruff, black, mypy, eslint, prettier). Production releases require healthy observability dashboards with no unresolved Sev1 alerts. Rationale: robust observability and automated gates prevent regressions in a media-processing workflow.

### V. Security & Data Governance
Authentication MUST rely on an OpenID Connect provider with least-privilege scopes, and secrets MUST reside in a managed secret store (e.g., HashiCorp Vault, AWS Secrets Manager, Doppler) with `.env` files reserved for local development only. Uploaded assets MUST undergo size validation, MIME sniffing, and content scanning before processing, and data retention policies MUST purge source images after ASCII conversion unless an explicit retention flag is set. Rationale: enforcing zero-trust practices protects user content and aligns with compliance commitments across hosting environments.

## System Architecture Constraints

The solution MUST operate the canonical folder layout (`apps/frontend`, `apps/api`, `packages/shared`, `infra/ops`). All infrastructure provisioning, including storage, database, cache, messaging, CDN, and monitoring, MUST be declared using provider-agnostic infrastructure-as-code with environment overlays (e.g., Terraform, Pulumi, Ansible) stored under `infra/ops`. Feature specs MUST plan for horizontal scaling of FastAPI workers and asynchronous job handling to meet throughput targets of 50 concurrent conversions with p95 latency under 5 seconds for standard images (<= 5 MB). Any third-party library additions REQUIRE evaluation for portability across common cloud and self-hosted environments as well as OSS licensing alignment.

## Development Workflow & Quality Gates

Work MUST begin with specification artifacts generated via `/speckit.plan`, `/speckit.spec`, and `/speckit.tasks`, each referencing this constitution in their Constitution Check sections. Pull requests MUST demonstrate updated infrastructure manifests when resources change, migration scripts for PostgreSQL updates, and refreshed tests covering ASCII conversion paths. Code review approval requires documented evidence of passing CI, updated observability dashboards or alerts when telemetry changes, and confirmation that accessibility evaluations (manual or automated) pass for new UI flows.

## Governance

This constitution supersedes conflicting guidance. Amendments REQUIRE a written proposal, architecture review sign-off, and updated validation in plan/spec/tasks templates. Version numbers follow semantic versioning: MAJOR for principle changes or removals, MINOR for new principles or sections, PATCH for clarifications. Compliance reviews MUST occur each release cycle, with findings tracked in project issues and resolved before deployment. Runtime guidance updates MUST remain synchronized with this document.

**Version**: 2.0.0 | **Ratified**: 2025-10-25 | **Last Amended**: 2025-10-31
