# Test Plan
## Unit
- Project validation and permissions.
- Script/storyboard JSON schema validation.
- Scene duration calculations and reorder invariants.
- Prompt template substitutions and versioning.
- Job state transition validation.
- Asset path normalization and upload validation.
- FFmpeg command construction with safe arguments.
## Integration
- Existing Eduserver auth and roles.
- DB migrations/rollback.
- Ollama health, timeout and malformed response.
- ComfyUI workflow submission, poll, output collection, cancel.
- Worker heartbeat and offline handling.
- Piper TTS output.
- FFmpeg/FFprobe render validation.
- GMP adapter using staging/test data.
## End-to-end
1. Create project and brief.
2. Generate script locally; edit and approve.
3. Generate storyboard; reorder scene; approve.
4. Generate prompt and reference image.
5. Enqueue video job; verify progress/output and cancellation.
6. Generate narration/subtitle.
7. Render MP4.
8. Verify MP4 stream, duration, audio, subtitle.
9. Publish to GMP; verify metadata and media preview.
10. Confirm project can reopen and assets resolve.
## Resilience
- Ollama unavailable; ComfyUI unavailable; worker disconnected.
- Worker restart mid-job; API restart with queued jobs.
- Disk near full; invalid output; render timeout.
- Duplicate request/idempotency; cancel after completion.
- Retry limit reached; clean temp files.
## Security
- Cross-user IDOR tests for project, scene, job, asset and export.
- Upload path traversal, oversized file, MIME spoofing.
- SSRF attempts against engine configuration.
- Secret leakage in logs/errors.
- Unauthorized publish and admin settings.
## Release gates
- No P0/P1 defect.
- Existing Eduserver smoke tests pass.
- All local-only tests confirm no external inference call.
- Export file passes FFprobe validation.
- Backup/restore documented and tested.
- License inventory completed for bundled code, models, workflows and assets.
