---
name: Resolve a company name to FactSet identifiers and securities
description: Concord a company name or third-party identifier to the FactSet entity
  id, then pull its reference data and listed securities.
api: openapi/factset-factset-concordance-api-openapi.yml
operations: [getEntityMatch, getEntityReferences, getEntitySecurities]
generated: '2026-07-22'
method: generated
---

# Resolve a company name to FactSet identifiers and securities

## Auth
HTTP Basic (USERNAME-SERIAL + API key) or OAuth 2.0 bearer from
`auth.factset.com`; base URL `https://api.factset.com`.

## Steps
1. Call `getEntityMatch` — `GET /factset-concordance/v2/entity-match` with
   required `name`. Sharpen the match with `country`, `state`, `url`, or any
   third-party identifier you already hold (`isin`, `cusip`, `sedol`, `ticker`,
   `lei`, `cik`, `duns`, `gvkey`, …). The response ranks candidate FactSet
   entities with confidence signals — take the top match's FactSet entity id.
2. Call `getEntityReferences` — `GET /factset-entity/v1/entity-references`
   (openapi/factset-factset-entity-api-openapi.yml) with `ids` set to the
   entity id to get the entity's reference profile.
3. Call `getEntitySecurities` — `GET /factset-entity/v1/entity-securities` with
   `ids` (and optional `securityType`) to enumerate the entity's listed
   securities for downstream price/estimates calls.

## Rules
- Batch large concordance jobs through the Universe/Task endpoints instead of
  looping `getEntityMatch` (see the Concordance spec's task operations).
- `429` → honor `Retry-After`; `Accept: application/json` required.
- No idempotency-key contract; all three operations are GETs and safe to retry.
