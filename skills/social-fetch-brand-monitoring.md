---
name: social-fetch-brand-monitoring
description: Watch a social account or keyword and receive a signed webhook when something new is posted.
api: Social Fetch REST API
generated: '2026-09-11'
method: generated
source: openapi/social-fetch-openapi.json + docs/monitors/quickstart.mdx
operations:
  - GET /v1/monitors/sources
  - POST /v1/webhook-endpoints
  - POST /v1/monitors
  - GET /v1/monitors/{id}/checks
  - POST /v1/webhook-endpoints/{id}/rotate-secret
---

# Monitor a source and get signed webhooks

Set up a Monitor that watches a supported source and delivers a **signed** webhook
whenever a new item appears.

## Steps

1. **See what can be watched:** `GET /v1/monitors/sources` lists every public
   operation Monitors can watch.

2. **Create a delivery target:** `POST /v1/webhook-endpoints` with `kind=http` and
   your HTTPS URL (or `kind=sink` for a capture inbox while testing). Store the
   returned signing secret — it is shown once. Verify signatures on every delivery.

3. **Create the monitor:** `POST /v1/monitors` pointing at the source and the
   webhook endpoint. The first (baseline) check records what already exists and
   does NOT fire events; only later discoveries deliver.

4. **Diagnose delivery:** if you expected a webhook and did not get one, call
   `GET /v1/monitors/{id}/checks` (the "why didn't I get a webhook" view) and
   `GET /v1/monitors/{id}/events` to pull events regardless of push state.

5. **Rotate secrets safely:** `POST /v1/webhook-endpoints/{id}/rotate-secret`
   keeps the old secret valid for a 24h overlap — deploy the new secret within
   that window.

## Notes
- Delivery is at-least-once; deduplicate on the event id.
- `POST /v1/monitors/{id}/trigger` forces an immediate check (max once per 60s).
- Deleting a monitor or endpoint is permanent — disable (`enabled:false`) instead
  when you may want it back.
