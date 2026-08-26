---
name: lvt-deter-onsite-activity
description: Operate the physical deterrence hardware on an LVT mobile security unit — lights, prerecorded sounds, speaker talk-down and PTZ camera positioning — with the irreversibility of each action made explicit.
api: LVT Partner API
base_url: https://api.lvt.com/v1
operations:
  - GetLiveUnit
  - GetLiveUnitLights
  - ToggleLiveUnitLight
  - GetLiveUnitSounds
  - PlayLiveUnitSound
  - GetLiveUnitCallInfo
  - TalkDownStart
  - GetCameraPosition
  - UpdateCameraPosition
generated: '2026-08-25'
method: generated
source: openapi/lvt-partner-api-openapi.yml + https://docs.lvt.com/r/lvt-partner-api-manual
---

# Operate LVT deterrence hardware

**Read this first.** Every operation in this skill actuates hardware at a real, physical site where real
people may be present. Unlike most APIs an agent calls, the effects here are not database rows. LVT's
contract provides **no idempotency key, no dry-run, and no cancel operation** for any of them. Put a
human in the loop.

## Discover what the unit has

- `GetLiveUnit` (`GET /liveUnits/{liveUnitId}`) — unit detail.
- `GetLiveUnitLights` (`GET /liveUnits/{liveUnitId}/lights`) — the lights that exist on this unit.
- `GetLiveUnitSounds` (`GET /liveUnits/{liveUnitId}/sounds`) — the quick sounds this unit can play.
- `GetLiveUnitCallInfo` (`GET /liveUnits/{liveUnitId}/callInfo`) — how to place a talk-down call.

Never assume a light, sound or capability exists — enumerate it first. Scope:
`account.liveUnits.manage`.

## The actions, and how reversible each one is

| Action | Operation | Reversible? |
|---|---|---|
| Toggle a light | `ToggleLiveUnitLight` — `POST /liveUnits/{liveUnitId}/lights/{lightId}:toggle` | **Self-inverse.** It flips state, so calling it again restores the prior state — but a duplicate retry therefore *undoes* your intended change. Read state from `GetLiveUnitLights` before and after. |
| Play a sound | `PlayLiveUnitSound` — `POST /liveUnits/{liveUnitId}/sounds/{soundId}:play` | **No.** A sound emitted at a site cannot be un-played. A duplicate retry plays it twice. |
| Start speaker talk-down | `TalkDownStart` — `POST /liveUnits/{liveUnitId}:call` | **No documented stop.** There is no cancel/end operation in the contract. Treat as irreversible. |
| Move a PTZ camera | `UpdateCameraPosition` — `PUT /cameras/{cameraId}/position` (or `PATCH`, `UpdateCameraPosition2`) | **Yes, caller-side.** Read `GetCameraPosition` (`GET /cameras/{cameraId}/position`) first, keep the PTZF values, and re-`PUT` them to restore. LVT does not frame it as an undo; this is the pattern that works. Scope: `account.cameras.manage`. |

## Operating rules

- **Colon custom methods are literal**: `:toggle`, `:play`, `:call` must terminate the request URL
  exactly as written.
- **`Accept: application/json` is required** on every call or the API returns 406.
- **No idempotency.** There is no `Idempotency-Key` header and no request-key mechanism anywhere in the
  contract. Do not blind-retry a deterrence action on a timeout — read state back (lights) or escalate
  to a human (sounds, talk-down).
- **No 429, no `RateLimit-*` headers**, but the API is metered and LVT can block clients that flood it.
- **Talk-down and streaming share the camera-relay network path.** Outbound `*.camerarelay.com` on 554
  and 1934-1935 must be allowlisted; see the firewall topic in the Partner API manual.
- **Log `errorId`** from every failure — it is the only correlation handle LVT exposes.

## Before an agent fires any of these

Confirm the target unit and location by reading them back (`GetLocation`, `GetLiveUnit`) rather than
trusting an id passed in from context — every id in this API is a bare UUID with no type prefix, so a
`cameraId` and a `liveUnitId` are indistinguishable by inspection and a mix-up points hardware at the
wrong site.
