---
name: Sync employees into Peakon
description: Discover, create, update, and delete employee records to keep Peakon in sync with an HRIS.
api: openapi/peakon-openapi-original.json
operations:
  - GET /employees
  - GET /employees/{employeeId}
  - POST /employees
  - PATCH /employees/{employeeId}
  - DELETE /employees/{employeeId}
  - GET /employees/count
---

# Sync employees into Peakon

Keep Peakon's employee list aligned with a source-of-truth HRIS. Authenticate first
(see `peakon-authenticate.md`). For continuous provisioning, prefer the SCIM 2.0 API
(`authentication/peakon-authentication.yml` > scim); this skill covers the native
JSON:API employee endpoints.

## Steps
1. **Discover** — `GET /employees` with a `filter[...]` (e.g. `filter[account.email]`
   or `filter[employeeId]`) to check whether the person already exists. Use
   `GET /employees/count` to size the sync.
2. **Create** — if absent, `POST /employees` with the basic attributes (email, name,
   locale, timezone, employee number).
3. **Update** — `PATCH /employees/{employeeId}` with the full attribute set. Do this as a
   separate step from create so all referenced segments/attributes exist first.
4. **De-provision** — `DELETE /employees/{employeeId}` (returns `204 No Content`).

## Rules
- Pick one shared unique identifier (email by default, or employee number) and use it
  consistently for discovery.
- No idempotency-key is supported; guard creates with a discovery read first.
- JSON:API request/response bodies; errors are a top-level `errors` array.
