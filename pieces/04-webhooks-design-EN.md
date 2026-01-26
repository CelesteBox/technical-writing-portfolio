### Integrations · APIs · Webhooks (EN)

# Webhooks: Design, Security, and Recommended Patterns for a SaaS That Takes Integrations Seriously

Webhooks are deceptively simple.

On paper, they’re just HTTP callbacks: *“something happened, here’s the payload.”*
In practice, poorly designed webhooks are one of the fastest ways to create brittle integrations, frustrated developers, and support tickets that never quite die.

If your SaaS exposes webhooks, you are not just sending events. You are designing an external contract.

This article covers the core design, security, and reliability patterns that separate “we technically have webhooks” from “our integrations actually work.”

### What webhooks are (and what they are not)

A webhook is an **event-driven push mechanism**. Your system sends an HTTP request to a customer-defined endpoint when a specific event occurs.

Key implications:

* delivery is **asynchronous**
* you do **not control** the receiving system
* failure is normal, not exceptional

Webhooks are not APIs. They invert control. That inversion is where most problems start.

### Designing webhook events: think in facts, not actions

A common mistake is to model webhook events around internal actions:

> `user_created`, `invoice_generated`, `subscription_updated`

Those are fine names — but the payload must represent **facts**, not instructions.

Good webhook payloads:

* describe *what happened*
* include stable identifiers
* avoid leaking internal implementation details

Example (simplified):

```json
{
  "event": "subscription.updated",
  "occurred_at": "2026-01-25T14:32:10Z",
  "data": {
    "subscription_id": "sub_123",
    "status": "active",
    "previous_status": "trialing"
  }
}
```

Avoid payloads that imply what the consumer should *do*. Consumers decide that.

### Version your webhook schema (you will need it)

Webhook consumers often process events automatically. Breaking changes are costly.

Recommended practices:

* version the event schema explicitly (`v1`, `v2`)
* keep old versions active during a deprecation window
* document differences clearly

Never assume consumers will “just update.” They won’t. Or they can’t.

### Delivery guarantees: be honest, then be reliable

You cannot guarantee delivery. Networks fail. Endpoints go down.

What you *can* guarantee:

* **at-least-once delivery**
* **retry behavior**
* **ordering semantics (or lack thereof)**

Best practices:

* retry with exponential backoff
* retry only on network / 5xx errors
* stop retrying eventually and surface failures

Document this explicitly. Ambiguity here causes integration bugs that are painful to debug.

### Idempotency: assume duplicates will happen

Because retries exist, **duplicate events are expected**.

Include:

* a unique event ID
* a consistent event structure

Consumers should be able to safely ignore duplicates. Your documentation should say this clearly.

If your webhook system *doesn’t* generate duplicates, that’s fine — but consumers should not rely on that.

### Security: webhooks are an attack surface

Sending data to arbitrary endpoints is risky by default.

Minimum security baseline:

* HTTPS only
* request signing (HMAC with shared secret)
* timestamp + signature validation

Example flow:

1. You compute a signature from the payload + secret
2. You send it in a header
3. The consumer verifies it before processing

This protects against:

* payload tampering
* replay attacks
* spoofed requests

IP allowlists alone are not sufficient.

### Acknowledge fast, process later

Webhook endpoints should:

* respond quickly (200 OK)
* enqueue processing internally
* avoid long-running logic in the request cycle

From the sender’s perspective, a slow endpoint is indistinguishable from a failing one.

From the receiver’s perspective, blocking logic increases failure risk.

Asynchronous handling on both sides keeps the system resilient.

### Observability: make failures visible

If consumers cannot see:

* delivery attempts
* failures
* last successful event

they will open support tickets.

Provide:

* a webhook delivery log
* timestamps
* response status codes
* retry state

This reduces support load and builds trust.

### Documentation: the integration is the product

A webhook without documentation is not an integration.

At minimum, document:

* available events
* payload schemas
* retry behavior
* security mechanism
* example consumers

Clear webhook docs reduce integration time dramatically — and are often the difference between “we tried” and “we shipped.”

### Final note

Webhooks are not a checkbox feature. They are a long-term contract with external systems you do not control.

Design them as if you’ll have to support them for years — because you will.

