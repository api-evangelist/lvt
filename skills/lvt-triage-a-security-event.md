---
name: lvt-triage-a-security-event
description: Retrieve, review, annotate, assign and resolve a security alert event raised by an LVT mobile security unit, including fetching its evidence media.
api: LVT Partner API
base_url: https://api.lvt.com/v1
operations:
  - GET /alerts/events
  - GET /alerts/events/{id}
  - POST /alerts/events/{id}:addNote
  - POST /alerts/events/{id}:assignUser
  - POST /alerts/events/{id}:resolve
  - GET /alerts/media/{mediaId}/url
generated: '2026-08-25'
method: generated
source: openapi/lvt-partner-api-openapi.yml + https://docs.lvt.com/r/lvt-partner-api-manual
---

# Triage an LVT security event

> **Why this skill names paths, not operationIds.** LVT's OpenAPI v1.0.1 defines **no `operationId`** on
> any of the Events, Media, Webhooks or PublicKeys operations — 13 of 33 in total. Nothing is invented
> here; the operations are addressed the only way the published contract allows.

## Domain model

An **alert** is a single detection raised by a Live Unit, carrying media captured at that moment. An
**event** (`alertEvent`) groups the alerts triggered in the same time frame and is handled as one
incident. An event carries `alerts[]`, `client`, `location`, `liveUnit`, `notes`, `assignedUser`,
`resolution` and a `priority` of `high`, `medium` or `low`.

## Steps

1. **List events.** `GET /alerts/events`. Cursor-paginated (`limit` 1-100, follow `nextCursorUri`).
2. **Read one.** `GET /alerts/events/{id}` — `id` is the event UUID.
3. **Annotate.** `POST /alerts/events/{id}:addNote`.
4. **Assign.** `POST /alerts/events/{id}:assignUser`.
5. **Resolve.** `POST /alerts/events/{id}:resolve`.
6. **Get the evidence.** Alert media carries **no URL** in the schema by design. Call
   `GET /alerts/media/{mediaId}/url` to mint a signed URL. **It expires after 30 minutes** — fetch the
   bytes promptly, and do not persist the signed URL as if it were durable. Media is `video/mp4` or
   `image/jpeg`.

## Rules that will bite you

- **Resolution may not be reversible.** There is no un-resolve, un-assign or delete-note operation in
  the contract, and LVT does not document whether resolution can be undone. Treat `:resolve` as a
  one-way door and confirm with a human before an agent fires it.
- **No idempotency key.** A retried `:addNote` can add the note twice; a retried `:assignUser` and
  `:resolve` are more likely to be state-setting, but the contract does not promise it. Read the event
  back before retrying.
- **Colon verbs are literal.** `:addNote`, `:assignUser` and `:resolve` must terminate the request URL
  exactly.
- **`Accept: application/json` is mandatory** or you get a 406.

## Getting these events pushed instead of polled

Register a webhook — see `skills/lvt-register-a-webhook-receiver.md`. The `securityAlerts` namespace
emits `alertRaised`, `alertTypeChanged`, `eventRaised`, `mediaAvailable`, `noteAdded`, `resolved` and
`userAssigned`, and LVT shapes the payloads so a consumer can reconstruct the full
`GET /alerts/events/{id}` object from the message series. See `asyncapi/lvt-webhooks.yml`.
