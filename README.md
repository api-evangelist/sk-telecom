# SK Telecom (sk-telecom)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
