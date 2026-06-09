---
name: "data-modeling"
description: "Logical/physical data modeling: normalization, keys, relationships, naming, and translating a legacy schema to the target model."
version: 1
created: "2026-06-09"
updated: "2026-06-09"
---
## When to Use
Use when designing the persistence model or migrating a legacy schema. Activate in the design/infrastructure phase regardless of DB engine.

## Procedure
1. Build a logical model from the domain (entities, attributes, relationships, cardinality).
2. Normalize to remove redundancy; denormalize deliberately only for proven read performance.
3. Choose natural vs surrogate keys consistently; define referential integrity.
4. Map legacy tables/columns to the target model; preserve required semantics, modernize naming.
5. Apply consistent naming conventions (tables, columns, constraints, indexes).
6. Hand the physical model to the engine-specific skill (postgresql/oracle/mongodb) + migration skill.

## Patterns
- Logical-then-physical modeling
- Surrogate keys + enforced referential integrity
- Deliberate, measured denormalization
- Legacy-to-target mapping table
- Consistent naming conventions

## Anti-Patterns
- Modeling straight from UI/screens
- Over-normalization that cripples reads
- Inconsistent keys/naming across tables
- Dropping legacy semantics during migration
- No referential integrity

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): erd_generator, legacy_schema_mapper

## Validation Rules
- Logical model covers all domain entities
- Keys + referential integrity defined
- Legacy mapping preserves required semantics
- Naming conventions consistent

## RAG Sources
- data-modeling-reference
- tsa.domain
- legacy schema artifacts
