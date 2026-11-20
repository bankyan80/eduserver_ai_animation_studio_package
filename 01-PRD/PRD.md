# PRD — Eduserver AI Animation Studio

## 1. Product
AI Animation Studio is a full local-first educational animation production workspace inside Eduserver.institute. It turns a teacher's topic or supplied lesson material into a script, storyboard, visual references, scene prompts, locally generated images/video/audio, a rendered MP4, and optionally a published GMP media entry.

## 2. Goals
- Simplify production so teachers need not understand prompt engineering or model workflows.
- Provide a guided pipeline from lesson brief to exportable media.
- Use local LLM and local generation engines only in local-only mode.
- Keep HG680P resource usage low and offload GPU work to a separate workstation.
- Reuse Eduserver auth, navigation, storage conventions and GMP publication flow.
- Save projects and intermediate assets so generation can resume or be revised.

## 3. Non-goals
- Cloud inference or SaaS video generation.
- Training foundation models.
- Running diffusion/video generation on HG680P.
- Automatic public publishing without user approval.
- Guaranteeing factual correctness of generated educational content without teacher review.
- Replacing a professional NLE for advanced compositing.

## 4. Users and permissions
- Super Admin: global settings, worker/engine configuration, job monitoring, all projects.
- Teacher/Operator: create and manage own projects, review scripts/scenes, run generation, export.
- Viewer/Public: view or download only published GMP media according to GMP policy.
Use Eduserver's existing identity and role system; map roles rather than create separate accounts.

## 5. Core workflow
1. Create project.
2. Enter lesson topic, grade/level, subject, learning objectives, language, style, target duration, aspect ratio and optional reference files.
3. Generate outline and script locally with LLM; edit and approve.
4. Generate scene breakdown/storyboard; edit, reorder, approve.
5. Create/choose character and environment reference assets.
6. Generate scene prompts and negative prompts; edit and approve.
7. Select available local workflow/model; enqueue image/video jobs.
8. Review results; regenerate selected scenes.
9. Generate narration, captions/subtitles and audio assets locally.
10. Assemble/render via FFmpeg; preview; perform QC checklist.
11. Export MP4/SRT/project archive or publish to GMP after confirmation.

## 6. Functional requirements
### Project dashboard
- Create, rename, duplicate, archive, delete (soft-delete by default).
- Search/filter by title, subject, level, state and modified date.
- Project states: draft, scripting, storyboard, assets, generation, editing, review, exported, published, failed.
### Lesson brief
- Fields: title, subject, grade/level, topic, objectives, key facts, target duration, language, visual style, aspect ratio, audience notes, source material.
- Accept text and local file references. Validate file type/size and do not execute uploaded files.
### Script writer
- Generate outline, narration, dialogue, scene transitions, glossary and teacher notes.
- Editable revisions; preserve approved version.
- Use a structured response schema; validate; retry safely; never claim source verification unless sources were actually supplied/checked.
### Storyboard
- Scene number, title, duration, location, characters, narration, dialogue, visual action, camera, transition, prompt, negative prompt, references, status.
- Add/delete/reorder scenes; scene durations adjustable; total duration displayed.
- Generate thumbnails/references; regenerate one scene only.
### Character & environment library
- Store prompt description, reference images, tags, project/global scope and usage notes.
- Reuse approved references; avoid identity/consistency guarantees unsupported by model.
### Prompt studio
- Prompt templates by style and task; Indonesian/English variants; scene context; camera; motion; lighting; negative prompt.
- Editable before generation; version history.
### Generation queue
- Queue jobs, concurrency limits, progress, logs, ETA only if engine reports/estimate is calibrated, cancel, retry, priority.
- Worker heartbeat and capability discovery.
- Persist jobs and recover stale jobs after server restart.
### Audio
- Local TTS, voice selection if installed, narration generation per scene, subtitles, audio asset import, mixing levels.
- Mark missing voice models clearly; do not fake successful generation.
### Editor / assembly
- Scene order, clips, trim, transitions, title cards, narration, background audio, subtitles, watermark/brand.
- MVP editor is timeline-based assembly with FFmpeg; advanced effects are out of scope.
### Export
- MP4 H.264, selectable 720p/1080p if hardware supports; aspect ratio 16:9, 9:16, 1:1.
- SRT, project archive, optionally separate audio.
- Render progress, cancellation, error logs and output validation.
### GMP
- Publish only after explicit confirmation.
- Use existing GMP API/data model discovered during code audit.
- Add subject, grade, topic, duration, thumbnail, tags, attribution/license and project creator metadata as available.
- If GMP integration API is absent, create an adapter and document contract; do not write directly to unknown tables.
### Settings / health
- Local LLM endpoint, ComfyUI endpoint, FFmpeg availability, worker status, model/workflow list, disk space, queue, GPU/VRAM when reported.
- Local-only switch blocks any external inference endpoints.

## 7. UX principles
- Guided wizard for beginners; advanced controls expandable.
- Save drafts automatically; show explicit save state.
- Clear stage progression and approval gates.
- No generation starts without a selected local workflow and valid engine.
- Show warning when model/workflow unavailable or resource constraints detected.
- Do not expose raw ComfyUI node graphs to normal users; admin/advanced view may expose workflow details.

## 8. Acceptance criteria
- Project can be created, persisted and reopened.
- Script generation works through configured local LLM only; no cloud fallback.
- Scene breakdown is editable/reorderable and persists.
- Worker reports capabilities and heartbeat.
- Generation jobs can queue, cancel, retry, survive backend restart and link outputs to scenes.
- Audio and video assembly can use local tools only.
- Export file is validated (exists, nonzero, FFprobe-readable, expected stream present).
- GMP publish is explicit, role-protected and uses existing integration.
- Access control prevents cross-user project access.
- UI remains usable if worker/LLM/video engine is offline.
- Logs contain no secrets; project deletion and asset retention behavior are documented.

## 9. MVP boundary
MVP includes project, brief, local script, storyboard, prompt generation, ComfyUI adapter, queue, asset library, audio TTS integration, FFmpeg assembly, export and GMP publishing adapter. Advanced multi-track editing, collaborative realtime editing, training, marketplace and cloud APIs are post-MVP.
