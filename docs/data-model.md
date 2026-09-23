# Data Model

## sales_leads

One current record per WhatsApp prospect.

Important fields:

| Field | Purpose |
|---|---|
| `wa_id` | WhatsApp user identifier |
| `lead_stage` | Current sales lifecycle stage |
| `product_interest` | Service/product currently discussed |
| `company` | Prospect company |
| `role` | Prospect role |
| `budget` | Budget language or range |
| `timeline` | Buying timeline |
| `pain_point` | Primary problem |
| `objection` | Most recent meaningful objection |
| `last_intent` | Latest classified intent |
| `lead_score` | Deterministic engagement/fit score |
| `meeting_status` | Meeting lifecycle |
| `preferred_meeting_time` | Natural-language booking preference |
| `human_handoff` | Stops autonomous sales continuation |
| `opted_out` | Prevents automated follow-up |
| `last_contact_at` | Most recent prospect contact |
| `next_followup_at` | Current follow-up target |
| `final_outcome` | Won / Lost / Nurture when terminal |

## sales_messages

Immutable conversation log.

Stores:

- inbound/outbound direction
- text
- detected intent
- stage at time of message
- raw provider payload when useful

## sales_stage_history

Append-only audit of stage changes.

Example:

```text
DISCOVERY → QUALIFYING
reason: pain point and product interest captured
```

## sales_followups

Separate scheduled jobs.

This lets a follow-up be:

- scheduled
- sent
- cancelled
- skipped
- failed

without overloading the lead record.

## sales_handoffs

Tracks when and why a lead moved to a human.

A handoff can be resolved separately from the AI conversation.
