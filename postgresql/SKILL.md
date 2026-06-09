---
name: "postgresql"
description: "PostgreSQL design and usage: schema design, indexing, constraints, transactions/isolation, JSONB, and connection pooling."
version: 1
created: "2026-06-09"
updated: "2026-06-09"
---
## When to Use
Use when tsa.technology.database.type/vendor resolves to PostgreSQL. Activate for relational persistence on Postgres.

## Procedure
1. Design normalized schemas; add constraints (PK/FK/UNIQUE/CHECK/NOT NULL) to enforce invariants in the DB.
2. Index for actual query patterns; use partial/expression/GIN (JSONB) indexes where appropriate.
3. Choose isolation level deliberately; understand MVCC and avoid long-running transactions.
4. Use JSONB for semi-structured data, but keep relational columns for queried fields.
5. Configure HikariCP pool sizing to the workload; set statement/lock timeouts.
6. Put all DDL under the migration tool (see flyway/liquibase), never manual prod changes.

## Patterns
- Constraints enforce invariants at the DB layer
- Targeted indexes from query patterns
- JSONB for flexible attributes + GIN index
- Connection pool sizing + timeouts
- Read-only transactions for queries

## Anti-Patterns
- Indexing everything / no indexes at all
- Long-running or idle-in-transaction connections
- Storing all data as JSONB with no constraints
- Manual schema changes outside migrations
- Ignoring N+1 query patterns

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): postgres_ddl_generator, query_plan_analyzer

## Validation Rules
- Invariants backed by DB constraints
- Indexes match top query patterns
- All DDL is migration-managed
- Pool + timeouts configured

## RAG Sources
- postgresql-docs
- use-the-index-luke
- tsa.technology.database
