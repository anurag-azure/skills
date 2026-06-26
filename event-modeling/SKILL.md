---
name: "event-modeling"
description: "Event modeling for event-driven systems: commands, events, read models, event storming outputs, and event-flow design across services."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-26"
---
## When to Use
Use when the design involves asynchronous flows, domain events, or event-driven integration. Activate when tsa.technology.messaging.enabled = true or the spec describes event flows.

## Procedure
1. Identify commands (intent), events (facts), and read models (projections) from the spec flows.
2. Define the event flow per use case: command -> aggregate -> event(s) -> subscribers/read models.
3. Name events in past tense; version each event schema from day one.
4. Decide delivery semantics (at-least-once vs exactly-once) and idempotency strategy per consumer.
5. Separate internal domain events from published integration events.
6. Document the event catalogue (producer, schema, consumers, ordering/partition key).

## Patterns
- Command/Event/Read-model separation (CQRS-friendly)
- Event storming to derive flows
- Integration events distinct from domain events
- Idempotent consumers keyed by event id
- Schema versioning + backward compatibility

## Anti-Patterns
- Using events as RPC (request/response over a bus)
- Present/future-tense event names
- Publishing internal domain events directly to other contexts
- Unversioned event payloads
- Assuming ordered, exactly-once delivery without design for it

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): event_flow_modeler, event_catalogue_generator

## Validation Rules
- Every async flow has command -> event -> consumer mapped
- All events are past-tense and versioned
- Each consumer has a defined idempotency key
- Integration vs domain events are clearly separated

## RAG Sources
- event-storming-guide
- cqrs-es-patterns
- tsa.execution_chain
