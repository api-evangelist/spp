# Southwest Power Pool (spp)

Southwest Power Pool (SPP) is a nonprofit Regional Transmission Organization regulated by the Federal Energy Regulatory Commission and headquartered in Little Rock, Arkansas. Founded in 1941 and approved as an RTO in 2004, SPP operates the Integrated Marketplace day-ahead and real-time balancing markets, the Western Energy Imbalance Service (WEIS) market, Western Reliability Coordination services, and is building the Markets+ day-ahead market for the West with a targeted 2027 go-live. SPP sits at the wholesale layer of the United States energy value chain: it dispatches generation, prices congestion, plans transmission, and settles the market for its member utilities — it has no retail customers, so no consumer energy-data mandate such as Green Button applies to it. Its API posture is a clean split. Market and grid data is genuinely open: locational marginal prices, market clearing prices, operating reserves, load, generation mix, VER curtailments and outage capacity are served anonymously as CSV from the SPP Portal file browser and from an anonymous public FTP site, with an Esri ArcGIS REST price-contour service alongside — no account, no key, no licence click-through. Everything a market participant actually transacts against is the opposite: the Integrated Marketplace SOAP web services, the Settlement Management System API, and the FERC Order 881 LEP/TROLIE ratings API are member-only, require an OATI webCARES x.509 digital certificate plus an SPP UAA role, and SPP publishes no base URL and no OpenAPI for them.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/spp/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/spp/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Energy Markets
- Electricity
- Grid
- Utilities
- Renewables
- Market Data
- Transmission
- System Operator

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### SPP Public Data FTP

SPP's officially documented programmatic interface to its public data. The SPP Public Data Access guide (v3.0, July 2023) names FTP as the programmatic access path for Integrated Marketplace, Western Reliability Coordination and WEIS public data. Verified anonymously on 2026-07-27: a listing returned FTP status 226 with top-level directories including Markets, Operational_Data, Operations_Reports, Settlements, Reliability, TCR, Market_Monitoring and West_EIS. No account, key or licence agreement is required.

