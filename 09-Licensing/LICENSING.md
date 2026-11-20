# Licensing and upstream audit checklist
This document is an initial review guide, not legal advice or a live verification of current upstream files. Before adopting code, inspect the exact commit/tag, LICENSE, dependencies, assets, model cards, workflow nodes and terms.

## Candidate repos
| Repository | Candidate license noted in prior review | Proposed use | Gate |
|---|---|---|---|
| OpenX Flow | MIT (verify) | Pipeline reference/adaptation | Check current license, provider dependencies and model assets |
| StoryBored | MIT (verify) | Storyboard workflow/reference | Verify commit, deps, integration boundaries |
| OpenDirector | Apache-2.0 (verify) | UI/workspace reference | Check notices and source modifications |
| AI Video Production Editor | GPL-3.0-or-later (verify) | Feature reference; avoid direct incorporation initially | Review obligations if code is copied/combined/distributed |
| Opsis | Unknown/unverified | Workflow reference only | Do not adopt code until license confirmed |
| ComfyUI | GPL-3.0 (verify) | Separate engine/service | Keep independent; comply with distribution requirements |
| Ollama | MIT (verify) | Local LLM service | Verify release and bundled dependencies |
| FFmpeg | LGPL/GPL depends on build | Media engine | Check build flags, codecs and redistribution |
## Model licensing
For each model, record name, exact version/hash, model-card URL, license, commercial-use status, attribution requirements, restrictions, quantization source, LoRA license and dataset notes. Model weights may have terms different from engine code.
## Required release files
- `THIRD_PARTY_LICENSES.md`
- `MODEL_CATALOG.md`
- `SBOM` (CycloneDX or SPDX)
- Attribution/notice bundle when required
- Review record with date, commit/hash, reviewer and decision
## Policy
No unknown-license code, models, audio, fonts, stock media or LoRA may be bundled for public/commercial distribution until reviewed. The studio should allow users to add local models while storing license metadata and warning on missing license.
