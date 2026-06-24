---
name: "dddv2"
description: "Domain-Driven Design tactical and strategic patterns: bounded contexts, aggregates, entities, value objects, domain events, ubiquitous language."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when modeling business logic for a DOMAIN service. Activate when the TSA service.type=DOMAIN, or the spec enumerates aggregates, invariants, or a ubiquitous language. Resolve the actual language/framework from the TSA - DDD patterns are stack-independent.

## Procedure
1. Read tsa.domain (bounded_context, aggregates, value_objects, invariants, ubiquitous_language) as the model source of truth.
2. Define one aggregate per consistency boundary; expose behavior through the aggregate root only.
3. Model invariants inside the aggregate; never let a service mutate internal entities directly.
4. Represent immutable concepts as value objects; give them equality by value, not identity.
5. Emit domain events for state changes other contexts care about; name them in past tense (PasswordReset, OtpIssued).
6. Keep the domain layer free of framework/persistence annotations where the stack allows (hexagonal core).
7. Map each ubiquitous-language term to exactly one type/name in code; do not invent synonyms.

## Patterns
- Aggregate root as transactional + consistency boundary
- Value object for any concept defined by its attributes (Money, OperationContext, PasswordStrength)
- Domain events for cross-context integration
- Repository per aggregate root (not per table)
- Anti-corruption layer at context boundaries

## Anti-Patterns
- Anemic domain model (logic in services, data in entities)
- Aggregates that span multiple consistency boundaries
- Leaking persistence/ORM concerns into the domain core
- One repository per entity instead of per aggregate root
- Reusing the same entity across bounded contexts

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): ddd_model_validator (checks aggregate boundaries), ubiquitous_language_linter

## Validation Rules
- Every aggregate in tsa.domain.aggregates has a root type and enforced invariants
- No domain type imports a persistence/web framework package
- Each ubiquitous_language term maps to exactly one code symbol
- Domain events exist for each externally-observable state change

## RAG Sources
- ddd-reference (Evans/Vernon patterns)
- tsa.domain section
- spec acceptance criteria
