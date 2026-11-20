# API Contract (draft)
Prefix: `/api/animation/v1` or adapt to Eduserver API conventions. Auth inherited from Eduserver.

## Projects
- `GET /projects` list own projects (filters: status, subject, grade, q)
- `POST /projects` create
- `GET /projects/{projectId}` detail
- `PATCH /projects/{projectId}` update
- `DELETE /projects/{projectId}` soft-delete
- `POST /projects/{projectId}/duplicate`
## Script / storyboard
- `POST /projects/{projectId}/script/generate` accepts brief, generation options; returns job id or structured draft
- `GET /projects/{projectId}/script`
- `PUT /projects/{projectId}/script` save/edit version
- `POST /projects/{projectId}/storyboard/generate`
- `GET /projects/{projectId}/scenes`
- `POST /projects/{projectId}/scenes`
- `PATCH /scenes/{sceneId}` edit
- `DELETE /scenes/{sceneId}` delete
- `POST /projects/{projectId}/scenes/reorder` ordered scene IDs
- `POST /scenes/{sceneId}/prompt/generate`
## Assets and generation
- `GET /projects/{projectId}/assets`
- `POST /projects/{projectId}/assets/upload` multipart, validated
- `POST /scenes/{sceneId}/generate` enqueue image/video generation
- `GET /jobs/{jobId}` status, progress and safe logs
- `POST /jobs/{jobId}/cancel`
- `POST /jobs/{jobId}/retry`
## Audio, render and export
- `POST /projects/{projectId}/audio/narration/generate`
- `POST /projects/{projectId}/subtitles/generate`
- `POST /projects/{projectId}/render` validated render preset and timeline
- `GET /projects/{projectId}/exports`
- `POST /exports/{exportId}/validate`
## GMP
- `POST /projects/{projectId}/publish` explicit confirmation and metadata
- `POST /projects/{projectId}/unpublish` only if GMP supports it
## Health and settings (admin)
- `GET /health/engines` returns configured local engines, versions, reachability, capability summary; never leaks credentials
- `GET /workers` list worker status/capabilities
- `POST /settings/engine` admin-only, local endpoint validation
- `POST /workers/register` authenticated worker registration
- `POST /workers/heartbeat` worker status/capability heartbeat

## Common response
```json
{
  "ok": true,
  "data": {},
  "error": null,
  "request_id": "opaque-id"
}
```
Errors include stable `code`, human-readable `message`, optional safe `details`, and `request_id`. Do not expose stack traces, tokens, filesystem paths outside permitted scope or raw workflow credentials.

## Job state machine
`queued -> processing -> completed | failed | cancelled`
Transient failures may transition to `retryable`, then back to `queued`. Transitions are server-validated. Progress 0-100 is informational and may be unknown. Persist before dispatch; cancellation is best effort and must be reported honestly.
