---
name: Fetch sell-side consensus estimates for a company
description: Retrieve rolling analyst consensus estimates (revenue, EPS, EBITDA and
  other metrics) from the FactSet Estimates API.
api: openapi/factset-factset-estimates-api-openapi.yml
operations: [getRollingConsensus, getRollingConsensusForList]
generated: '2026-07-22'
method: generated
---

# Fetch sell-side consensus estimates for a company

## Auth
HTTP Basic (USERNAME-SERIAL + API key) or OAuth 2.0 bearer from
`auth.factset.com`; base URL `https://api.factset.com`.

## Steps
1. Call `getRollingConsensus` — `GET /factset-estimates/v2/rolling-consensus`
   with required `ids` (tickers/CUSIP/SEDOL/ISIN/FactSet ids) and `metrics`
   (e.g. SALES, EPS, EBITDA — metric codes per the Estimates docs). Optional:
   `startDate`/`endDate`, `periodicity` (annual/quarterly), `frequency`,
   `relativeFiscalStart`/`relativeFiscalEnd` to window fiscal periods,
   `currency`.
2. For large universes use the POST mirror `getRollingConsensusForList` with the
   same fields in the request body.

## Rules
- `429` → honor `Retry-After`; `Accept: application/json` required (else 406).
- Consensus values roll daily; pin `startDate`/`endDate` for reproducible runs.
- Plain-JSON error envelopes (`errors/factset-problem-types.yml`); no
  idempotency-key contract exists.
