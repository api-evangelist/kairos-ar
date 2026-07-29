---
name: Analyze emotion and attention in media
description: Submit an image or video and retrieve per-frame emotion, demographics and attention, plus aggregated impressions.
api: openapi/kairos-ar-openapi.yml
operations: [createMedia, getMedia, getAnalytics]
---

# Analyze emotion with Kairos

Use this to run the Emotion Analysis (v2) pipeline over an image or video.

## Auth
Send `app_id` + `app_key` headers on every request.

## Steps

1. **Submit media** — call `createMedia` (POST `/v2/media`) with the `source`
   query parameter (media URL or file upload). Optionally set `landmarks=1` to
   receive facial feature points and `timeout` (default 10s, max 60s). A `202`
   response with `status_code: 2` means processing is still in progress.
2. **Fetch results** — call `getMedia` (GET `/v2/media/{id}`) with the returned
   `id`. Read `frames[].people[]` for per-person `emotions`, `demographics`,
   `appearance`, `pose`, and `tracking` (attention/dwell/glances).
3. **Get aggregate impressions** — call `getAnalytics` (GET
   `/v2/analytics/{id}`) for rolled-up `average_emotion`, `emotion_score`
   (positive/neutral/negative), demographics and tracking across the media.
4. **Clean up (optional)** — call `deleteMedia` (DELETE `/v2/media/{id}`) to
   remove processed results.

## Error handling
Emotion endpoints use a flat `{ code, message }` or
`{ status_code, status_message }` object rather than the face-recognition
`Errors[]` envelope. `status_code: 3` means the media record was not found. See
`errors/kairos-ar-error-codes.yml`.
