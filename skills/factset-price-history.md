---
name: Pull end-of-day price history for securities
description: Retrieve global end-of-day OHLC prices, volume, and VWAP for a list of
  securities from the FactSet Global Prices API.
api: openapi/factset-factset-global-prices-api-openapi.yml
operations: [getGPDPrices, getSecurityPricesForList]
generated: '2026-07-22'
method: generated
---

# Pull end-of-day price history for securities

## Auth
Authenticate with either HTTP Basic (FactSet USERNAME-SERIAL + API key from
developer.factset.com) or an OAuth 2.0 bearer token from
`https://auth.factset.com/as/token.oauth2` (client credentials; see
`authentication/factset-authentication.yml`). Base URL `https://api.factset.com`
(sandbox: `https://api-sandbox.factset.com`).

## Steps
1. Call `getGPDPrices` — `GET /factset-global-prices/v1/prices` with required
   `ids` (tickers, CUSIP, SEDOL, ISIN, or FactSet ids; comma-separated) and
   `startDate` (YYYY-MM-DD). Optional: `endDate`, `frequency` (D/W/M/Q/Y),
   `currency` (ISO 4217), `adjust` for split/spinoff adjustment, `fields` to
   trim the payload, `calendar`.
2. For large id lists that exceed URL limits, use the POST mirror
   `getSecurityPricesForList` — same contract carried in the request body; set
   `batch` to run asynchronously and poll the returned batch id (Batch Status
   endpoints, see `conventions/factset-conventions.yml` create-then-poll rules).

## Rules
- On `429` wait the `Retry-After` header before retrying; on `503` retry later
  (documented as a retryable timeout).
- Set `Accept: application/json` — the API returns `406` otherwise.
- There is no idempotency-key contract; GETs are safe to retry as-is.
- Errors are plain JSON envelopes, not RFC 9457 (`errors/factset-problem-types.yml`).