- **Human URL:** [https://spp.org/documents/28853/spp%20public%20data%20access%2020230707.pdf](https://spp.org/documents/28853/spp%20public%20data%20access%2020230707.pdf)
- **Base URL:** `ftp://pubftp.spp.org`

#### Tags

- Public Data
- Market Data
- FTP

#### Properties

- [Documentation](https://spp.org/documents/28853/spp%20public%20data%20access%2020230707.pdf)
- [Documentation](https://www.spp.org/stakeholder-center/user-guides-apis-integrations/)

### SPP Portal Public Data File Browser API

The HTTP download service behind the SPP Portal public data pages. Anonymous GET requests of the form `/file-browser-api/download/{dataset}?path={file}` return CSV for real-time balancing market LMP by settlement location, real-time and day-ahead market clearing prices, operating reserves, hourly load, historical generation mix, VER curtailments, capacity of generation on outage, and the WEIS market equivalents. Eight dataset paths were confirmed HTTP 200 with content-type `text/csv` on 2026-07-27 with no credentials. SPP documents the datasets and file formats in its Markets Public Data Guide but does not publish this HTTP path as a supported interface specification — FTP is the interface SPP formally supports.

- **Human URL:** [https://portal.spp.org/pages/rtbm-lmp-by-location](https://portal.spp.org/pages/rtbm-lmp-by-location)
- **Base URL:** `https://portal.spp.org/file-browser-api/download`

#### Tags

- Public Data
- Market Data
- LMP
- Load
- Generation Mix

#### Properties

- [Documentation](https://portal.spp.org/pages/rtbm-lmp-by-location)
- [Documentation](https://www.spp.org/documents/19541/spp%20markets%20public%20data%20guide.pdf)

### SPP Portal Chart API

JSON and CSV chart feeds serving the SPP Portal dashboards. Confirmed anonymously on 2026-07-27: `/chart-api/gen-mix/asChart` and `/chart-api/load-forecast/asChart` return `application/json` with labelled hourly datasets by fuel type and load forecast series, and `/chart-api/gen-mix-365/asFile` returns a rolling 365-day generation mix CSV. Undocumented as a formal interface specification; served without authentication.

- **Human URL:** [https://portal.spp.org/pages/generation-mix-historical](https://portal.spp.org/pages/generation-mix-historical)
- **Base URL:** `https://portal.spp.org/chart-api`

#### Tags

- Public Data
- Generation Mix
- Load Forecast

#### Properties

- [Documentation](https://portal.spp.org/pages/generation-mix-historical)

### SPP Price Contour Map ArcGIS REST Services

An Esri ArcGIS Server REST directory serving the geospatial layers behind SPP's price contour map. Confirmed anonymously on 2026-07-27 at ArcGIS 11.5: the service root returned JSON listing the PCM and Utilities folders, and the PCM folder listed DA_Features, DA_LMP, DELTA_Features, DELTA_LMP, RTBM_Features, RTBM_LMP, States and Transmission MapServer services. Follows the standard Esri ArcGIS REST API contract (`?f=json`), not an SPP-authored specification.

- **Human URL:** [https://pricecontourmap.spp.org/arcgis/rest/services](https://pricecontourmap.spp.org/arcgis/rest/services)
- **Base URL:** `https://pricecontourmap.spp.org/arcgis/rest/services`

#### Tags

- Geospatial
- Market Data
- LMP
- ArcGIS

#### Properties

- [API Reference](https://pricecontourmap.spp.org/arcgis/rest/services?f=json)

### SPP LEP/TROLIE Ratings API

SPP's REST implementation of GE Vernova's Limit Exchange Portal (LEP) using the LF Energy TROLIE (Transmission Ratings and Operating Limits Information Exchange) API specification, built to meet FERC Order 881 ambient-adjusted ratings obligations. SPP published a versioned Data Exchange Guide (v1.0, 8 Nov 2024) covering monitoring sets, forecast ratings, real-time ratings and seasonal ratings. No production base URL is published — SPP's own guide uses the placeholder `https://<baseURL>/` throughout — and access requires an x.509 digital certificate from a CA SPP trusts plus a role assigned in SPP UAA. Not verifiable anonymously.

- **Human URL:** [https://www.spp.org/documents/72496/spp%20lep%20api%20data%20exchange%20guide.pdf](https://www.spp.org/documents/72496/spp%20lep%20api%20data%20exchange%20guide.pdf)

#### Tags

- Transmission
- Ratings
- TROLIE
- FERC Order 881
- Gated

#### Properties

- [Documentation](https://www.spp.org/documents/72496/spp%20lep%20api%20data%20exchange%20guide.pdf)
- [OpenAPI](openapi/trolie-standard-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) — the LF Energy TROLIE community specification that SPP's LEP API implements. Published by trolie.energy, **not** by SPP.
- [Specification](https://trolie.energy/spec)

### SPP Integrated Marketplace Markets Web Service

The SOAP web and notification services market participants use to submit offers and bids and retrieve results from the SPP Integrated Marketplace, with WEIS Markets Web Service as the western equivalent and the Settlement Management System API for settlements. SPP's System Interfaces Stakeholder Reference Guide (v1.0, 29 Jan 2025) names these as SOAP and REST services over HTTPS requiring two-factor authentication; the Markets+ platform is stated to move to RESTful APIs. WSDLs, XSDs and endpoint URLs are published only inside the member-facing Change User Forum technical reference documents and were not retrievable anonymously; no base URL is public.

- **Human URL:** [https://www.spp.org/documents/73131/spp%20system%20interfaces%20stakeholder%20reference%20guide%2020250129.pdf](https://www.spp.org/documents/73131/spp%20system%20interfaces%20stakeholder%20reference%20guide%2020250129.pdf)

#### Tags

- SOAP
- Wholesale Markets
- Settlements
- Gated

#### Properties

- [Documentation](https://www.spp.org/documents/73131/spp%20system%20interfaces%20stakeholder%20reference%20guide%2020250129.pdf)
- [Documentation](https://www.spp.org/stakeholder-center/user-guides-apis-integrations/)
- [Documentation](https://www.spp.org/documents/18307/digital%20certificates%20and%20the%20integrated%20marketplace%20doc_final.pdf)

## Common Properties

- [Website](https://www.spp.org/)
- [Portal](https://portal.spp.org/)
- [Documentation](https://www.spp.org/stakeholder-center/user-guides-apis-integrations/)
- [Terms of Service](https://portal.spp.org/terms-of-use)
- [Support](https://rms.spp.org/)
- [Blog](https://www.spp.org/newsroom/)
- [LinkedIn](https://www.linkedin.com/company/southwest-power-pool)

## Mandate and Access Posture

- **Mandate regime:** Other — FERC Order 881 (ambient-adjusted transmission ratings exchange). No consumer energy-data right applies; Green Button/ESPI is a retail-utility standard and SPP has no retail customers.
- **Mandate status:** Live claimed, unverified. SPP published a real versioned LEP/TROLIE data exchange guide, but no production base URL is public and the surface requires a digital certificate plus a UAA role, so no live conformant endpoint could be observed.
- **Data standard:** LF Energy TROLIE (OpenAPI 3.0.3, spec 1.1.0) for ratings; ICCP and PMU streaming for telemetry; proprietary SOAP WSDL/XSD for market and settlement services; no standard reference found for the public CSV market data.
- **Consumer data API:** No — none exists and none should.
- **Market data open:** Yes — anonymous, no key, verified by direct probe.
- **Access gate:** Self-serve for public data (nothing to sign up for); partner-only/membership for participant APIs (OATI webCARES x.509 certificate + SPP UAA roles + an RMS ticket).

## Maintainers

- Kin Lane — kin@apievangelist.com
