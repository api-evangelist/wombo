---
name: Detect and segment objects on w.ai
description: Run YOLO11n object detection or SAM2 segmentation over an image or video on the w.ai network.
api: openapi/wombo-wai-openapi.yml
operations: [listModels, createPrediction]
---

# Detect and segment objects on w.ai

Base URL `https://api.w.ai/v1`; bearer `wsk-` key.

## Steps

1. **Confirm the model** — `yolo11n` (object detection) and `sam2`
   (segmentation) are available; call `listModels` to confirm availability.
2. **Run a prediction** — call `createPrediction` (`POST /v1/predictions`) with
   `model` and an `input` object.
   - **YOLO11n**: `input.image` (URL or base64) plus optional `conf`, `iou`,
     `imgsz`, `return_json` (true = JSON detections, false = annotated image).
   - **SAM2 (image)**: `input.image` plus `points` (`[{x, y, label}]`, label
     1=foreground/0=background) or `boxes`; control with `mask_type`,
     `annotation_type`.
   - **SAM2 (video)**: `input.video` plus `points`, `video_fps`,
     `output_frame_interval`, `output_format`.

## Rules
- Exactly one of `input.image` or `input.video` is required.
- Set `return_json` to control JSON vs annotated-media output.
- Handle `400`/`401` per errors/wombo-problem-types.yml.
