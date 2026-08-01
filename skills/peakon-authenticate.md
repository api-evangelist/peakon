---
name: Authenticate with the Peakon API
description: Exchange a Peakon custom-app access token for a session bearer JWT and call the API.
api: openapi/peakon-openapi-original.json
operations:
  - POST /auth/application
---

# Authenticate with the Peakon API

Use this before any other Peakon API call. Peakon uses a two-step application
authentication model (see `authentication/peakon-authentication.yml`).

## Prerequisites
- A **custom application access token**, created in the Peakon dashboard under
  Integrations > Custom Apps. Grant only the permission areas your use case needs.
- The company **subdomain** (API host is `https://{subdomain}.peakon.com/api/v1/`,
  default subdomain `api`).

## Steps
1. **Exchange the app token for a session JWT** — `POST /auth/application` with body
   `{ "token": "<application access token>", "persists": false }`. Set `persists: true`
   only if you want a session that never expires.
2. The response is a JSON:API resource of type `bearer_tokens`; the `id` **is** the JWT.
3. **Send the JWT on every subsequent call** as `Authorization: Bearer <jwt>`.

## Rules
- Treat the application access token as a secret; it is shown once at creation.
- On `401`, the session token is missing/expired — repeat step 1.
- Responses are JSON:API (`application/vnd.api+json`); errors come back as a top-level
  `errors` array (see `errors/peakon-problem-types.yml`).
