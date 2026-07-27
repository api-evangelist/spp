---
name: Pull SPP public market data
description: Retrieve Southwest Power Pool locational marginal prices, market clearing prices, operating reserves, load, generation mix, curtailments and outage capacity from SPP's anonymous public data surface, without any credential.
api: spp:spp-portal-file-browser-api
generated: '2026-07-27'
method: generated
source: verified anonymous probes recorded in review.yml and data-model/spp-data-model.yml
operations: []
endpoints:
  - https://portal.spp.org/file-browser-api/download/{dataset}?path={file}
  - https://portal.spp.org/chart-api/{feed}/asChart
  - ftp://pubftp.spp.org
---

# Pull SPP public market data

Southwest Power Pool publishes wholesale market data with no account, no API key and
no licence click-through. There is **no OpenAPI for this surface** — every path below
was verified anonymously on 2026-07-27 and is listed in `data-model/spp-data-model.yml`.
Do not guess dataset slugs or column names; use the catalogued ones.

## Choose the right interface

| Need | Use |
|---|---|
| Programmatic access SPP formally supports | anonymous FTP at `ftp://pubftp.spp.org` |
| A single dated CSV file | `GET https://portal.spp.org/file-browser-api/download/{dataset}?path={file}` |
| A pre-shaped JSON series for a dashboard | `GET https://portal.spp.org/chart-api/{feed}/asChart` |
| Geospatial price contours | `GET https://pricecontourmap.spp.org/arcgis/rest/services/PCM/{service}/MapServer?f=json` |

SPP's System Interfaces Stakeholder Reference Guide states that access outside
officially published SPP interface specifications is "neither supported nor
encouraged". FTP is the supported path; the HTTP paths are the portal's internals.
Say so when reporting data provenance.

## Steps

1. **Pick the dataset slug.** Real-time prices `rtbm-lmp-by-location`; real-time
   clearing prices `rtbm-mcp`; day-ahead clearing prices `da-mcp`; reserves
   `operating-reserves`; load `hourly-load`; renewables curtailment
   `ver-curtailments`; outages `capacity-of-generation-on-outage`; fuel mix
   `generation-mix-historical`; western market `lmp-by-settlement-location-weis`.
2. **Build the path.** Latest interval files use a stable name, e.g.
   `?path=/RTBM-MCP-latestInterval.csv`. History is date-partitioned by convention,
   e.g. `?path=/2026/07/DA-MCP-202607250100.csv`. **There is no listing endpoint** —
   `/file-browser-api/` returns HTTP 404 with an empty body — so paths must be
   constructed, and a wrong path fails silently as a 404 rather than an error object.
3. **Request anonymously.** Send no Authorization header. Expect `200` with
   `content-type: text/csv` (or `application/json` for chart feeds).
4. **Parse against the known header row.** Column names are in
   `data-model/spp-data-model.yml`; a sample response is in
   `examples/spp-rtbm-mcp-latest-interval.csv`. Both a local `Interval` and a
   `GMTIntervalEnd` column are present on interval data — always key on the GMT
   column and convert once, never mix the two.
5. **Respect the grain.** Real-time datasets are 5-minute intervals; day-ahead and
   load are hourly; `hourly-load` is wide, one column per load-serving entity code
   (CSWS, EDE, GRDA, …) rather than long format.
6. **Backfill politely.** No rate limits are published and none are signalled in
   response headers (`rate-limits/spp-rate-limits.yml`). Some files are large — the
   year-to-date generation mix file exceeded 9 MB and the rolling 365-day chart file
   16 MB — so cache locally, request each dated file once, and prefer FTP for bulk.
7. **Rehearse against MTE.** `https://portal-mte.itespp.org` and
   `ftp://pubftp-mte.itespp.org` mirror the same path conventions in SPP's Model Test
   Environment (`sandbox/spp-sandbox.yml`). Use them to validate a client; never
   report MTE numbers as market truth.

## Failure handling

- `404` with a zero-byte body means the constructed path does not exist — most often
  a date that has not been published yet, or the wrong partition depth.
- `200` with `content-type: text/html` and ~609 bytes is the portal's React shell,
  not data. Treat it as a miss, not a payload.
- There is no error envelope, no request id and no status page. If a dataset stops
  updating there is nothing to poll but the data itself.
