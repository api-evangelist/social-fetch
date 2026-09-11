---
name: social-fetch-profile-lookup
description: Resolve a social handle to profile metrics and recent posts using the Social Fetch REST API.
api: Social Fetch REST API
generated: '2026-09-11'
method: generated
source: openapi/social-fetch-openapi.json (routes verified against the live spec)
operations:
  - GET /v1/whoami
  - GET /v1/tiktok/profiles/{handle}
  - GET /v1/tiktok/profiles/{handle}/videos
  - GET /v1/twitter/profiles/{handle}
  - GET /v1/instagram/profiles/{handle}
  - GET /v1/instagram/profiles/{handle}/posts
---

# Look up a social profile and its recent posts

Resolve a handle to profile metrics, then page its recent content. The pattern is
identical across platforms — swap the `/v1/{platform}/...` prefix.

## Prerequisites
- API key in `x-api-key: sfk_...` (create at https://app.socialfetch.dev/api-keys).
- 100 free credits are granted on signup. `whoami` and `balance` are free.

## Steps

1. **Smoke-test auth (free):**
   `GET /v1/whoami` with the `x-api-key` header. A 200 confirms the key.

2. **Fetch the profile:** call the platform profile route, e.g.
   `GET /v1/tiktok/profiles/{handle}` (handle with or without a leading `@`).
   Read `data.lookupStatus` FIRST — a 200 with `not_found` or `private` means the
   payload is not a successful find (and still costs credits).

3. **Page recent posts:** call the profile content route, e.g.
   `GET /v1/tiktok/profiles/{handle}/videos`. Read `data.page.nextCursor` and send
   it back as the `cursor` query parameter; stop when `data.page.hasMore` is false.
   Never build or decode a cursor yourself.

## Conventions to respect
- Responses use `{ data, meta }`; keep `meta.requestId` for support.
- Errors use `{ error: { code, message, requestId } }` (not problem+json).
- On `503`, honor `Retry-After`; do not blindly retry `4xx`.
- Metered routes bill on successful lookup, including `not_found`/`private` on 200.
