---
name: Register and run a Khoros Care bot
description: >-
  Register a bot with the Khoros Care Automation Framework, respond to inbound customer
  messages, annotate and route the conversation, and hand control back to a human agent.
api: openapi/lithium-care-bot-api-v3-openapi.json
base_url: https://api.app.lithium.com/bots/v3
operations:
  - registrations-post
  - botsv3registrations
  - respond-post
  - note-put
  - tag-put
  - priority-put
  - control-put
  - workqueue-put
  - resolve-put
  - tokensappid-put
  - healthappidappid
---

# Register and run a Khoros Care bot

The Automation Framework is how a bot participates in a Khoros Care conversation. Unlike the
rest of Care, the Bot API v3 authenticates with a **JWT**, not HTTP Basic, and it lives on
`api.app.lithium.com/bots/v3` rather than the per-instance `*.response.lithium.com` host.

## Authenticate

Bot API v3 requires a signed JSON Web Token — header, payload, signature — carrying the
company key, user name and AWS region. See
https://developer.khoros.com/khoroscaredevdocs/docs/jwt-authentication. Refresh with
`tokensappid-put` before expiry and invalidate with `tokensappid-delete` when retiring an
app. Care v1 and v2 APIs use Basic auth and a different host; do not reuse credentials
across them.

## Register the bot

1. `registrations-post` — register the bot against a network key and external id.
2. `botsv3registrations` — confirm the registration is visible under your JWT.
3. `networknetworkkeyexternalidexternalidappidappid-get` — read back the
   registration detail for a specific app id.

Bot operating mode is changed with
`registrationsnetworknetworkkeyexternalidexternalidappidappidmodemode-put`.

## Handle an inbound message

Khoros delivers inbound activity by webhook (see `../asyncapi/lithium-webhooks.yml`). For
each message:

1. `respond-post` — send the bot's reply back to the channel.
2. `note-put` — add an internal note visible to agents but not to the customer.
3. `tag-put` — tag the message by Care tag id (`untag-message-by-care-tag-id` to reverse).
4. `priority-put` — raise or lower conversation priority.

If the bot needs a secure data capture (payment details, identity), use `send-a-bot-response-with-a-secure-form`
rather than asking for the value in plain conversation.

## Hand off or close

- `control-put` — pass conversation control to a human agent or another entity. Confirm who
  holds control with `controlnetworknetworkkeyexternalidexternalidauthorauthorid-get`.
- `workqueue-put` — move the conversation to a specific work queue.
- `resolve-put` — mark the conversation resolved.

## Rules that apply to every call

- **No idempotency.** There is no `Idempotency-Key` header anywhere in this API. A retried
  `respond-post` sends a second message to the customer. Track your own request ids and use
  `requestrequestid` to check whether a request already landed before retrying.
- **Rate limits.** Care Author and Conversation APIs allow 60 calls per 60 seconds. No
  rate-limit headers are returned, so throttle client-side
  (`../rate-limits/lithium-rate-limits.yml`).
- **Errors.** No `application/problem+json`. Parse the product-specific JSON body; see
  `../errors/lithium-problem-types.yml`.
- **Health.** `healthappidappid` reports bot health by app id; poll it rather than inferring
  health from failures.
