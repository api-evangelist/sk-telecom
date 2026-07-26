# SK Telecom (sk-telecom)

SK Telecom Co., Ltd. is South Korea's largest mobile network operator and the telecom arm of SK Group, headquartered at SK T-Tower in Jung-gu, Seoul. It runs the country's largest 5G and LTE network, sells fixed broadband and IPTV through subsidiary SK Broadband, and has repositioned itself as an "AI company" around its A.X sovereign Korean LLM family, the NUGU voice assistant, and the A. (A dot) assistant.

SK Telecom is one of the few mobile network operators that actually runs a real, self-serve public developer portal — but it is not a network-API portal. **SK open API** at [openapi.sk.com](https://openapi.sk.com/) is operated by SK Telecom Co., Ltd., has open registration, a live AWS-fronted gateway at `https://apis.openapi.sk.com/`, and a public catalog of 26 products. What it publishes is AI, big-data and mobility APIs — speech synthesis, face recognition, congestion and floating-population analytics derived from mobile-network signalling, video analysis, road-event alerting. There is no Number Verification, no SIM Swap, no device location, no QoD, no carrier billing, no SMS and no IoT connectivity-management API on it.

The legacy "T developers" programme is dead. `developer.sktelecom.com` and `developers.sktelecom.com` both still resolve in DNS to `211.188.149.18` and refuse connections on port 443. `api.sktelecom.com` and `opengateway.sktelecom.com` do not answer at all.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/sk-telecom/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/sk-telecom/refs/heads/main/apis.yml)

## CAMARA posture

SK Telecom is a **listed CAMARA participant** (two named representatives in the CAMARA Governance participant roster; listed in the CAMARA landscape under Operation / Operators; present in SimSwap, NumberVerification and OTPValidation sync-call minutes) and GSMA maintains an Open Gateway organisation page for it. It has three commercial arrangements to bring network APIs to market:

- **TTA three-operator MoU** (28 Oct 2024) — SK Telecom, KT and LG Uplus agreed to standardise six common network APIs for Korea through the Telecommunications Technology Association: five identity / mobile-financial-security APIs including Number Verification and SIM Swap, plus Quality on Demand.
- **Bridge Alliance API Exchange (BAEx)** — SK Telecom is one of 13 endorsing operators of the CAMARA-aligned APAC exchange running on Singtel's Paragon platform, surfaced to developers through Nokia's Network as Code portal.
- **Aduna / SK telink MoU** (8 Sep 2025) — a strategic commercial partnership prioritising Number Verification, SIM Swap and KYC.

**Not one CAMARA endpoint is callable from anything SK Telecom operates.** All three of the above are commitments, standardisation items or aggregator listings — a press release is not an implementation. Developers who want SK Telecom network capability reach it through Bridge Alliance or Aduna, not through SK Telecom.

No TM Forum Open API conformance certification was found. No public NEF/SCEF, network-slicing or edge/MEC API is documented. CIBA does not appear anywhere in SK Telecom's public surface.

## Auth

API key in an `appKey` request header (the Puzzle APIs use lowercase `appkey`), issued from the SK open API dashboard. The gateway is AWS API Gateway and returns HTTP 401 `UnauthorizedException` without a key. No OAuth2/OIDC discovery document is served on `openapi.sk.com` (HTTP 404) or `developers.t-id.co.kr` (returns the SPA shell).

## Harvested API definitions

Six real OpenAPI 3.1.0 documents, saved verbatim in [`openapi/`](openapi/). SK Telecom publishes no "Download OpenAPI" button; the definitions live in ReadMe projects (`*-skopenapi.readme.io`) and are downloadable anonymously from the ReadMe API registry. Provenance for each is recorded in [`review.yml`](review.yml).

| API | Paths | Server |
| --- | --- | --- |
| A.X TTS | 3 | `https://apis.openapi.sk.com/axtts` |
| A. facecan (NUGU facecan) | 9 | `https://apis.openapi.sk.com/nugufacecan` |
| Puzzle Place Congestion | 16 | `https://apis.openapi.sk.com/puzzle/place` |
| Puzzle Residence | 5 | `https://apis.openapi.sk.com/puzzle/residence` |
| META | 7 | `https://apis.openapi.sk.com/sigmeta` |
| OVS | 10 | `https://apis.openapi.sk.com/api/ovs` |

TMAP (32 paths) and Urbanbase (8 paths) are also served from SK Telecom's gateway with real OpenAPI, but belong to TMAP Mobility Co., Ltd. and Urbanbase Inc. respectively and are not attributed to SK Telecom here.

## Tags

- Telecommunications
- South Korea
- Mobile Network Operator
- Network APIs
- CAMARA
- Open Gateway
- 5G
- Identity Verification
- SIM Swap
- Artificial Intelligence
- Location
- Big Data

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## Links

- Website — https://www.sktelecom.com/
- Developer portal (SK open API) — https://openapi.sk.com/
- Sign up — https://openapi.sk.com/user/signUp
- Newsroom — https://news.sktelecom.com/
- Open source — https://sktelecom.github.io/en/ · https://github.com/sktelecom
- Models — https://huggingface.co/skt
- Partner programme (T Open API via Open2U) — https://open2u.sktelecom.com/web/WWO01N051X01.do
- Enterprise — https://www.sktenterprise.com/
- Support — skopenapi@sktelecom.com
