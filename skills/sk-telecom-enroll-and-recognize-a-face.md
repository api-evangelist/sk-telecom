---
name: Enroll and recognize a face with SK Telecom A. facecan
description: Build a per-group, per-subject face database on A. facecan and then recognize a submitted image against it — plus stateless detect and landmark inference.
api: openapi/sk-telecom-facecan-openapi.json
base_url: https://apis.openapi.sk.com/nugufacecan
operations:
  - group-create
  - group-list
  - subject-create
  - subject-list
  - face-create
  - subject-list-1
  - face-recognize-1
  - face-detect-1
  - face-landmark-1
  - face-delete
  - subject-delete
  - group-delete
generated: '2026-07-25'
method: generated
source: openapi/sk-telecom-facecan-openapi.json + https://nugufacecan-skopenapi.readme.io/reference/error-codes
---

# Enroll and recognize a face with SK Telecom A. facecan

A. facecan (published as "NUGU facecan") stores a face database of `app → group → subject →
face` and answers recognition queries against a chosen group. This skill processes biometric
data — treat every step as a regulated operation and confirm you have a lawful basis and the
subject's consent before enrolling anyone.

## Before you start

- `appKey` header on every call (from https://openapi.sk.com/mypage/project/).
- `app-id` header on every call — the application identifier.
- Input images: JPG/JPEG, 320x240 to 3840x2160, under 2 MB. Images travel as a multipart form
  field named `image` even though the spec models the body as `application/json` with
  `format: binary`.

## Step 1 — create a group (`group-create`)

`POST /v1/group` with a `group-name` header (no Korean characters). Returns `group_id` and
`group_name`. `group-list` (`GET /v1/group`) enumerates existing groups for the app — call it
first so you do not hit error 1011 `group already exists`.

## Step 2 — create a subject (`subject-create`)

`POST /v1/subject` with `group-id` and `subject-name` headers. `subject-name` is alphanumeric
and must be unique inside the (app, group) pair — a duplicate returns error 2004
`subject-name must be unique`. Use a stable internal id (employee number, member id), not a
display name. Returns `subject_id`.

## Step 3 — enroll faces (`face-create`)

`POST /v1/face` with `group-id`, `subject-id` and `face-name` headers plus the image.

- Enroll several poses per subject and name them (`front`, `left`, `right`) — `face-name` is
  how you distinguish them later.
- Re-enrolling a false-negative image under a name like `false_negative` improves recall.
- `allow-mask` (any value) lets you enroll masked or low-score faces that would otherwise be
  rejected with 3010 `mask in face` or 3007 `inappropriate face in image`.
- To send encrypted images: generate an AES-128 key client-side, encrypt it with the public key
  from the service's `/v1/key` endpoint, and pass it in the `key` header.

`subject-list-1` (`GET /v1/face`) lists enrolled faces for a subject, with box, landmarks,
face score, expression, estimated age, gender, attribute and engine version.

## Step 4 — recognize (`face-recognize-1`)

`POST /v1/recognize` with `group-id` and the query image.

- `multi: 0` (default) returns the single best match; `multi: 1` returns a result per face.
- `threshold` (default 0.32) tunes precision against recall — lower is stricter.
- `request-id` is echoed back on the response; set it, and log it. It is the only correlation
  id anywhere in this API.
- `use-spoof` (any value) turns on anti-spoofing; presentation attacks then fail with 3040
  `face spoofing attack detected`, and faces under 96x96 with 3041 `small face detected`.

`3005 matching faces do not exist` is the normal "no match" answer, returned as HTTP 400 — do
not treat it as a transport error.

## Stateless inference

- `face-detect-1` (`POST /v1/detect`) — face boxes only, nothing persisted.
- `face-landmark-1` (`POST /v1/landmark`) — landmark points only, nothing persisted.

Prefer these when you only need geometry; they never touch the face database.

## Cleanup

`face-delete` (`DELETE /v1/face/{face_id}`), `subject-delete`
(`DELETE /v1/subject/{subject_id}`), `group-delete` (`DELETE /v1/group/{group_id}`). Deleting
enrolled biometrics on request is a compliance obligation, not a nice-to-have — wire it into
your own data-subject-request path.

## Rules

- **No idempotency.** A retried `face-create` enrolls a second face record for the same
  subject. Check `subject-list-1` before retrying rather than blindly repeating the write.
- **Two error envelopes.** The gateway returns `{"error":{"id","category","code","message"}}`;
  A. facecan itself returns `{"message":"...","code":<int>}` with the 1xxx/2xxx/3xxx/4xxx
  families catalogued in `errors/sk-telecom-error-codes.yml`.
- **503 codes 3050/3051/3052/3060/3061/3062 are GPU-capacity errors**, not client errors —
  back off and retry those; do not retry 4xxx image errors.
