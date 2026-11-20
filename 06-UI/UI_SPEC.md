# UI Specification
Use Eduserver design system, responsive behavior and current navigation. Do not redesign unrelated pages.

## Navigation
Add `AI Animation Studio` with icon `clapperboard` or `film` and permission-aware visibility.

## Screens
1. Studio dashboard: project cards/list, status, recent exports, create project.
2. New project wizard: lesson brief, style, duration, format, reference uploads.
3. Project workspace: top project title/status/save state; step navigation; central work area; right inspector.
4. Script editor: outline and narration tabs, regenerate section, version/approve.
5. Storyboard: scene cards, thumbnails, duration, reorder, select, approve.
6. Character/environment library: reusable assets and prompt details.
7. Prompt studio: prompt/negative prompt, camera, motion, model/workflow, seed, copy, save version.
8. Generation queue: queue, progress, worker, cancel/retry, logs panel.
9. Audio studio: narration per scene, voice preview where supported, captions, audio tracks.
10. Editor: preview, timeline, tracks for scenes, narration, music, subtitles; MVP trims/order/transitions.
11. Export: preset, resolution, aspect ratio, output name, progress, validation results.
12. GMP publish dialog: preview, metadata, attribution/license, explicit confirmation.
13. Admin settings: local engine URLs, worker, workflows, health, storage, retention, limits.

## Interaction rules
- Auto-save draft with visible `Saving`, `Saved`, or `Failed to save` status.
- Destructive operations require confirmation.
- Job operations show truthful state; never simulate progress.
- Empty, offline, error, loading and permission-denied states are designed.
- Generation button disabled until engine/workflow and required inputs are valid.
- Scene selection and job status should update without full page reload.
- Keyboard navigation, focus, contrast and responsive layout required.

## Suggested visual system
Dark neutral workspace; one Eduserver accent color; compact panels; generous preview area; consistent icons; avoid excessive cards. Teacher mode prioritizes guided actions; advanced mode reveals model, seed, steps, CFG, frame count and workflow options only when supported.
