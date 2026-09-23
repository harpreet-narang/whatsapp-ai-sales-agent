# Architecture

## Design goal

The agent should behave like a stateful sales workflow, not a stateless FAQ bot.

The system separates four responsibilities:

```text
LLM                  → interpret language
state engine         → control progression
Supabase/PostgreSQL  → persist memory
Meta WhatsApp API    → deliver messages
```

## Inbound path

```text
Meta webhook
  ↓
normalize inbound message
  ↓
load lead by WhatsApp ID
  ↓
structured intent extraction
  ↓
deterministic state transition
  ↓
update qualification memory
  ↓
calculate score / follow-up decision
  ↓
generate response
  ↓
persist lead + message
  ↓
send WhatsApp reply
```

## Why deterministic state transitions?

The LLM is good at extracting:

- intent
- pain points
- objections
- timing
- budget language
- booking intent

It should not have unrestricted authority to change business state.

For example, the model cannot independently decide that a lead is `WON`.

## Human-controlled outcomes

Terminal commercial outcomes are intentionally handled through a separate human-controlled workflow:

```text
HUMAN_HANDOFF
     ↓
human salesperson
     ↓
WON / LOST / NURTURE
```

## Follow-up isolation

Follow-ups are handled by a separate scheduled workflow rather than keeping the inbound execution waiting.

That makes retries, cancellation, and due-time queries easier to observe and test.

## Production extensions

A production version can add:

- CRM sync
- calendar availability
- payment links
- sales-rep assignment
- multiple products
- multilingual replies
- approved WhatsApp template catalog
- attribution / campaign source
- sales analytics dashboard
- SLA monitoring
