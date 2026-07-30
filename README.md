# Lithium

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
