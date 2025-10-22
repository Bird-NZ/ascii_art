```markdown
# framework-spec

Spec for agentic coding — a concise specification describing the architecture, responsibilities, interfaces and behaviours expected from agentic coding frameworks and components.

## Table of contents

- [Overview](#overview)
- [Principles](#principles)
- [Specification surface](#specification-surface)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Overview

This repository defines a minimal, implementation‑agnostic specification for "agentic coding" systems — systems that use autonomous or semi-autonomous agents to write, review, and modify code. The goal is to provide a stable set of expectations and interfaces so different agent implementations can interoperate and so projects can evaluate or adopt agentic tools with predictable behaviour.

The spec focuses on:
- Agent lifecycle and capabilities
- Communication and messaging formats
- Task and goal representation
- Observability and safety controls
- Integration hooks for repositories and CI systems

## Principles

- Language-agnostic: keep definitions independent of any single programming language.
- Minimal and actionable: specify only what is necessary for interoperability and evaluation.
- Safe-by-default: include guidance for permissions, rate limits, and human-in-the-loop controls.
- Extendable: allow implementors to add features while remaining compatible with the core spec.

## Specification surface

The repository organizes the specification around these core concepts:

- Agent
  - Identity, capabilities, authorization scopes
  - Lifecycle: propose → plan → act → report → retire
- Task / Goal
  - Standardized task structure (title, description, inputs, acceptance criteria, priority)
  - Task states and transitions
- Messaging
  - Standard message types (request, progress, result, exception)
  - Suggested JSON schema for messages and events
- Repositories & Integrations
  - Hooks for cloning, patch creation, and PR metadata
  - Recommended patterns for CI gating and human approvals
- Observability & Safety
  - Event logging model, assertions, and audit trails
  - Permissioning and escalation patterns

(For full JSON schemas, message examples and governance guidelines, see the corresponding spec files in this repository.)

## Examples

- Example task message (JSON) and recommended PR metadata are included under the `examples/` directory.
- Example agent lifecycle flows and suggested webhooks are in `docs/flows.md`.

## Contributing

Contributions are welcome. If you'd like to propose a change:
1. Open an issue describing the rationale and proposed change.
2. Submit a PR that updates the relevant spec documents and includes example(s) and tests where applicable.
3. Aim for small, reviewable changes. Include cross-references to other spec parts when changing semantics.

See CONTRIBUTING.md (if present) for more detail.

## License

This project is released under the MIT License. See LICENSE for details.

## Contact

Maintained by Bird-NZ. For questions or discussions, open issues on this repository or reach out via GitHub.
```
