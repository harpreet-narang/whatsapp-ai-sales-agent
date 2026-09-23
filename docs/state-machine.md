# Sales State Machine

## Primary lifecycle

```text
NEW
 ↓
DISCOVERY
 ↓
QUALIFYING
 ↓
INTERESTED
 ↓
BOOKING
 ↓
HUMAN_HANDOFF
 ↓
WON / LOST / NURTURE
```

## Stage meaning

### NEW

A prospect has entered the system but no useful discovery has happened yet.

Typical next step:

`NEW → DISCOVERY`

### DISCOVERY

The agent is trying to understand:

- what the prospect wants
- current problem
- relevant product/service

Transition to `QUALIFYING` once enough problem/product context exists.

### QUALIFYING

Collect useful sales information such as:

- company
- role
- scale
- budget signal
- timeline
- urgency

Transition to `INTERESTED` when meaningful fit and buying intent are present.

### INTERESTED

The prospect has enough fit/intent to justify a stronger CTA.

Possible next step:

`INTERESTED → BOOKING`

### BOOKING

The prospect has asked for or accepted a meeting/call.

Store the requested time and either connect a booking system or move toward a human salesperson.

### HUMAN_HANDOFF

Autonomous sales replies stop.

The lead is now owned by a person or another explicitly controlled workflow.

### WON / LOST / NURTURE

Terminal commercial outcomes.

These cannot be set by the conversational LLM.

## Controlled exceptions

The state engine may bypass the normal sequence when the prospect is explicit.

Examples:

```text
NEW + "I want to speak to sales"
→ HUMAN_HANDOFF

DISCOVERY + "Can we book a call?"
→ BOOKING

ANY ACTIVE STAGE + opt-out
→ LOST
```

## Reopening terminal stages

The portfolio implementation does not let the AI silently reopen:

```text
WON
LOST
NURTURE
```

A human-controlled process should make that decision.

## Why the LLM does not own stage

Natural language is probabilistic.

Commercial state is business logic.

Keeping these separate makes transitions inspectable, testable, and auditable.
