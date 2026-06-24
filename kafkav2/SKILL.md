---
name: "kafkav2"
description: "Apache Kafka producer/consumer design: topics, partitions, consumer groups, delivery semantics, DLQ, and transactional outbox integration."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when tsa.technology.messaging.broker = Kafka. Activate for event streaming / high-throughput async messaging.

## Procedure
1. Resolve broker config, topic naming, and serialization (Avro/JSON/Protobuf) from the TSA/spec.
2. Design topic + partition key for ordering guarantees that matter (per-entity ordering).
3. Use consumer groups for scaling; make consumers idempotent (keyed by event id).
4. Choose delivery semantics: at-least-once by default; enable idempotent/transactional producer for exactly-once-in-effect.
5. Add a dead-letter topic + retry policy for poison messages.
6. Integrate with the transactional outbox to avoid dual-write between DB and Kafka.
7. Externalize bootstrap servers, group id, and credentials; never hardcode.

## Patterns
- Consumer group for horizontal scaling
- Transactional outbox -> Kafka (no dual write)
- Dead-letter topic + bounded retries
- Idempotent consumer keyed by event id
- Partition key = aggregate id for per-entity ordering

## Anti-Patterns
- Dual write (DB commit + Kafka publish without outbox)
- Assuming global ordering across partitions
- Auto-commit offsets before processing completes
- Unbounded in-place retries (head-of-line blocking)
- Hardcoded bootstrap servers/credentials

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): kafka_generator, kafka_validator

## Validation Rules
- Producers writing critical events use outbox (no dual write)
- Every consumer is idempotent
- DLQ + retry policy defined for each topic
- Partition key chosen for required ordering

## RAG Sources
- kafka-docs
- spring-kafka-docs
- confluent-patterns
