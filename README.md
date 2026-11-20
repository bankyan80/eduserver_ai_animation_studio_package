# Eduserver AI Animation Studio — Implementation Package

**Version:** 1.0.0 (implementation baseline)  
**Target:** Eduserver.institute  
**Mode:** Local-first / no cloud AI API  
**Host:** HG680P (lightweight control plane)  
**GPU:** Separate local GPU worker for generation

## Package contents

- `01-PRD/PRD.md` — product requirements, scope, roles, workflows, acceptance criteria
- `02-Architecture/ARCHITECTURE.md` — architecture, services, data flow, hardware boundary
- `03-Implementation/ROADMAP.md` — phased implementation plan and definition of done
- `04-Database/SCHEMA.md` — entities and starter PostgreSQL schema
- `05-API/API.md` — internal REST API contract
- `06-UI/UI_SPEC.md` — screen and interaction specification
- `07-Deployment/DEPLOYMENT.md` — Docker, local network, Cloudflare Tunnel, storage
- `08-Testing/TEST_PLAN.md` — tests and release gates
- `09-Licensing/LICENSING.md` — repository/model license audit checklist
- `10-AI-Coding/AI_CODING_PROMPT.md` — copy-ready VS Code AI implementation instructions


## Non-negotiable design constraints

1. Do not run video diffusion models on HG680P (2 GB RAM).
2. HG680P hosts the portal integration, API, database, queue/control, metadata and storage management only.
3. ComfyUI and Ollama run on a separate capable local workstation/worker. GPU worker may be offline from the internet after models are installed.
4. No Gemini, OpenRouter, Google Flow, paid inference API, or cloud generation endpoint in local-only mode.
5. Keep the existing Eduserver authentication, UI conventions, database and GMP integration. Inspect the existing codebase before changing anything.
6. Treat every upstream repository as an optional reference or isolated service. Do not copy code until license and dependency review is completed.
7. Do not fabricate model availability or GPU capacity. Probe installed engines/workflows and show capability status.
8. All generation jobs must be queued, cancellable, retryable, observable and recoverable after restart.

## First action for the implementation agent

Audit the current Eduserver repository and report the actual framework, auth, DB, menu, media storage and GMP APIs. Do not scaffold a second Eduserver or replace existing components before this audit.
