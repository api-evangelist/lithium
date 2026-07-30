---
name: Look up a Khoros Care author and their conversations
description: >-
  Resolve a customer from a social handle or network id to a Khoros Care author record, read
  their conversation history, and update or merge their author attributes.
api: openapi/lithium-care-author-api-v2-openapi.json
base_url: https://{instance}.response.lithium.com/api/v2
operations:
  - authorsnetworks_networktype_handles_networkhandle
  - authorsnetworksnetworktypeidsnetworkid
  - authorslswuuid
  - authors_lswuuid_conversations
  - authorsnetworksnetworktypehandlesnetworkhandleconversations
  - authorschanged
  - authors-1
  - authorslswuuid-1
  - authorsnetworksnetworktypeidsnetworkidconversationsreactivate
  - lswuuid-1
---

# Look up a Khoros Care author and their conversations

Khoros Care models a customer as an **author** with a stable `lswUuid`, linked to one or more
source-network identities (a Twitter/X handle, a Facebook PSID, and so on). Most integration
work starts by resolving an inbound identity to that author.

Base URL is per customer instance: `https://{instance}.response.lithium.com/api/v2`. The
Author and Conversation APIs use **HTTP Basic** authentication.

## Resolve the author

Pick the lookup that matches what you have:

- Social handle → `authorsnetworks_networktype_handles_networkhandle`
- Network id → `authorsnetworksnetworktypeidsnetworkid`
- Network id scoped to an instance →
  `authorsnetworksnetworktypeinstancesnetworkinstanceidsnetworkid-1`
- Care author id → `authorslswuuid`

Create a new author with `authors-1` when no record exists.

## Read the conversation history

- `authors_lswuuid_conversations` — conversation ids for a Care author, by day.
- `authorsnetworksnetworktypehandlesnetworkhandleconversations` — conversation ids straight
  from a network handle.
- `lswuuid-1` (Conversation API) — full conversation detail by unique id.
- `get-conversation-details-by-document-source-id` — when you hold the source platform's
  document id rather than a Khoros id.

## Maintain the record

- `authorslswuuid-1` — update or merge author handles by Care author id. Merging is how two
  identities for the same human are collapsed; it is not reversible through the API.
- `authors-2` — batch update author records.
- `authorsnetworksnetworktypeinstancesnetworkinstanceidsnetworkidsplit` — remove Person
  attributes by network id and instance.
- `authorsnetworksnetworktypeidsnetworkidconversationsreactivate` — reactivate an author's
  closed conversations.

## Keep a mirror in sync

`authorschanged` lists author records changed today. Poll it to keep a downstream CRM in
step; there is no author-changed webhook.

## Rules that apply to every call

- **Rate limit:** 60 calls per 60 seconds across Author and Conversation APIs, with no
  rate-limit headers returned. Throttle client-side.
- **No idempotency:** a retried `authors-1` creates a second author. Resolve before you
  create.
- **Pagination:** `authors` returns a paginated list; pagination parameters differ by
  endpoint (see `../conventions/lithium-conventions.yml`).
