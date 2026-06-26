---
name: "oracle"
description: "Oracle Database usage: schema and sequence design, indexing, PL/SQL boundaries, hints discipline, partitioning, and JDBC/pool tuning."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-26"
---
## When to Use
Use when tsa.technology.database.vendor = Oracle. Activate for relational persistence on Oracle.

## Procedure
1. Design schemas with appropriate sequences/identity columns and constraints.
2. Index for query patterns; consider bitmap vs b-tree per cardinality; use function-based indexes when needed.
3. Keep business logic in the application; use PL/SQL only for set-based DB-local operations.
4. Use bind variables everywhere to enable cursor sharing and prevent SQL injection.
5. Consider partitioning for very large tables; align partition key with access patterns.
6. Tune the JDBC fetch size and connection pool; set sensible timeouts.

## Patterns
- Bind variables for cursor sharing
- Partitioning for large tables
- Function-based / bitmap indexes by cardinality
- Set-based SQL over row-by-row processing
- Migration-managed DDL

## Anti-Patterns
- Literal SQL (no bind variables) causing hard parses + injection risk
- Heavy business logic in PL/SQL
- Optimizer hints sprinkled to mask bad design
- Row-by-row (slow-by-slow) processing
- Manual prod schema changes

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): oracle_ddl_generator, plsql_linter

## Validation Rules
- Bind variables used throughout
- Indexes match query patterns
- DDL migration-managed
- No unmanaged optimizer hints

## RAG Sources
- oracle-database-docs
- oracle-sql-tuning-guide
