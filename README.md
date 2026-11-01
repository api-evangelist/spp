# Southwest Power Pool

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

Southwest Power Pool (SPP) is a nonprofit Regional Transmission Organization regulated by the Federal Energy Regulatory Commission and headquartered in Little Rock, Arkansas. Founded in 1941 and approved as an RTO in 2004, SPP operates the Integrated Marketplace day-ahead and real-time balancing markets, the Western Energy Imbalance Service (WEIS) market, Western Reliability Coordination services, and is building the Markets+ day-ahead market for the West with a targeted 2027 go-live. SPP sits at the wholesale layer of the United States energy value chain: it dispatches generation, prices congestion, plans transmission, and settles the market for its member utilities — it has no retail customers, so no consumer energy-data mandate such as Green Button applies to it. Its API posture is a clean split. Market and grid data is genuinely open: locational marginal prices, market clearing prices, operating reserves, load, generation mix, VER curtailments and outage capacity are served anonymously as CSV from the SPP Portal file browser and from an anonymous public FTP site, with an Esri ArcGIS REST price-contour service alongside — no account, no key, no licence click-through. Everything a market participant actually transacts against is the opposite: the Integrated Marketplace SOAP web services, the Settlement Management System API, and the FERC Order 881 LEP/TROLIE ratings API are member-only, require an OATI webCARES x.509 digital certificate plus an SPP UAA role, and SPP publishes no base URL and no OpenAPI for them.

**Areas:** Energy · United States · Energy Markets · Electricity · Grid · Utilities · Renewables · Market Data · Transmission · System Operator

## Southwest Power Pool on APIs.io

Everything profiled for Southwest Power Pool is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/spp/](https://apis.io/providers/spp/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [SPP Public Data FTP](https://apis.io/apis/spp/spp-public-data-ftp/) — SPP's officially documented programmatic interface to its public data. The SPP Public Data Access guide (v3.0, July 2023) names FTP as the programmatic…
- [SPP Portal Public Data File Browser API](https://apis.io/apis/spp/spp-portal-file-browser-api/) — The HTTP download service behind the SPP Portal public data pages. Anonymous GET requests of the form /file-browser-api/download/{dataset}?path={file}…
- [SPP Portal Chart API](https://apis.io/apis/spp/spp-portal-chart-api/) — JSON and CSV chart feeds serving the SPP Portal dashboards. Confirmed anonymously on 2026-07-27: /chart-api/gen-mix/asChart and…
- [SPP Price Contour Map ArcGIS REST Services](https://apis.io/apis/spp/spp-price-contour-map-arcgis/) — An Esri ArcGIS Server REST directory serving the geospatial layers behind SPP's price contour map. Confirmed anonymously on 2026-07-27 at ArcGIS 11.5: the…
- [SPP Integrated Marketplace Markets Web Service](https://apis.io/apis/spp/spp-integrated-marketplace-web-services/) — The SOAP web and notification services market participants use to submit offers and bids and retrieve results from the SPP Integrated Marketplace, with WEIS…
- [Southwest Power Pool Forecasting API](https://apis.io/apis/spp/spp-forecasting-api/) — The Forecasting API operations define the exchange of forecasted ratings, including the 240-hour-ahead forecasted AAR data exchange required by FERC Order…
- [Southwest Power Pool Monitoring Sets API](https://apis.io/apis/spp/spp-monitoring-sets-api/) — [Monitoring Sets](https://trolie.energy/concepts#monitoring-sets) are named sets of power system resources that may be used to filter ratings and limits…
- [Southwest Power Pool Seasonal API](https://apis.io/apis/spp/spp-seasonal-api/) — Seasonal ratings are static ratings associated with extended durations, typically months. These are typically used in both planning and operations. Power…
- [Southwest Power Pool Seasonal Overrides API](https://apis.io/apis/spp/spp-seasonal-overrides-api/) — A Seasonal Override instructs the system to use a temporary static rating instead of any concurrent Seasonal Rating for a resource. A typical use case is a…
- [Southwest Power Pool Temporary AAR Exceptions API](https://apis.io/apis/spp/spp-temporary-aar-exceptions-api/) — A [Temporary AAR Exception](https://trolie.energy/concepts#temporary-aar-exception) is provided by the Ratings Provider when a Ratings Obligation cannot be…
- [Southwest Power Pool Realtime API](https://apis.io/apis/spp/spp-realtime-api/) — If permitted by the Clearinghouse Provider, its Ratings Providers can use these functions to exchange real-time ratings, supplementing or replacing…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Examples (3)

Example requests and responses.

- [Spp Arcgis Rest Info](https://apis.io/examples/spp/spp-arcgis-rest-info/)
- [Spp Arcgis Rtbm Lmp Mapserver](https://apis.io/examples/spp/spp-arcgis-rtbm-lmp-mapserver/)
- [Spp Chart Api Gen Mix](https://apis.io/examples/spp/spp-chart-api-gen-mix/)

#### Collections (1)

Postman and Open Collection request collections.

- [Transmission Ratings and Operating Limits Information Exchange (TROLIE)](https://apis.io/collections/spp/open-trolie-standard/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Spp Trolie Standard Overlay](https://apis.io/overlays/spp/spp-trolie-standard-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Spp Authentication](https://apis.io/security/spp/spp-authentication/)
- [Spp Domain Security](https://apis.io/security/spp/spp-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Spp Scopes](https://apis.io/scopes/spp/spp-scopes/)

#### Rate limits (1)

Published request limits.

- [Spp Rate Limits](https://apis.io/rate-limits/spp/spp-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.spp.org/)
- [Documentation](https://www.spp.org/stakeholder-center/user-guides-apis-integrations/)
- [Getting started](https://spp.org/documents/28853/spp%20public%20data%20access%2020230707.pdf)
- [Sign up](https://www.spp.org/account/create-account/)
- [Support](https://rms.spp.org/)
- [Blog](https://www.spp.org/newsroom/)
- [Compliance](https://www.spp.org/stakeholder-center/spp-rto-compliance/)
- [Terms of service](https://www.spp.org/terms-conditions/)
- [Privacy policy](https://www.spp.org/terms-conditions/)

## API Evangelist rating

**Kin Score 45.2** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 57.1 |
| Contract quality | 56.9 |
| Contract governance | 4.5 |
| Operational transparency | 5.3 |
| Developer ergonomics | 62.5 |
| Access clarity | 42.1 |

**Agent readiness 43.0** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 38.5** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/spp/](https://apis.io/providers/spp/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Southwest Power Pool: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/spp/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
