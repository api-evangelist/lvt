# LVT

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

LVT (LiveView Technologies) is an American physical-security company that builds solar-powered, cellular-connected mobile surveillance units (MSUs) and the SaaS/PaaS platform that runs them — live video, AI analytics, alerting, lights, loudspeakers and AI talk-down — for retail, commercial, industrial, critical-infrastructure and public-sector sites. Founded in 2005 and headquartered in American Fork, Utah, LVT publishes a public Partner API: a REST service over JSON, described by a first-party OpenAPI 3.0.3 document on GitHub, that lets SOC operators and integrators list live units and locations, control cameras (pan/tilt/zoom), start RTSP and WebRTC streams through the LVT camera relay, play sounds, toggle lights, initiate speaker talk-down, review and resolve security alert events, fetch signed media URLs, and register HMAC-signed webhooks. The same API backs LVT's pre-built integrations with Immix, Axon Fusus and Genetec Security Center, and LVT units also present themselves to third-party VMS platforms as ONVIF cameras.

**Areas:** Company · Physical Security · Video Surveillance · Camera · Video Streaming · WebRTC · RTSP · ONVIF · IoT · Alerts · Webhook · Public Safety

## LVT on APIs.io

Everything profiled for LVT is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/lvt/](https://apis.io/providers/lvt/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [LVT Cameras API](https://apis.io/apis/lvt/lvt-cameras-api/) — Operations on cameras
- [LVT Events API](https://apis.io/apis/lvt/lvt-events-api/) — The Events API from LVT — 5 operation(s) for events.
- [LVT Live Units API](https://apis.io/apis/lvt/lvt-liveunits-api/) — Operations on LiveUnits
- [LVT Locations API](https://apis.io/apis/lvt/lvt-locations-api/) — Operations on locations
- [LVT Media API](https://apis.io/apis/lvt/lvt-media-api/) — The Media API from LVT — 1 operation(s) for media.
- [LVT Streams API](https://apis.io/apis/lvt/lvt-streams-api/) — The Streams API from LVT — 3 operation(s) for streams.
- [LVT Talkdown API](https://apis.io/apis/lvt/lvt-talkdown-api/) — Operations related to Partner Talkdown
- [LVT Webhooks API](https://apis.io/apis/lvt/lvt-webhooks-api/) — Operations on webhooks
- [LVT Public Keys API](https://apis.io/apis/lvt/lvt-public-keys-api/) — Operations related to outbound message verification

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Lvt Webhooks](https://apis.io/asyncapis/lvt/lvt-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [lvt](https://apis.io/servers/lvt/lvt/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Lvt Partner Api Overlay](https://apis.io/overlays/lvt/lvt-partner-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Lvt Authentication](https://apis.io/security/lvt/lvt-authentication/)
- [Lvt Domain Security](https://apis.io/security/lvt/lvt-domain-security/)
- [Lvt Trust Center](https://apis.io/security/lvt/lvt-trust-center/)
- [Lvt Vulnerability Disclosure](https://apis.io/security/lvt/lvt-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Lvt Scopes](https://apis.io/scopes/lvt/lvt-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Lvt Plans Pricing](https://apis.io/plans/lvt/lvt-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Lvt Rate Limits](https://apis.io/rate-limits/lvt/lvt-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.lvt.com/)
- [Documentation](https://docs.lvt.com/r/lvt-partner-api-manual)
- [Getting started](https://github.com/LiveViewTech/lvt-public-api)
- [API reference](https://github.com/LiveViewTech/lvt-public-api/blob/master/api-specs/api.yaml)
- [Pricing](https://www.lvt.com/pricing)
- [Sign up](https://vms.liveviewtech.com/)
- [Support](https://www.lvt.com/contact)
- [Blog](https://www.lvt.com/blog)
- [Trust centre](https://trust.lvt.com/)
- [Terms of service](https://www.lvt.com/legal/msa)
- [Privacy policy](https://www.lvt.com/legal/privacy)
- [GitHub](https://github.com/LiveViewTech)
- [Status](https://status.lvt.com/)

## API Evangelist rating

**Kin Score 54.8** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 60.3 |
| Contract governance | 18.2 |
| Operational transparency | 36.8 |
| Developer ergonomics | 39.9 |
| Access clarity | 68.4 |

**Agent readiness 29.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | negotiable (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | mostly |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/lvt/](https://apis.io/providers/lvt/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for LVT: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/lvt/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
