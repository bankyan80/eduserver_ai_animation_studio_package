# Deployment and Local Setup

## Topology
- HG680P Armbian: existing Eduserver host, API/control, DB, queue, metadata, project files and proxy/tunnel.
- GPU workstation: Ollama, ComfyUI, generation worker, TTS and FFmpeg render worker.
- Storage: project HDD on HG680P; model weights on GPU workstation SSD.

## Preflight
- Confirm Eduserver repo, current deployment, DB, auth, media/GMP contract.
- Check HG680P disk, memory and Docker support.
- Check GPU worker OS, GPU, driver, VRAM, RAM, free SSD.
- Choose and license-check one LLM, one image workflow and one video workflow before downloading models.
- Confirm private network reachability between worker and API.

## Docker guidance
Use existing compose files and conventions. Add services only if needed. Suggested control-plane services: API module, optional Redis, reverse proxy (existing), database (reuse existing). GPU worker compose runs separately and contains worker, Ollama, ComfyUI and audio/render dependencies. Avoid privileged containers; mount only required project/model paths; pin image versions; health checks; restart policy; CPU/RAM limits for HG680P services.

## Local-only configuration example
```env
AI_MODE=local_only
LLM_PROVIDER=ollama
LLM_BASE_URL=http://gpu-worker.lan:11434
COMFYUI_BASE_URL=http://gpu-worker.lan:8188
WORKER_SHARED_SECRET=replace-with-secret
GENERATION_CONCURRENCY=1
VIDEO_JOB_TIMEOUT_SECONDS=3600
PROJECT_STORAGE=/mnt/media/eduserver-animation
MODEL_STORAGE=/mnt/worker-models
EXTERNAL_INFERENCE_ALLOWED=false
```
Do not commit secrets. Use secret manager or protected env files. Validate endpoints against private-network allowlist.

## Security network rules
- Never expose Ollama or ComfyUI directly to public internet.
- Worker endpoints restricted to LAN/VPN and authenticated.
- Cloudflare Tunnel only exposes the Eduserver web/API routes needed.
- TLS where traffic crosses untrusted networks.
- Limit upload MIME, size, and file path; use generated UUID names; reject path traversal.
- Render commands built from validated parameters, not arbitrary command text.

## Storage
- Mount HDD path with stable UUID.
- Keep models on GPU worker.
- Set per-user/project quotas and temporary-file cleanup.
- Check free space before generation/render.
- Backup database and project metadata; define whether large assets are backed up.

## Operations
- Monitor API, worker heartbeat, queue length, disk space, engine availability and failed jobs.
- Logs rotated; redact tokens, prompts containing private data when policy requires.
- Document upgrade/rollback and restore test.
