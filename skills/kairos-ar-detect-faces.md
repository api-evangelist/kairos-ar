---
name: Detect faces and facial attributes in a photo
description: Find faces and return coordinates plus attributes without enrolling anything.
api: openapi/kairos-ar-openapi.yml
operations: [detect]
---

# Detect faces with Kairos

Use this to find faces and their attributes in a photo without enrolling or
matching against any gallery.

## Auth
Send `app_id` + `app_key` headers and `Content-Type: application/json`.

## Steps

1. **Detect** — call `detect` (POST `/detect`) with the `image` (public URL or
   Base64). Optionally set `selector` (`FRONTAL` default, `FULL`, `PARTIAL`,
   `ROLL`) to tune the detector for pose, and `minHeadScale` (0.015–0.5) for the
   smallest face to look for.
2. **Read the result** — each entry in `images[].faces[]` returns coordinates
   (`topLeftX/Y`, eye/chin points, `yaw`/`pitch`/`roll`), a `confidence`, and an
   `attributes` block (age, gender, ethnicity confidences, glasses, lips).
   Coordinates originate at the top-left of the image.

## Error handling
`5002` no faces found is returned with HTTP 200 in an `Errors[]` envelope. See
`errors/kairos-ar-error-codes.yml`.
