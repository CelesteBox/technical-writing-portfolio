### Integrations · APIs · Webhooks (EN)

# Webhooks: Design, Security, and Recommended Patterns for a SaaS That Takes Integrations Seriously

Webhooks are deceptively simple. If your SaaS exposes webhooks, you are designing an external contract. 

On paper, they’re just HTTP callbacks: *“something happened, here’s the payload.”* In practice, poorly designed webhooks create brittle integrations and support tickets that never quite die.


### What webhooks are

A webhook is an **event-driven push mechanism**. Your system sends an HTTP request to a customer-defined endpoint when a specific event occurs.

Key implications:
* Delivery is **asynchronous**
* You do **not control** the receiving system
* Failure is not exceptional but normal

They are not APIs. They invert control. And that inversion is where most problems start.

### Designing webhook events

A common mistake is to model webhook events around internal actions:

> `user_created`, `invoice_generated`, `subscription_updated`

Instead, the payload must represent **facts**, not instructions.

Good webhook payloads:

* Describe *what happened*
* Include stable identifiers
* Avoid leaking internal implementation details

Example:

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

Avoid payloads that imply what the consumer should *do*. That is in consumers' hands.

### Version your webhook schema

Webhook consumers often process events automatically. Breaking changes are costly.

Recommended practices:

* Version the event schema explicitly (`v1`, `v2`)
* Keep old versions always active during a deprecation window
* Document all differences clearly

Consumers won't just update. That's not in your control, so better not to assume it.

### Delivery guarantees

You cannot guarantee delivery -networks fail, endpoints go down. What you actually *can* guarantee:

* At-least-once delivery
* Retry behavior
* Ordering semantics (or lack thereof)

Best practices:

* Retry with exponential backoff
* Retry only on network / 5xx errors
* Stop retrying eventually and surface failures

And document it explicitly: ambiguity here causes integration bugs that are painful to debug.

### Idempotency

Because retries exist, you should expect **duplicate events**.

Include:

* A unique event ID
* A consistent event structure

Consumers should be able to ignore duplicates safely. And your documentation should say this clearly.

If your webhook system *doesn’t* generate duplicates, consumers should not rely on that.

### Security

Webhooks are an attack surface because you send data to arbitrary endpoints. That is risky by definition.

Minimum security baseline:

* HTTPS only
* Request signing (HMAC with shared secret)
* Timestamp + signature validation

Example flow:

1. You compute a signature from the payload + secret
2. You send it in a header
3. The consumer verifies it before processing

This will protect against:

* Payload tampering
* Replay attacks
* Spoofed requests

IP allowlists alone are not sufficient.

### Response time

Webhook endpoints should:

* Respond quickly (200 OK)
* Enqueue processing internally
* Avoid long-running logic in the request cycle

From the sender’s perspective, a slow endpoint is indistinguishable from a failing one.
From the receiver’s perspective, blocking logic increases failure risk.

So asynchronous handling on both sides keeps the system resilient.

### Observability

Consumers will open support tickets if they cannot see:

* Delivery attempts
* Failures
* Last successful event

So provide:

* A webhook delivery log
* Timestamps
* Response status codes
* Retry state

It will reduce support load and build trust.

### Documentation

The integration *is* the product. Reduce integration time by documenting:

* Available events
* Payload schemas
* Retry behavior
* Security mechanism
* Example consumers


### Final note

Webhooks are a long-term contract with external systems you do not control.

Design them as if you’ll have to support them for years — because you will.

