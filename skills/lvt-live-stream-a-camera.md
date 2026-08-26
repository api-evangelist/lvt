---
name: lvt-live-stream-a-camera
description: Start, keep alive and cleanly end a live RTSP or WebRTC video stream from a camera on an LVT mobile security unit, using the LVT Partner API.
api: LVT Partner API
base_url: https://api.lvt.com/v1
operations:
  - GetLocations
  - GetLocationLiveUnits
  - GetLiveUnitCameras
  - GetProtocols
  - StartStream
  - CheckIn
  - CheckOut
generated: '2026-08-25'
method: generated
source: openapi/lvt-partner-api-openapi.yml + https://docs.lvt.com/r/lvt-partner-api-manual
---

# Live-stream a camera on an LVT unit

Every operationId below is verified present in `openapi/lvt-partner-api-openapi.yml` (LVT Partner API v1.0.1).

## Before you start

- You need an LVT partnership agreement and a client ID + secret issued by LVT (`integrations@lvt.com`).
- Get a token: `POST https://api.lvt.com/oauth2/v1/token` with `grant_type=client_credentials`,
  `Authorization: Basic base64(CLIENT_ID:CLIENT_SECRET)`,
  `Content-Type: application/x-www-form-urlencoded`. Tokens last 3600 seconds.
- Send `Authorization: Bearer <access_token>` **and** `Accept: application/json` on every call — the API
  returns **406** if `Accept` is missing or is not `application/json`.
- Required scope for this flow: `account.cameras.manage` (and `account.locations.manage` /
  `account.liveUnits.manage` for the discovery steps).

## Steps

1. **Find the unit.** `GetLocations` (`GET /locations`) → `GetLocationLiveUnits`
   (`GET /locations/{locationId}/liveUnits`). Both are cursor-paginated: pass `limit` (1-100) and follow
   the absolute `nextCursorUri` in the response. Do not build a cursor yourself.
2. **Find the camera.** `GetLiveUnitCameras` (`GET /liveUnits/{liveUnitId}/cameras`).
3. **Check what the camera supports.** `GetProtocols` (`GET /cameras/{cameraId}/protocols`) before
   choosing a protocol. Do not assume both `rtsp` and `webrtc` are available on a given camera.
4. **Start the stream.** `StartStream` (`POST /cameras/{cameraId}/streams`) with
   `{"protocol":"rtsp"}` or `{"protocol":"webrtc"}`.
   - RTSP returns `streamId`, `streamingUrl`, `refreshInterval`.
   - WebRTC returns `streamId`, `signalUrl`, `streamInfo`, `refreshInterval`.
5. **Keep it alive.** `CheckIn` (`POST /streams/{streamId}:checkIn`) on the `refreshInterval` the server
   returned (example: 10000 ms). A successful check-in is **204, no body**. The colon is a URL
   delimiter — the literal string `:checkIn` must terminate the URL.
6. **End it.** `CheckOut` (`DELETE /streams/{streamId}`) when finished — **204, no body**.

## WebRTC extra steps

Open a WebSocket to `signalUrl`, send `{direction:"play", command:"getOffer", streamInfo}`, and handle
the reply status: **502** = relay is still connecting, **504** = relay has not started connecting (both
expected for the first seconds — resend after ~1s), **200** = offer ready. Merge the relay's
`streamInfo.sessionId` back into your `streamInfo` before answering. The `streamInfo` keys
(`applicationName`, `streamName`, `sessionId`) are **case-sensitive** — do not let a JSON layer
normalise them.

## Rules that will bite you

- **Always check out.** Letting a stream time out works, but explicit `CheckOut` avoids undefined
  behaviour. After checkout or timeout, further `CheckIn` or `DELETE` on that `streamId` return **404**.
- **The real timeout is not published.** It is longer than `refreshInterval`, so one or two missed
  check-ins will not drop the stream — but do not derive a number from that.
- **No idempotency.** There is no `Idempotency-Key`. A retried `StartStream` starts another stream.
  Record the `streamId` from the first response and reconcile before retrying.
- **No rate-limit signal.** The API declares no 429 and returns no `RateLimit-*` headers, but LVT
  documents that the API is metered and that flooding clients can be blocked. Back off conservatively.
- **Firewall.** Streaming needs outbound access to `*.camerarelay.com` on 554 and 1934-1935 (plus
  UDP 6970-10000 for Firefox). See the allowlisting topic in the Partner API manual.

## Errors

All errors are `application/json` with `{errorCode, errorSummary, errorId, errorCauses[]}` — **not**
RFC 9457. Log `errorId`: it is the only handle LVT can map to a server-side error. See
`errors/lvt-problem-types.yml`.
