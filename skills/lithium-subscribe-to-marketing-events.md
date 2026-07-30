---
name: Subscribe to Khoros Marketing events
description: >-
  Create and manage webhook subscriptions on the Khoros Marketing Notification Service, and
  replay events that were missed.
api: openapi/lithium-marketing-notification-service-openapi.json
base_url: https://api.spredfast.com/v2/events
operations:
  - create-a-subscription
  - list-subscriptions
  - retrieve-subscription-details
  - update-a-subscription
  - delete-a-subscription
  - retrieve-subscription-events
---

# Subscribe to Khoros Marketing events

The Notification Service is the one real event surface in the Khoros platform. A subscription
binds a single `eventName` (at a chosen `version`) to a `notificationUri` that Khoros will
POST to.

## Authenticate

Khoros Marketing uses OAuth 2.0 against `https://login.spredfast.com/v3/oauth/authorize` and
`/v3/oauth/token`. The only scope is `all` — read/write across every endpoint, so there is no
read-only token. Verify what a token represents with the introspection endpoint
`https://api.spredfast.com/v2/whoami`.

Every call also carries tenancy headers: `x-sf-company-id`, `x-sf-initiative` and
`x-sf-user-email`. Bind these per connection; they are not agent-selectable.

## Create the subscription

`create-a-subscription` (POST `/subscription`) with:

- `eventName` — the event to listen for.
- `version` — the payload version to be delivered.
- `notificationUri` — your HTTPS endpoint.
- `bearerToken` — optional; Khoros presents it on delivery so your endpoint can authenticate
  the caller. **Set this.** There is no request-signing scheme, so without it you cannot tell
  a genuine delivery from a forged one.
- `externalId` — your own identifier for the subscription.
- `query` — optional regex filter over events.

Events documented by Khoros include message, rule applied, label applied to stream item, bots
pass conversation control, errors, and channel de-authorization. See
`../asyncapi/lithium-webhooks.yml`.

## Manage and replay

- `list-subscriptions` — all subscriptions for the tenant.
- `retrieve-subscription-details` — one subscription by id.
- `update-a-subscription` — PUT `/subscription/{id}/{status}` to move between `ACTIVE`,
  `PAUSED` and `DISABLED`.
- `delete-a-subscription` — remove it.
- `retrieve-subscription-events` — GET `/data/{subscriptionId}` pulls the events for a
  subscription. This is the recovery path when your endpoint was down: pull rather than wait
  for a redelivery, because no retry contract is published.

## Rules that apply

- No delivery-retry, ordering or signature-verification contract is documented. Treat
  deliveries as at-most-once and reconcile with `retrieve-subscription-events`.
- No idempotency on `create-a-subscription`; a retry creates a duplicate subscription and
  your endpoint will receive each event twice. List before you create.
