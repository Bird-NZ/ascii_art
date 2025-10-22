# SPEC.md — Standard Application Framework (Azure Edition)

This specification defines the **standard frameworks, structure, and rules** for all projects that I (and my AI coding agents) build.  
It ensures every project uses the same reliable, Azure-based stack with consistent structure and style.

---

## 1. Purpose

To keep all my projects consistent, modern, and deployable to **Microsoft Azure** — while allowing fast, creative development using Python and JavaScript.

---

## 2. Core Framework Stack

| Layer | Framework | Description | Azure Service |
|-------|------------|--------------|----------------|
| **Frontend** | **Next.js (React)** | Builds the user interface and handles routing (SSR, SSG, SPA). | **Azure Static Web Apps** or **Azure App Service (Node)** |
| **Styling** | **Tailwind CSS + shadcn/ui** | Provides a modern, accessible design system. | — |
| **Backend** | **FastAPI (Python)** | Async REST API for logic and data. | **Azure App Service / Container Apps / AKS** |
| **Database** | **PostgreSQL** | Stores app data. | **Azure Database for PostgreSQL (Flexible Server)** |
| **Cache** | **Redis** | Speeds up reads and sessions. | **Azure Cache for Redis** |
| **Messaging** | **Azure Service Bus** | Handles background tasks and event messaging. | **Service Bus** |
| **Storage** | **Blob Storage** | Stores images, media, and files. | **Azure Blob Storage + CDN / Front Door** |
| **Identity** | **Microsoft Entra ID (Azure AD)** | Auth & single sign-on. | **Azure Entra ID** |
| **Monitoring** | **OpenTelemetry + Application Insights** | Logs, metrics, tracing. | **Azure Monitor / App Insights** |

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
    bicep/         # Azure infrastructure templates
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
- **Secrets:** Never stored locally — always in **Azure Key Vault**.  
- **Config:** Use `.env` for local dev only.

---

## 5. Deployment Standards

- **Infrastructure:** Written in **Azure Bicep**.  
- **CI/CD:** GitHub Actions or Azure Pipelines.  
- **Hosting:**  
  - Frontend → Azure Static Web Apps  
  - API → Azure App Service / Container Apps  
- **Data:** Azure PostgreSQL + Redis  
- **Security:** Azure Entra ID (OIDC).  
- **Monitoring:** Azure Application Insights + Log Analytics.

---

## 6. Local Development Setup

Use **Docker Compose** to run local versions of services:

```yaml
services:
  postgres:
    image: postgres:16
    ports: ["5432:5432"]
  redis:
    image: redis:7
    ports: ["6379:6379"]
  azurite:
    image: mcr.microsoft.com/azure-storage/azurite
    ports: ["10000:10000", "10001:10001"]
```

### Local run commands
- **API:** `uvicorn app.main:app --reload --port 8000`  
- **Frontend:** `pnpm dev`  
- **Test:** `pytest`, `pnpm test`, `npx playwright test`

---

## 7. Spec Compliance Rules

Every repository that follows this spec must:

1. Include this `SPEC.md` (or sync it from the `framework-spec` repo).  
2. Use **Next.js + Tailwind/shadcn/ui + FastAPI + Azure Bicep**.  
3. Pass the **validate-stack** workflow before merging pull requests.  
4. Never include other web frameworks (e.g., Flask, Express, Django).  
5. Use Azure as the only cloud provider.  

---

## 8. Validation Workflow (Summary)

Each project will use this GitHub Action:
- Syncs `SPEC.md` from `framework-spec`.  
- Runs checks for:
  - `apps/frontend/package.json` includes `"next"` and `"tailwind"`.  
  - `apps/api/pyproject.toml` includes `"fastapi"`.  
  - `infra/bicep/` folder exists.  
- Fails the pull request if the project breaks the rules.

---

## 9. Versioning

Projects should note the spec version they’re built on:
```
spec-version: 1.0.0
```
When `framework-spec` updates, projects can pull the new version or stay pinned to their current one.

---

## 10. Summary

✅ This `SPEC.md` defines:  
- The **exact frameworks** (Next.js + Tailwind/shadcn/ui + FastAPI)  
- The **Azure services** you must use  
- The **folder layout and coding rules**  
- The **GitHub validation workflow** to enforce it  

All your coding agents and collaborators should start each project from a template that references this spec.

---

**Author:** Mat  
**Repository:** `framework-spec`  
**Version:** 1.0.0  
**Last updated:** 2025-10-17
