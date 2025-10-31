# SPEC.md — Standard Application Framework (Portable Edition)

This specification defines the **standard frameworks, structure, and rules** for all projects that I (and my AI coding agents) build.  
It ensures every project uses the same portable stack with consistent structure and style while avoiding provider lock-in.

---

## 1. Purpose


```yaml
---

## 2. Core Framework Stack

| Layer | Framework | Description | Hosting Notes |
|-------|-----------|-------------|---------------|
| **Frontend** | **Next.js (React)** | Builds the user interface and handles routing (SSR, SSG, SPA). | Deploy via static hosting or Node runtimes on any platform (Vercel, Netlify, Cloudflare, Kubernetes, custom VM). |
| **Styling** | **Tailwind CSS + shadcn/ui** | Provides an accessible, composable design system. | Works with any CSS pipeline; ship design tokens from `packages/shared`. |
| **Backend** | **FastAPI (Python)** | Async REST API for logic and data. | Package as OCI image; run on containers, serverless, or managed runtimes. |
| **Database** | **PostgreSQL** | Stores application data and metadata. | Use managed PostgreSQL or self-hosted container. |
| **Cache** | **Redis** | Accelerates reads and sessions. | Compatible with Redis OSS, Valkey, Dragonfly, or managed equivalents. |
| **Messaging** | **Vendor-neutral queue** | Handles background tasks and event messaging. | Recommended: Redis Streams, RabbitMQ, NATS JetStream, or Celery brokers. |
| **Storage** | **S3-compatible object storage** | Stores images, media, and files. | Use MinIO locally; deploy to S3, DigitalOcean Spaces, Backblaze B2, etc. |
| **Identity** | **OpenID Connect provider** | Authentication & single sign-on. | Works with Auth0, Okta, Entra ID, Keycloak, Cognito, etc. |
| **Monitoring** | **OpenTelemetry + OTLP collector** | Logs, metrics, tracing. | Export to Grafana Tempo, Honeycomb, New Relic, Datadog, etc. |

---

## 3. Folder Structure

All projects must follow this structure:

```
repo/
  apps/
    frontend/      # Next.js app
    api/           # FastAPI backend
  packages/
    shared/        # Shared code (types, clients)
  infra/
    ops/           # Provider-agnostic infrastructure templates
  spec/
    SPEC.md        # This spec file
```

---

## 4. Coding Standards

- **Languages:** Python ≥ 3.12, Node ≥ 20  
- **Linters/Formatters:**  
  - Python: `ruff`, `black`, `mypy`  
  - JS/TS: `eslint`, `prettier`, `typescript`  
- **Testing:**  
  - Backend: `pytest`  
  - Frontend: `Vitest` or `Jest`  
  - End-to-End: `Playwright`  
- **Accessibility:** Must meet **WCAG AA** standards.  
- **Commits:** Follow [Conventional Commits](https://www.conventionalcommits.org/).  
- **Containerization:** Provide multi-stage Dockerfiles for frontend and backend.  
- **Secrets:** Store in a managed secret manager (e.g., HashiCorp Vault, AWS Secrets Manager, Doppler). `.env` files are for local development only.

---

## 5. Deployment Standards

- **Infrastructure:** Defined with provider-agnostic IaC (Terraform, Pulumi, Crossplane, Ansible).  
- **CI/CD:** GitHub Actions with workflows for linting, testing, container builds, and infrastructure validation.  
- **Hosting:**  
  - Frontend → Static hosting or containerized Node runtime.  
  - API → Container platform (Kubernetes, ECS, Fly.io, Azure Container Apps, etc.).  
- **Data:** PostgreSQL + Redis deployed via managed services or self-hosted containers.  
- **Security:** Standards-based OIDC login; infrastructure policies reviewed for least privilege.  
- **Monitoring:** OpenTelemetry exporters sending OTLP data to the selected observability backend.

---

## 6. Local Development Setup

Use **Docker Compose** to run local versions of services:

```yaml
services:
  postgres:
    image: postgres:16
    ports: ["5432:5432"]
    environment:
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: devpass123
    volumes:
      - postgres-data:/var/lib/postgresql/data
  redis:
    image: redis:7
    ports: ["6379:6379"]
  minio:
    image: minio/minio
    command: server /data --console-address ":9001"
    environment:
      MINIO_ROOT_USER: dev
      MINIO_ROOT_PASSWORD: devpass123
    ports: ["9000:9000", "9001:9001"]
    volumes:
      - minio-data:/data
  queue:
    image: rabbitmq:3-management
    ports: ["5672:5672", "15672:15672"]

volumes:
  postgres-data:
  minio-data:
```
```

### Local run commands
- **API:** `uvicorn app.main:app --reload --port 8000`  
- **Frontend:** `pnpm dev`  
- **Test:** `pytest`, `pnpm test`, `npx playwright test`

---

## 7. Spec Compliance Rules

Every repository that follows this spec must:

1. Include this `SPEC.md` (or sync it from the `framework-spec` repo).  
2. Use **Next.js + Tailwind/shadcn/ui + FastAPI + provider-agnostic IaC**.  
3. Pass the **validate-stack** workflow before merging pull requests.  
4. Avoid alternative web frameworks (e.g., Flask, Express, Django) without written approval.  
5. Document any provider-specific services together with portability fallbacks.

---

## 8. Validation Workflow (Summary)

Each project will use this GitHub Action:
- Syncs `SPEC.md` from `framework-spec`.  
- Runs checks for:
  - `apps/frontend/package.json` includes `"next"` and `"tailwind"`.  
  - `apps/api/pyproject.toml` includes `"fastapi"`.  
  - Provider-agnostic infrastructure files exist (`infra/ops`, Terraform, Pulumi, or docker-compose).  
  - No Azure-only Bicep templates are committed.  
- Fails the pull request if the project breaks the rules.

---

## 9. Versioning

Projects should note the spec version they’re built on:
```
spec-version: 2.0.0
```
When `framework-spec` updates, projects can pull the new version or stay pinned to their current one.

---

## 10. Summary

✅ This `SPEC.md` defines:  
- The **exact frameworks** (Next.js + Tailwind/shadcn/ui + FastAPI)  
- The **portable infrastructure** expectations  
- The **folder layout and coding rules**  
- The **GitHub validation workflow** that enforces portability  

All coding agents and collaborators should start each project from a template that references this spec.

---

**Author:** Mat  
**Repository:** `framework-spec`  
**Version:** 2.0.0  
**Last updated:** 2025-10-31
