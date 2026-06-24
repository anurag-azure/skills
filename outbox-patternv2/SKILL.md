---
name: "outbox-patternv2"
description: "Transactional Outbox pattern to guarantee reliable event publishing without dual writes between a database and a message broker."
version: 1
created: "2026-06-09T23:28:12+05:30"
updated: "2026-06-12T01:10:56+05:30"
---
## When to Use
Use whenever a state change must be persisted AND an event published atomically. Activate for any messaging broker when the service owns persistence.

## Procedure
1. Write the domain change and an outbox row in the SAME local DB transaction.
2. Run a relay/poller (or CDC) that reads unsent outbox rows and publishes to the broker.
3. Mark rows as sent only after broker acknowledgement; make publishing idempotent (event id).
4. Use the event id as the dedup/idempotency key on the consumer side.
5. Add retry with backoff and a poison-row strategy for permanently failing events.
6. Index the outbox on (status, created_at) for efficient polling.

## Patterns
- Single local transaction for state + outbox row
- Relay/poller or CDC (Debezium) to publish
- Idempotent publish keyed by event id
- At-least-once delivery + idempotent consumers
- Ordered relay by aggregate id when ordering matters

## Anti-Patterns
- Dual write (commit DB then publish separately)
- Publishing inside the request thread before commit
- Deleting outbox rows before broker ack
- No dedup key (duplicates on retry)
- Unindexed outbox table (slow polling)

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): outbox_scaffolder, dual_write_detector

## Validation Rules
- No code path commits DB then publishes without the outbox
- Outbox row written in the same tx as the change
- Publish is idempotent (event id)
- Relay marks sent only after ack

## RAG Sources
- microservices-io-outbox
- debezium-docs
- kafka/sqs/servicebus skill
