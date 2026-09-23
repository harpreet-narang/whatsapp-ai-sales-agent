# WhatsApp Integration

## Inbound messages

The main workflow accepts Meta-style WhatsApp webhook payloads and also supports a simplified test payload.

The normalizer extracts:

- WhatsApp user ID
- display name
- message ID
- text/button/list reply
- message timestamp

Provider status updates are ignored by the conversational branch.

## Outbound messages

The example sender uses the Meta Graph API with environment variables:

```text
META_GRAPH_API_VERSION
WHATSAPP_PHONE_NUMBER_ID
WHATSAPP_ACCESS_TOKEN
```

No credential is stored in the workflow JSON.

## Idempotency

Production systems should treat the Meta message ID as an idempotency key.

The public schema keeps `provider_message_id` unique where available so webhook retries do not need to create duplicate message records.

## Follow-ups

Follow-up messages are handled separately from immediate replies.

A production deployment should enforce the currently applicable Meta messaging/template rules before sending an outbound follow-up.

## Verification

Meta webhook verification can be handled by:

- n8n webhook verification workflow
- reverse proxy/application layer
- another small endpoint

It is kept outside the core sales-state demonstration so the portfolio workflow stays readable.
