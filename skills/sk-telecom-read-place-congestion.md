---
name: Read place and commercial-area congestion from SK Telecom Geovision Puzzle
description: Resolve a TMAP POI or commercial-area id, then read real-time congestion, hourly congestion statistics and daily visitor analytics derived from SK Telecom mobile network data.
api: openapi/sk-telecom-puzzle-place-congestion-openapi.json
base_url: https://apis.openapi.sk.com/puzzle/place
operations:
  - pois
  - areas
  - poicongestionrltm
  - poicongestionraw
  - poicongestionstat
  - areacongestionrltm
  - areacongestionraw
  - areacongestionstat
  - poivisitcount
  - poivisitdistance
  - poivisitseg
  - areavisitcount
  - areavisitdistance
  - areavisitseg
  - areavisittype
  - areabizcategory
generated: '2026-07-25'
method: generated
source: openapi/sk-telecom-puzzle-place-congestion-openapi.json + https://openapi.sk.com/products/detail?svcSeq=56
---

# Read place and commercial-area congestion from SK Telecom Geovision Puzzle

Geovision Puzzle estimates footfall and congestion from SK Telecom mobile network signalling.
Everything is keyed on one of two identifiers, and you must resolve the identifier first —
there is no free-text place search in this API.

## Before you start

- `appkey` request header — **lowercase** on the Puzzle APIs, unlike every other SK open API.
- The Free plan is quota-limited per API group per day (place list 100/day, real-time
  congestion 1,000/day, estimated visitor count 1,000/day as published on 2023-03-17). Budget
  your calls: there are no rate-limit headers to read.

## Step 1 — resolve the identifier

- `pois` (`GET /meta/pois`) — the places Puzzle can answer for, keyed on TMAP `poiId`.
- `areas` (`GET /meta/areas`) — the commercial areas (상권), keyed on `areaId`.

Cache these. They are the scarcest calls in your quota and they change slowly.

## Step 2 — read congestion

Every congestion operation exists in a POI form and an area form:

| Question | POI | Area |
|---|---|---|
| How busy is it right now? | `poicongestionrltm` | `areacongestionrltm` |
| Raw congestion by hour | `poicongestionraw` | `areacongestionraw` |
| Statistical (typical) congestion by hour | `poicongestionstat` | `areacongestionstat` |

Use the `stat` variants to draw a "usually busy at this hour" baseline and the `rltm` variant
for the live reading; the `raw` variants give you the observed hourly series to compare against.

## Step 3 — read visitor analytics

| Question | POI | Area |
|---|---|---|
| Visitors per day | `poivisitcount` | `areavisitcount` |
| Where visitors travel from (catchment) | `poivisitdistance` | `areavisitdistance` |
| Visitor age distribution | `poivisitseg` | `areavisitseg` |
| Visitor type distribution | — | `areavisittype` |
| Business-category mix (monthly) | — | `areabizcategory` |

`areavisittype` and `areabizcategory` are area-only — there is no POI equivalent, so a
place-level product needs to fall back to the enclosing area.

## Rules

- **All 16 operations are GET and side-effect free.** Cache aggressively; the statistical and
  monthly series do not change between calls within a day.
- **Errors.** Each operation declares 400, 401, 403, 404, 429, 500 and 504. `429` arrives as
  `THROTTLED` (per-second rate) or `QUOTA_EXCEEDED` (daily plan quota) inside
  `{"error":{"id","category","code","message"}}`. `QUOTA_EXCEEDED` will not clear by retrying
  in the same period.
- **403 `ACCESS_DENIED`** means your caller IP is not on the app's whitelist, not that the
  place is private.
- **Do not bulk-harvest.** SK Telecom cut the free tier tenfold in March 2023 specifically
  because callers were looping these endpoints to copy the dataset.
- **Companion API.** For apartment-complex resident analytics use the Puzzle Residence API
  (`apts`, `aptresidentage`, `aptresidentdemo`, `aptsubwayuserate`, `aptcategories`) on
  https://apis.openapi.sk.com/puzzle/residence, keyed on the Korean `kaptCode`.
