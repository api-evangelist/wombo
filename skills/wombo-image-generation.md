---
name: Generate and edit images on w.ai
description: Generate an image from a text prompt with FLUX/SDXL on w.ai, then optionally edit it with a FLUX Kontext model.
api: openapi/wombo-wai-openapi.yml
operations: [listModels, createImage, createImageEdit]
---

# Generate and edit images on w.ai

OpenAI-compatible Images API. Base URL `https://api.w.ai/v1`; bearer `wsk-` key.

## Steps

1. **Pick an image model** — call `listModels` and choose an `id` whose
   `output_modalities` include `image` (e.g. `flux-1-dev`, `sdxl`,
   `z-image-turbo-8bit`).
2. **Generate** — call `createImage` (`POST /v1/images/generations`) with `model`
   and `prompt`. Tune with `size` (default `1024x1024`), `quality`, `seed`,
   `steps` (1-100), `guidance_scale` (1-20), and `negative_prompt`. Set
   `"stream": true` for progress updates.
3. **Edit (optional)** — call `createImageEdit` (`POST /v1/images/edits`) as
   `multipart/form-data` with a Kontext model (e.g. `flux-1-kontext-dev`), the
   edit `prompt`, and the source `image` file(s).

## Rules
- Image edits are multipart/form-data, not JSON.
- Use `seed` for reproducible generations.
- Handle `400` (bad model/params) and `401` per errors/wombo-problem-types.yml.
