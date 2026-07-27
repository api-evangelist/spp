---
name: Exchange transmission ratings with SPP (LEP/TROLIE)
description: Submit forecast, real-time and seasonal transmission line ratings to Southwest Power Pool and read back the operating limits, using the LF Energy TROLIE contract SPP adopted for FERC Order 881.
api: spp:spp-lep-trolie-api
generated: '2026-07-27'
method: generated
source: openapi/trolie-standard-openapi.yml
standard: LF Energy TROLIE 1.1.0
operations:
  - getDefaultMonitoringSet
  - createMonitoringSet
  - getMonitoringSet
  - updateMonitoringSet
  - deleteMonitoringSet
  - patchRatingForecastProposal
  - getRatingForecastProposalStatus
  - getLimitsForecastSnapshot
  - getHistoricalLimitsForecastSnapshot
  - postRealTimeProposal
  - getRealTimeProposalStatus
  - getRealTimeLimits
  - patchSeasonalRatingsProposal
  - getSeasonalRatingProposalStatus
  - getSeasonalRatingsSnapshot
  - createSeasonalOverride
  - getSeasonalOverrides
  - createTemporaryAARException
  - getTemporaryAARExceptions
---

# Exchange transmission ratings with SPP (LEP/TROLIE)

SPP implements GE Vernova's Limit Exchange Portal against the **LF Energy TROLIE**
specification to meet FERC Order 881 ambient-adjusted ratings obligations. Every
operationId below exists verbatim in `openapi/trolie-standard-openapi.yml`.

**Before you start, understand what is and is not public.** SPP publishes a versioned
implementation guide (SPP LEP/TROLIE API Data Exchange Guide v1.0, 2024-11-08) but
**no production base URL** — its own examples use the placeholder `https://<baseURL>/`.
Access requires an x.509 digital certificate from a CA SPP trusts (OATI webCARES for
Integrated Marketplace applications) presented over mTLS, plus a role and scope in SPP
UAA, requested through an RMS ticket. You cannot reach this API anonymously, and the
running endpoint has not been observed publicly. Never invent a host.

## 1. Get onboarded

1. Your organization must be registered with SPP as a transmission owner/operator or
   market participant.
2. Obtain the digital certificate; have your Local Security Administrator assign the
   UAA roles.
3. Open an RMS ticket (`https://rms.spp.org/`) to request the LEP endpoint and access.
4. Pre-coordinate your **Ratings Obligation** (the resources you provide ratings for)
   and your **Monitoring Sets** with SPP — the specification puts that coordination
   out of scope, so it happens contractually before any call.

## 2. Establish your monitoring set

- `getDefaultMonitoringSet` — read the default set SPP assigned you.
- `createMonitoringSet` → `getMonitoringSet` → `updateMonitoringSet` /
  `deleteMonitoringSet` — manage named sets of power system limits of interest.
- `createMonitoringSet` returns **409 Conflict** with `application/problem+json` when
  the set already exists; `updateMonitoringSet` and `deleteMonitoringSet` also define
  409. Read the problem body, do not blind-retry.

## 3. Propose ratings

| Cadence | Submit | Check status |
|---|---|---|
| Forecast | `patchRatingForecastProposal` | `getRatingForecastProposalStatus` |
| Real-time | `postRealTimeProposal` | `getRealTimeProposalStatus` |
| Seasonal | `patchSeasonalRatingsProposal` | `getSeasonalRatingProposalStatus` |

- Submissions are **not** idempotent-key protected — the contract defines no
  `Idempotency-Key` header (`conventions/spp-conventions.yml`). Do not fire-and-retry
  a proposal on a timeout; poll the matching status operation first.
- `422 Unprocessable Content` with a problem body means the proposal was structurally
  valid but rejected on content (e.g. a real-time proposal outside its window).
- `409 Conflict with Forecast Window` / `409 Conflict with Seasonal Ratings Schedule`
  mean you are outside the submission window — wait for the next window, do not retry.
- `413` means the payload exceeded the limit; split the resource set.

## 4. Read the limits back

- `getLimitsForecastSnapshot` and `getHistoricalLimitsForecastSnapshot` (by `period`)
  for forecast limits; `getRegionalLimitsForecastSnapshot` for the regional view.
- `getRealTimeLimits` / `getRegionalRealTimeLimits` for the real-time snapshot.
- `getSeasonalRatingsSnapshot` for seasonal.
- **Poll with a conditional GET.** Keep the `ETag` from the last response and send
  `If-None-Match`; an unchanged snapshot returns `304 Not Modified` and does not burn
  your request budget. The spec explicitly instructs this.
- Snapshots come in full, `slim`, `detailed` and elide-psr representations — negotiate
  with `Accept`. A failed negotiation returns `406` with a problem body; an
  unsupported request media type returns `415`.

## 5. Exceptions and overrides

- `createTemporaryAARException` / `getTemporaryAARExceptions` /
  `getTemporaryAARException` / `updateTemporaryAARException` /
  `deleteTemporaryAARException` — time-bound exceptions to ambient-adjusted ratings.
- `createSeasonalOverride` / `getSeasonalOverrides` / `getSeasonalOverride` /
  `updateSeasonalOverride` / `deleteSeasonalOverride` — seasonal overrides.
- Deleting either returns **409** once the value is already employed in Operations.
  That is a business state, not a transient error.

## 6. Rate limits and errors

Every response carries `X-Rate-Limit-Limit`, `X-Rate-Limit-Remaining` and
`X-Rate-Limit-Reset`; exhaustion returns `429` with `Retry-After`. SPP publishes no
numeric limits, so read the headers rather than assuming a budget
(`rate-limits/spp-rate-limits.yml`). Errors are RFC 9457 `application/problem+json` —
the full catalogue derived from the spec is in `errors/spp-problem-types.yml`.
`401` and `403` are returned with an empty body: they mean your certificate or your
UAA scope, not your payload.
