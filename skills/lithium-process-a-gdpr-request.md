---
name: Process a GDPR data-subject request in Khoros Care
description: >-
  Submit a GDPR data-subject request against a Khoros Care instance and poll it to completion.
api: openapi/lithium-care-gdpr-api-v2-openapi.json
base_url: https://{instance}.response.lithium.com/api/v2/gdpr
operations:
  - requests
  - requestsrequestuuid
---

# Process a GDPR data-subject request in Khoros Care

Khoros Care exposes a dedicated GDPR API for data-subject requests. It is small — create and
poll — but it is the highest-consequence surface in the platform, because the outcome is
irreversible erasure or export of a real person's data.

## Submit

`requests` — POST `/requests` creates the GDPR request. Authentication is HTTP Basic against
the per-instance host `https://{instance}.response.lithium.com/api/v2/gdpr`.

## Poll

`requestsrequestuuid` — GET `/requests/{requestUUID}` returns the request status. There is no
webhook for completion; poll.

## Rules that apply

- **Require human authorization.** An agent must never call `requests` autonomously. Erasure
  cannot be undone through the API and there is no cancel operation.
- **No idempotency.** A retried POST creates a second request against the same data subject.
  Record the returned `requestUUID` and poll it rather than resubmitting on a timeout — a
  timeout does not mean the request was not accepted.
- **Identify the subject first.** Resolve the author with the Author API
  (`../skills/lithium-look-up-a-care-author.md`) and confirm the `lswUuid` before submitting,
  so the request targets the right person.
- Khoros' GDPR and CCPA obligations are set out in the Data Protection Agreement at
  https://khoros.ai/customer-agreements/.
