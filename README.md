# Lithium

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

Lithium Technologies is the enterprise online-community and social-customer-engagement platform that, after merging with Spredfast in 2018, rebranded as **Khoros** and is today operated by **IgniteTech**.

The Lithium name is not merely historical — it is still load-bearing in the live platform:

- the Khoros Care APIs are served from `*.response.lithium.com` and `api.app.lithium.com`
- the Community plugin SDK and its `li` CLI still ship on npm as **`lithium-sdk`**
- the public GitHub organization is still **[github.com/lithiumtech](https://github.com/lithiumtech)** (GitHub-verified, display name "Khoros")

## Developer surfaces

| Product | Docs | Base URL | Lineage |
|---|---|---|---|
| Khoros Care | [khoroscaredevdocs](https://developer.khoros.com/khoroscaredevdocs/reference) | `api.app.lithium.com`, `{instance}.response.lithium.com` | Lithium Social Response |
| Khoros Marketing | [khorosmarketingdevdocs](https://developer.khoros.com/khorosmarketingdevdocs/reference) | `api.spredfast.com`, `api.massrelevance.com` | Spredfast / Mass Relevance |
| Khoros Flow | [khoros-flow](https://developer.khoros.com/khoros-flow/reference) | `api.flow.ai/rest/v1` | flow.ai |
| Khoros Community | [khoroscommunitydevdocs](https://developer.khoros.com/khoroscommunitydevdocs/reference/khoros-communities-platform-apis) (login-gated) | `{community}/api/2.0` (LiQL), `/restapi/vc/` (v1) | Lithium Community |

**28 OpenAPI 3.1 definitions / 273 operations** were harvested from the developer portal, all carrying operationIds and summaries.

## Notable findings

- **LiQL** (Lithium Query Language), a SQL-like query language over community collections, is the platform's signature interface convention and has no standards-body equivalent.
- **No idempotency contract** anywhere in the surface — no `Idempotency-Key` header or parameter in any of the 28 definitions.
- **One OAuth scope**: Khoros Marketing publishes `all` (read/write across every endpoint). A read-only token cannot be issued.
- **Four authentication models**, one per product: HTTP Basic + JWT (Care), OAuth 2.0 (Marketing), API key (Flow), API app credentials (Community).
- **`lithium.com` serves an expired TLS certificate** (expired 2026-01-03) while asserting HSTS with a one-year max-age, so browsers cannot reach the original company domain at all.
- **No security.txt and no vulnerability disclosure channel**, despite a published security program carrying ISO 27001, SOC 2 Type II, FedRAMP and FISMA certifications.
- **No AsyncAPI**, but a real webhook surface: the Khoros Marketing Notification Service manages event subscriptions and supports pull-based replay.

## Artifacts

`openapi/` `overlays/` `authentication/` `scopes/` `conventions/` `errors/` `rate-limits/` `lifecycle/` `changelog/` `conformance/` `data-model/` `asyncapi/` `packages/` `cli/` `components/` `mcp/` `skills/` `llms/` `well-known/` `security/`

Backed by: sapphire-ventures — https://khoros.ai/
