---
name: Pull engagement scores from Peakon
description: Retrieve company- and context-level engagement overviews, drivers, and segment scores.
api: openapi/peakon-openapi-original.json
operations:
  - GET /engagement/overview
  - GET /engagement/contexts/{contextId}/overview
  - GET /engagement/drivers
  - GET /engagement/contexts/{contextId}/drivers
  - GET /engagement/contexts/{contextId}/segments/v2
  - GET /scores/contexts/{contextId}/group/{group}
---

# Pull engagement scores from Peakon

Read engagement analytics for reporting or downstream dashboards. Authenticate first
(see `peakon-authenticate.md`); all calls require `Authorization: Bearer <jwt>`.

## Steps
1. **Company overview** — `GET /engagement/overview` for the whole-company engagement score.
2. **Scope to a context** — `GET /engagement/contexts/{contextId}/overview` for a
   department/segment node. Discover contexts/segments via
   `GET /engagement/contexts/{contextId}/segments/v2` and `GET /segments`.
3. **Break down by driver** — `GET /engagement/drivers` (company) or
   `GET /engagement/contexts/{contextId}/drivers`; drill into one with
   `GET /engagement/drivers/{driver}`.
4. **Category / question scores** — `GET /scores/contexts/{contextId}/group/{group}` and
   `GET /scores/contexts/{contextId}/questions/group/{group}`.

## Rules
- Every resource is JSON:API; read data from `data[].attributes` and follow `included[]`.
- Use `filter[...]` query params for JSON:API filtering.
- Handle `404` (unknown context/segment) and `401` (expired session) per
  `errors/peakon-problem-types.yml`.
