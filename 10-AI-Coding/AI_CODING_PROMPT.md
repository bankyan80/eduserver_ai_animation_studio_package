# VS Code AI Coding Agent — Master Implementation Prompt

You are the senior engineer implementing **Eduserver AI Animation Studio** inside the existing Eduserver.institute repository.

## Goal
Implement the approved PRD incrementally. This is a full local-only educational animation production studio: lesson brief → script → storyboard → characters/environment → prompts → local image/video generation → local narration/subtitles → FFmpeg assembly → export → optional GMP publication.

## Mandatory first step: audit, do not code blindly
Inspect the repository and report:
- framework, routes, package manager and commands;
- authentication and roles;
- database, migration tool and existing schema;
- storage/media handling;
- GMP integration contract;
- deployment/hosting assumptions;
- UI components and style conventions;
- test baseline and known failures.
Do not replace existing auth, DB, UI, GMP or deployment. Do not create a parallel app if the feature can be integrated cleanly.

## Hardware boundary
HG680P has Armbian, Docker, 2 GB RAM and external HDD. It is a lightweight control-plane host only. Do not run Ollama model inference, ComfyUI diffusion/video, or heavy rendering on it. These run on a separately configured local GPU workstation/worker. No cloud AI API or provider fallback is allowed.

## Local-only policy
- AI_MODE must be `local_only` by default.
- Allowed configured engines: local Ollama, private ComfyUI worker, local TTS, FFmpeg.
- Reject external inference URLs and cloud providers.
- Never silently send project prompts/files to external services.
- Report engine unavailable instead of simulating successful generation.

## Engineering rules
- Follow existing repository conventions and lint/format rules.
- Type-safe request/response contracts; validate structured AI output.
- Server-side ownership and role checks for every resource.
- Use migrations; never drop/overwrite user data.
- Persistent job state, idempotency, cancel, retry, timeouts, heartbeat and safe recovery.
- Safe file handling: UUID paths, MIME/size checks, path traversal prevention, quota and cleanup.
- Never execute arbitrary prompts as shell commands or arbitrary workflow files supplied by users.
- Render commands use validated argument lists, not shell string interpolation.
- Redact secrets and private endpoint credentials from logs.
- Add tests for each feature and run them. Report commands and results truthfully.
- Build UI empty/loading/offline/error/success states. No fake progress.
- Keep changes focused; do not refactor unrelated code.

## Phased execution
### Phase 0
Audit and present a short integration plan, blockers, and files likely to change. Wait for approval only if there is a major irreversible choice or the repository has conflicting architecture; otherwise proceed after audit.
### Phase 1
Project CRUD, role integration, project wizard, DB migration, asset/storage abstractions, dashboard, health/settings page. Test and summarize.
### Phase 2
Ollama adapter, script/outline generation, validated schema, storyboard generation/edit/reorder/approval, prompt templates. Local-only test.
### Phase 3
Worker registration, heartbeat, queue, ComfyUI adapter, image workflow, asset linking, cancel/retry/recovery. No model download by default.
### Phase 4
One validated video workflow, image-to-video/text-to-video, capability/resource checks, scene-level regeneration.
### Phase 5
Local TTS, subtitle editor, FFmpeg timeline assembly, render/export, FFprobe validation.
### Phase 6
GMP adapter based on actual existing contract; explicit publication confirmation; staging tests.
### Phase 7
Security, license inventory, backup/restore, responsive/accessibility, deployment docs, release gate.

## UI requirements
Create a guided teacher workflow and an advanced mode. Main screens: dashboard, project wizard, script editor, storyboard, character/environment library, prompt studio, generation queue, audio studio, timeline editor, export, GMP publish and admin settings. Use existing Eduserver design tokens/navigation.

## Deliverables for each phase
1. Summary of implemented behavior.
2. Files changed.
3. Migration notes.
4. Tests run and actual outcomes.
5. Known limitations and next phase.
6. Commands to run locally.

Start now with Phase 0 repository audit. Do not claim the audit or tests are complete until you actually perform them.
