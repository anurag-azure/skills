---
name: "azure-service-bus"
description: "Azure Service Bus messaging: queues vs topics/subscriptions, sessions for ordering, dead-lettering, duplicate detection, and peek-lock processing."
version: 1
created: "2026-06-09"
updated: "2026-06-09"
---
## When to Use
Use when tsa.technology.messaging.broker = Azure Service Bus. Activate for enterprise messaging on Azure.

## Procedure
1. Choose queues (point-to-point) vs topics+subscriptions (pub/sub) from the integration design.
2. Use sessions when ordered, stateful processing per key is required.
3. Use peek-lock (not receive-and-delete) so failures redeliver; complete/abandon explicitly.
4. Enable duplicate detection where producers may resend.
5. Configure dead-lettering + max delivery count for poison messages.
6. Externalize namespace/connection via Managed Identity; never embed connection strings.

## Patterns
- Topic + subscription filters for pub/sub fan-out
- Sessions for ordered per-key processing
- Peek-lock with explicit complete/abandon
- Dead-letter + max delivery count
- Duplicate detection window

## Anti-Patterns
- Receive-and-delete for critical messages (loss on failure)
- Connection strings in code/config
- Ignoring max delivery count (poison loops)
- Assuming ordering without sessions
- Single subscription doing unrelated work

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): servicebus_generator, servicebus_validator

## Validation Rules
- Peek-lock used for at-least-once delivery
- DLQ + max delivery count configured
- Managed Identity used (no connection strings in code)
- Sessions used where ordering required

## RAG Sources
- azure-service-bus-docs
- azure-messaging-patterns
