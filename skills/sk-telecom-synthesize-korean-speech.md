---
name: Synthesize Korean speech with SK Telecom A.X TTS
description: Pick an A.X TTS model, list the voices it supports, and synthesize Korean speech to a wav/pcm/opus/mp3 stream — or to timestamped segments.
api: openapi/sk-telecom-ax-tts-openapi.json
base_url: https://apis.openapi.sk.com/axtts
operations:
  - get-voices
  - text-to-speech-tts
  - text-to-speech-with-timestamp
generated: '2026-07-25'
method: generated
source: openapi/sk-telecom-ax-tts-openapi.json + https://ax-tts-skopenapi.readme.io/reference/getting-started-with-ax-tts-api
---

# Synthesize Korean speech with SK Telecom A.X TTS

A.X TTS is SK Telecom's own DNN speech-synthesis service, run by SKT's speech synthesis
technology team. Three operations, no state, no SDK — raw HTTPS with a header key.

## Before you start

- Get an `appKey` from https://openapi.sk.com/mypage/project/ (마이페이지 > 앱 > 앱키). There is
  no test key and no sandbox host: every call is a live, metered call.
- Send it as the `appKey` request header on every call. HTTPS only — port 80 has been closed
  since 2026-07-01 and there is no redirect.

## Step 1 — choose a model

Valid `model` values, taken from the spec enum: `axtts-2-6` (default), `axtts-2-6-ainews`,
`axtts-2-1`, `axtts-2-1-dialect`.

## Step 2 — list the voices that model supports (`get-voices`)

`GET /voice?model=axtts-2-6` with the `appKey` header. `model` is a required query parameter.
Use the returned speaker names as the `voice` value in step 3 — do not guess a voice name.

## Step 3 — synthesize (`text-to-speech-tts`)

`POST /tts` with a JSON body. Required fields: `model`, `voice`, `text`.

- `text` must be under 300 characters. Split longer copy client-side and concatenate the audio.
- `speed` is a string in the range 0.5–2.0 (default "1").
- `sr` is one of 8000, 16000, 22050 (default 22050).
- `sformat` is one of `wav`, `pcm`, `opus`, `mp3` (default `wav`).
- **Set `accept` to match `sformat`** — `wav` → `audio/wav`, `pcm` → `application/octet-stream`,
  `opus` → `audio/ogg`, `mp3` → `audio/mpeg`. Mismatching them is the most common failure.
- `clientId` is a free-text service name; set it so your traffic is attributable.

The response body is the audio stream, not JSON.

## Step 4 (optional) — synthesize with timestamps (`text-to-speech-with-timestamp`)

`POST /segmentation` takes the same body but `accept: application/json` and returns segment
timing alongside the audio. Use it when you need to align captions or drive a viseme/avatar.

## Rules

- **No retries without care.** A.X TTS has no idempotency key and no request de-duplication.
  A retried `POST /tts` is a second billed synthesis, not a replay.
- **Errors.** The gateway answers `401 UNAUTHORIZED` with no key, `403 INVALID_API_KEY` with a
  bad key, `403 ACCESS_DENIED` if your caller IP is not on the app's whitelist, `429 THROTTLED`
  when the per-second rate is exceeded and `429 QUOTA_EXCEEDED` when the plan quota is spent.
  The envelope is `{"error":{"id","category","code","message"}}` — not RFC 9457. Back off on
  429; do not treat `QUOTA_EXCEEDED` as retryable within the same period.
- **No rate-limit headers exist.** You cannot read remaining quota from a response; track it
  yourself.
