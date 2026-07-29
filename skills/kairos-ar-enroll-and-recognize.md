---
name: Enroll faces and recognize them against a gallery
description: Build a face gallery by enrolling subjects, then identify unknown photos against it.
api: openapi/kairos-ar-openapi.yml
operations: [enroll, recognize, viewGallery]
---

# Enroll and recognize faces with Kairos

Use this to build a gallery of known people and then identify an unknown photo against it.

## Auth
Send both headers on every request (see `authentication/kairos-ar-authentication.yml`):

```
app_id: YOUR_APP_ID
app_key: YOUR_APP_KEY
Content-Type: application/json
```

## Steps

1. **Enroll each known person** — call `enroll` (POST `/enroll`) with `image`
   (public URL, Base64, or file upload), a `subject_id` you choose, and a
   `gallery_name`. Enrolling 6–8 photos per subject improves accuracy. A first
   enroll of a new `gallery_name` creates the gallery.
2. **Confirm the gallery** — call `viewGallery` (POST `/gallery/view`) with the
   `gallery_name` to list enrolled `subject_ids`.
3. **Recognize an unknown photo** — call `recognize` (POST `/recognize`) with
   the `image` and the `gallery_name`. Read `candidates[]` and their
   `confidence`; values above 0.60 are almost always the same person. Tune
   `threshold` (default 0.70) and `max_num_results` (default 10) as needed.

## Error handling
Face-recognition errors come back with HTTP 200 and an `Errors[]` envelope
(`Message`, `ErrCode`). Handle: `5002` no faces found, `5004` gallery not found,
`5012` no match found. See `errors/kairos-ar-error-codes.yml`.

## Notes
Enrolling biometric templates is a consequential write — capture consent and log
the operation. There is no idempotency key; de-duplicate on your own
`subject_id`/`gallery_name`.
