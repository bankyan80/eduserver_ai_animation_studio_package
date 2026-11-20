# Implementation Roadmap

## Phase 0 — Repository audit (mandatory)
1. Inspect repository root, framework, package manager, build commands and environment.
2. Identify authentication, roles, existing DB schema/migrations, storage, GMP model/API, navigation and design system.
3. Record current deployment (Vercel/host/HG680P) and constraints.
4. Create audit report and integration plan. No replacement of existing systems.
**Done when:** audit report reviewed; extension points identified; baseline tests/build pass.

## Phase 1 — Foundation
- Add Animation Studio navigation using existing role checks.
- Project CRUD, DB migrations, storage service, validation.
- Project dashboard and project wizard.
- Local-only settings and health page.
**Done when:** project lifecycle persists, permissions tested, no broken existing pages.

## Phase 2 — Script + storyboard
- Ollama adapter, model configuration, JSON schema output validation.
- Brief-to-outline/script generation.
- Scene breakdown/storyboard CRUD, reorder, approval.
- Prompt templates, prompt revision history.
**Done when:** local-only generation and manual editing work with Ollama disconnected graceful error.

## Phase 3 — Worker and image generation
- Worker registration/heartbeat/capability discovery.
- Persistent queue and job state machine.
- ComfyUI adapter; first supported image workflow.
- Scene reference image generation and asset management.
**Done when:** worker can generate an image, return asset and recover from restart.

## Phase 4 — Video generation
- Add supported local video workflow(s), beginning with one validated workflow/model.
- Scene image-to-video and text-to-video; configurable resolution/frames/seed.
- Cancel, retry, progress and resource checks.
**Done when:** one scene can be generated, reviewed and regenerated reproducibly within hardware constraints.

## Phase 5 — Audio and assembly
- Piper integration, narration per scene, subtitle generation/edit.
- FFmpeg render presets, transitions, title/credits, audio mix.
- FFprobe validation and render logs.
**Done when:** a complete project renders MP4/SRT/audio locally.

## Phase 6 — GMP publication
- Implement adapter against actual GMP contract.
- Metadata mapping, thumbnail, explicit publish confirmation, permissions.
- Draft/published state and rollback/unpublish if GMP supports it.
**Done when:** successful publish and failure handling verified in staging.

## Phase 7 — Production readiness
- Security review, license inventory, backup/restore, quota and cleanup.
- Accessibility, responsive UI, load tests, multiuser authorization.
- Deployment and operator documentation.
**Done when:** release checklist complete and no P0/P1 defects.

## Execution rules for coding agents
- Work in small phases; do not implement all phases in one giant change.
- Before edits, produce audit summary and plan.
- Preserve Eduserver conventions and UI.
- Keep migrations reversible; do not delete user data.
- Add tests with each feature.
- Stop and ask only if a real blocker or incompatible architectural choice is discovered.
