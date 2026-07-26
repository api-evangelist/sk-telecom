---
name: Analyze video and images with SK Telecom META
description: Drive META's asynchronous analyzer lifecycle — create a job, upload to the presigned URL, poll for the result — for golf swing and pose estimation, or call the synchronous end-to-end and licence-plate operations.
api: openapi/sk-telecom-meta-openapi.json
base_url: https://apis.openapi.sk.com/sigmeta
operations:
  - create_analyzer
  - get_result
  - e2e
  - create-analyzer
  - start_analyzer
  - e2e-1
  - lpr
generated: '2026-07-25'
method: generated
source: openapi/sk-telecom-meta-openapi.json + https://meta-skopenapi.readme.io/reference/error-code-2
---

# Analyze video and images with SK Telecom META

META is SK Telecom's Vision AI platform (service name `sigmeta`). It offers two shapes for the
same work: an asynchronous job lifecycle for large media, and a synchronous end-to-end call for
small media.

## Before you start

`appKey` request header on every call. HTTPS only.

## Asynchronous lifecycle (large media)

Golf swing analysis and pose estimation each expose their own pair of operations. The
operationIds are inconsistent between the two products — use them exactly as written:

| Product | Create job | Poll result |
|---|---|---|
| Golf Swing Analyzer | `create_analyzer` (`POST /golf/v1/create_analyzer`) | `get_result` (`GET /golf/v1/get_result/{JobID}`) |
| Pose Estimation | `create-analyzer` (`POST /pose/v1/create_analyzer`) | `start_analyzer` (`GET /pose/v1/get_result/{JobID}`) |

Note that Pose Estimation's *poll* operation carries the operationId `start_analyzer`. That is
what the published spec says; do not "correct" it in generated code.

1. **Create.** The create call returns a `JobID` and a temporary presigned S3 upload URL.
2. **Upload.** PUT/POST the media to that presigned URL with the `key`, `policy`,
   `x-amz-security-token` and `x-amz-signature` parameters the create call handed you. A
   missing or wrong one of these is a `403 Forbidden`; a missing `key` is a `400`.
3. **Poll.** Call the result operation with the `JobID` until the JSON result comes back. A
   `204 No Content` means the file is missing or the upload has only just succeeded — keep
   polling rather than treating it as failure.

Presigned URLs are temporary. If the upload window expires, create a new job — you cannot
re-use the `JobID`.

## Synchronous operations (small media)

- `e2e` (`POST /golf/v1/e2e`) and `e2e-1` (`POST /pose/v1/e2e`) — upload and analyze in one
  call, returning JSON. Hard limit **3 MB**; over it you get `413 Request Entity Too Large`.
- `lpr` (`POST /lpr/v1`) — licence plate recognition, returning the plate coordinates and the
  vehicle number as JSON. Treat plate numbers as personal data.

## Rules

- **Video constraints.** Clips shorter than 4 seconds and unsupported container/extension types
  fail with `500 Internal Server Error`, not a 4xx — read the message, not just the status.
- **`503 Service Unavailable`** means unsupported extension, test load, or an internal error;
  retry with backoff.
- **No idempotency.** A retried `create_analyzer` creates a second job and a second billed
  analysis. Store the `JobID` before you retry anything.
- **No webhooks.** There is no completion callback anywhere in SK open API — polling
  `get_result` / `start_analyzer` is the only completion signal.
