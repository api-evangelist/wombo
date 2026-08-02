---
name: Generate a chat completion on w.ai
description: Pick a text/vision model on the w.ai network and generate a chat completion, optionally with tool calling or streaming.
api: openapi/wombo-wai-openapi.yml
operations: [listModels, createChatCompletion]
---

# Generate a chat completion on w.ai

The w.ai Inference API is OpenAI-compatible. Base URL `https://api.w.ai/v1`.
Authenticate with an API key (`Authorization: Bearer wsk-...`) issued at
https://app.w.ai/developers/keys .

## Steps

1. **Discover a model** — call `listModels` (`GET /v1/models`). Choose an `id`
   whose `input_modalities` include `text` (e.g. `llama-3.3-70b-4bit`), or one
   that also lists `image` for vision (e.g. `gemma-3-27b-4bit`). Confirm
   `tool_calling`/`streaming` in the model's `supported_features` if you need them.
2. **Create the completion** — call `createChatCompletion`
   (`POST /v1/chat/completions`) with `model` and a `messages[]` array. Roles are
   `system`, `user`, `assistant`, `tool`. For vision, pass a content array with an
   `image_url` part.
3. **(Optional) tools** — supply `tools[]` (function definitions) and
   `tool_choice` (`auto`/`required`/`none`/a specific function). When the model
   returns `tool_calls`, execute them and append a `tool` message with the result.
4. **(Optional) stream** — set `"stream": true` to receive server-sent events.

## Rules
- Errors use the OpenAI error object `{ "error": { message, type, code } }` (see
  errors/wombo-problem-types.yml). Handle `401` (bad key) and `429` (rate/balance).
- Requests are not idempotent — no idempotency key is supported.
