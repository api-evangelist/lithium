---
name: Handle a Khoros Flow conversation
description: >-
  Open a thread on the Khoros Flow (flow.ai) REST API, exchange messages, pause or resume the
  bot, hand over to a human, and resolve the conversation.
api: openapi/lithium-flow-flow-rest-api-openapi.json
base_url: https://api.flow.ai/rest/v1
operations:
  - generate-a-thread-id
  - create-a-text-message
  - get-a-list-of-messages
  - get-a-list-of-threads
  - get-bot-status
  - pause-a-bot
  - resume-a-bot
  - handover-action
  - resolve-action
  - trigger-an-event
  - get-a-list-of-events
  - business-hours
  - list-all-flows
---

# Handle a Khoros Flow conversation

Khoros Flow is the flow.ai conversational-automation product. The REST API drives an
individual conversation thread and the bot that serves it.

## Open a thread and exchange messages

1. `generate-a-thread-id` — POST `/messages` returns the thread id all later calls key on.
2. `create-a-text-message` — POST `/messages/{threadId}` sends a message into the thread.
3. `get-a-list-of-messages` — read the thread back.
4. `get-a-list-of-threads` — enumerate threads.

## Control the bot

- `get-bot-status` — the bot's current status.
- `pause-a-bot` / `resume-a-bot` — POST and DELETE on `/pause/{threadId}`. Pause before a
  human takes over so the bot does not talk over the agent.
- `handover-action` — POST `/handover/{threadId}` transfers the conversation.
- `resolve-action` — POST `/resolve/{threadId}` closes it.

## Drive flows from outside

- `trigger-an-event` — POST `/{threadId}` fires an event into the thread to advance a flow.
- `get-a-list-of-events` — the events available to trigger.
- `list-all-flows` — GET `/projects/{projectId}/flows` enumerates the flows in a project.

## Broadcast

- `broadcast-to-msisdn` — send to a phone number.
- `broadcast-to-segments` — send to an audience segment; enumerate them first with
  `get-audience-segments`.

Broadcasts are the highest-consequence operations in this API: they message real people at
scale, they are not idempotent, and there is no undo. Require explicit human approval before
calling either, and confirm the segment size with `get-audience-segments` first.

## Respect business hours

`business-hours` returns the configured hours. Check it before triggering an escalation that
expects a human to answer.

## Rules that apply

- No idempotency contract: a retried `create-a-text-message` or broadcast sends again.
- No published rate limits for Flow — back off on errors rather than assuming headroom.
