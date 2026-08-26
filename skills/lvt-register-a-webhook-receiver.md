---
name: lvt-register-a-webhook-receiver
description: Build, test, verify and register an HTTPS receiver for LVT Partner API security-alert webhooks, including HMAC-SHA256 signature verification and surviving the auto-disable rule.
api: LVT Partner API
base_url: https://api.lvt.com/v1
operations:
  - POST /webhooks:test
  - POST /webhooks
  - GET /webhooks
  - GET /webhooks/{webhookId}
  - PATCH /webhooks/{webhookId}
  - DELETE /webhooks/{webhookId}
  - GET /publicKeys/{publicKeyId}
generated: '2026-08-25'
method: generated
source: >-
  openapi/lvt-partner-api-openapi.yml + https://docs.lvt.com/r/lvt-partner-api-manual +
  https://github.com/LiveViewTech/lvt-public-api/blob/master/examples/webhooks/README.md
---

# Register an LVT webhook receiver

These operations carry **no `operationId`** in LVT's published OpenAPI, so they are addressed by
method + path.

## Order of work — test before you create

1. **Dry-run first.** `POST /webhooks:test` with `{"url":"https://…","namespace":"securityAlerts"}`.
   This creates **no resource**; it immediately POSTs a signed test payload to your URL and then answers
   **204** (a subsequent `POST /webhooks` will succeed) or **400** (it will not). This is the only
   dry-run affordance in the whole API — use it.
   - Only two validations run: the `url` **must be HTTPS** (HTTP is explicitly disallowed), and
     `namespace` must be valid (`securityAlerts` is currently the only one).
   - Optional helpers: `?action=<action>` makes the test message match that action's documented example;
     a `data` object in the body overrides the payload entirely; `?mimeType=image/jpeg|video/mp4` toggles
     the media type for the `mediaAvailable` action.
2. **Verify the signature in your handler.** Every message carries:
   - `X-LVT-HMAC-SHA256` — base64 signature over the **entire stringified request body**
   - `X-LVT-PUBKEY-URL` — always a `GET /publicKeys/{publicKeyId}` URL, returning `application/x-pem-file`
   Verify with SHA256 and the PEM as-is (no extraction needed). LVT publishes a Node.js `createVerify`
   Express example.
   - **The public key endpoint requires a bearer token**, and LVT explicitly requires you to **cache and
     reuse** the key: it does not rotate, and the Partner API is metered — clients that flood it can be
     blocked.
3. **Create it.** `POST /webhooks` with `{url, namespace, enabled}`. The URL is tested regardless of
   `enabled`; if `enabled` is true, deliveries begin immediately.
4. **Change it carefully.** `PATCH /webhooks/{webhookId}`. If you change `url`, or flip `enabled` from
   false to true, a test notification must be answered **2xx** or the update does not execute.

## The rule that silently kills integrations

A message that does not receive an HTTP **2XX** enters a retry loop with exponential backoff of
`attempt ^ 2` seconds for up to **10 attempts**, after which **the webhook is disabled — with no
notification sent**. Retries are per-message: if message A 500s and message B 200s, A keeps retrying and
can still disable the endpoint. While disabled, **no security alerts are delivered at all**.

Therefore:
- Return 2xx from your handler **before** doing slow work; queue internally.
- Poll `GET /webhooks/{webhookId}` (or `GET /webhooks`) on a schedule and alarm on `enabled: false`.
- Re-enable with `PATCH /webhooks/{webhookId}`.

## Envelope

```json
{
  "action": "eventRaised",
  "attempt": 3,
  "currentAttemptTimestamp": "2024-01-16T19:34:16.335Z",
  "data": {},
  "initialAttemptTimestamp": "2024-01-16T18:33:15.335Z",
  "namespace": "securityAlerts"
}
```

Actions in `securityAlerts`: `alertRaised`, `alertTypeChanged`, `eventRaised`, `mediaAvailable`,
`noteAdded`, `resolved`, `userAssigned`. Full catalog: `asyncapi/lvt-webhooks.yml`.

Media in these payloads carries no URL — mint one with `GET /alerts/media/{mediaId}/url` (expires after
30 minutes).

## Reversal

`DELETE /webhooks/{webhookId}` removes the registration and `PATCH` can disable it; neither has a stated
window. See the `reversibility:` block in `conventions/lvt-conventions.yml`.
