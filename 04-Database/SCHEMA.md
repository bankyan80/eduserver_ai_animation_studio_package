# Data model (starter)
Reuse Eduserver DB and user table. The following is a logical schema; adapt names, types and migration framework to the existing repository.


## Tables
- `animation_projects`: id, owner_id, title, description, subject, grade_level, topic, objectives_json, duration_seconds, language, visual_style, aspect_ratio, status, created_at, updated_at, deleted_at.
- `animation_scripts`: id, project_id, version, outline_json, narration_text, notes, approved_at, created_at.
- `animation_scenes`: id, project_id, sort_order, title, duration_seconds, location_text, character_ids_json, narration, dialogue, action_description, camera_direction, transition, prompt, negative_prompt, status, approved_at, created_at, updated_at.
- `animation_characters`: id, project_id nullable (global library optional), name, description, style, reference_asset_id, metadata_json, created_at.
- `animation_environments`: id, project_id nullable, name, description, reference_asset_id, metadata_json, created_at.
- `animation_prompts`: id, scene_id, prompt_type, prompt_text, negative_prompt, model_key, workflow_key, version, created_at.
- `animation_assets`: id, project_id, scene_id nullable, asset_type, relative_path, mime_type, size_bytes, sha256, metadata_json, license_note, created_at.
- `animation_jobs`: id, project_id, scene_id nullable, job_type, status, priority, worker_id, workflow_key, input_json, output_asset_ids_json, progress, attempt_count, max_attempts, error_code, error_message_redacted, created_at, started_at, finished_at, updated_at.
- `animation_workers`: id, name, endpoint_private, status, capabilities_json, heartbeat_at, version, created_at.
- `animation_exports`: id, project_id, asset_id, preset, status, metadata_json, created_at.
- `animation_settings`: key, value_json, updated_at, updated_by.


## SQL starter (PostgreSQL)

```sql
CREATE TABLE animation_projects (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  owner_id TEXT NOT NULL,
  title TEXT NOT NULL,
  description TEXT,
  subject TEXT,
  grade_level TEXT,
  topic TEXT,
  objectives_json JSONB NOT NULL DEFAULT '[]',
  duration_seconds INTEGER,
  language TEXT NOT NULL DEFAULT 'id-ID',
  visual_style TEXT,
  aspect_ratio TEXT NOT NULL DEFAULT '16:9',
  status TEXT NOT NULL DEFAULT 'draft',
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  deleted_at TIMESTAMPTZ
);

CREATE INDEX animation_projects_owner_updated_idx
  ON animation_projects(owner_id, updated_at DESC);


CREATE TABLE animation_scenes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID NOT NULL REFERENCES animation_projects(id) ON DELETE CASCADE,
  sort_order INTEGER NOT NULL,
  title TEXT NOT NULL,
  duration_seconds NUMERIC(6,2) NOT NULL DEFAULT 5,
  location_text TEXT,
  character_ids_json JSONB NOT NULL DEFAULT '[]',
  narration TEXT,
  dialogue TEXT,
  action_description TEXT,
  camera_direction TEXT,
  transition TEXT,
  prompt TEXT,
  negative_prompt TEXT,
  status TEXT NOT NULL DEFAULT 'draft',
  approved_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  UNIQUE(project_id, sort_order)
);

CREATE TABLE animation_jobs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id UUID NOT NULL REFERENCES animation_projects(id) ON DELETE CASCADE,
  scene_id UUID REFERENCES animation_scenes(id) ON DELETE SET NULL,
  job_type TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'queued',
  priority INTEGER NOT NULL DEFAULT 0,
  worker_id TEXT,
  workflow_key TEXT,
  input_json JSONB NOT NULL DEFAULT '{}',
  output_asset_ids_json JSONB NOT NULL DEFAULT '[]',
  progress NUMERIC(5,2) NOT NULL DEFAULT 0,
  attempt_count INTEGER NOT NULL DEFAULT 0,
  max_attempts INTEGER NOT NULL DEFAULT 3,
  error_code TEXT,
  error_message_redacted TEXT,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  started_at TIMESTAMPTZ,
  finished_at TIMESTAMPTZ,
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE INDEX animation_jobs_queue_idx ON animation_jobs(status, priority DESC, created_at);
```


## Requirements
- Use the existing migration tool.
- Enforce project ownership in server-side queries, not only UI.
- Add indexes for owner, project, job status and created timestamps.
- Use soft-delete for projects and retention policy for large assets.
- Keep asset file metadata and physical files consistent; use cleanup jobs for orphaned files.
