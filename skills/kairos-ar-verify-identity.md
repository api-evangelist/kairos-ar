---
name: Verify a photo against a known subject
description: One-to-one check that a photo matches a specific enrolled subject.
api: openapi/kairos-ar-openapi.yml
operations: [verify]
---

# Verify an identity with Kairos

Use this for a 1:1 check — does this photo match a specific person you already
enrolled?

## Auth
Send `app_id` + `app_key` headers and `Content-Type: application/json` on the
request (see `authentication/kairos-ar-authentication.yml`).

## Steps

1. **Verify** — call `verify` (POST `/verify`) with the `image`, the
   `gallery_name`, and the `subject_id` you want to check against. The image may
   be a public URL, Base64, or a file upload.
2. **Read the result** — a `status` of `success` means a comparison was made.
   Use the `confidence` value to decide validity; above 0.60 is almost always
   the same person.

## Error handling
Errors return HTTP 200 with an `Errors[]` envelope. Handle: `5003` subject ID
not found, `5004` gallery not found, `5002` no faces found, `5010` too many
faces in image (verify expects a single face), `5012` no match. See
`errors/kairos-ar-error-codes.yml`.
