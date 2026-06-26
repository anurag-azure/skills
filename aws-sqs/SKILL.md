---
name: "aws-sqs"
description: "AWS SQS messaging: standard vs FIFO queues, visibility timeout, DLQ/redrive, long polling, and Spring Cloud AWS integration."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-26"
---
## When to Use
Use when tsa.technology.messaging.broker = SQS. Activate for fire-and-forget or work-queue async patterns on AWS.

## Procedure
1. Resolve queue type from requirements: FIFO for ordering+dedup, Standard for max throughput.
2. Set visibility timeout >= max processing time; extend for long handlers.
3. Configure a dead-letter queue with a sensible maxReceiveCount redrive policy.
4. Use long polling (WaitTimeSeconds) to cut empty receives and cost.
5. Make consumers idempotent; for FIFO use MessageDeduplicationId/MessageGroupId.
6. Integrate via Spring Cloud AWS (SqsTemplate / @SqsListener) when the stack is Spring.
7. Externalize queue URLs/region/credentials; use IAM roles, never static keys in code.

## Patterns
- DLQ + redrive policy for poison messages
- FIFO + MessageGroupId for ordered per-key processing
- Long polling to reduce cost/empty receives
- Idempotent handlers (dedup id)
- Fire-and-forget for non-critical notifications

## Anti-Patterns
- Visibility timeout shorter than processing time (duplicate processing)
- No DLQ (infinite redelivery of poison messages)
- Static AWS keys committed in code/config
- Relying on Standard-queue ordering
- Short polling everywhere (cost/throttling)

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): sqs_generator, sqs_config_validator

## Validation Rules
- Every queue has a DLQ + redrive policy
- Visibility timeout >= handler max runtime
- Consumers idempotent (or FIFO dedup configured)
- No static credentials; IAM role used

## RAG Sources
- aws-sqs-docs
- spring-cloud-aws-docs
