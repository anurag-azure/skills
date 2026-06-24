---
name: "flywayv2"
description: "Flyway database migrations: versioned/ repeatable scripts, naming conventions, idempotency, and CI integration for schema evolution."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when tsa.technology.database.migration_tool = Flyway. Activate whenever schema/DDL changes are produced.

## Procedure
1. Place migrations under the resolved resources path (e.g. db/migration); never change applied scripts.
2. Name versioned scripts V<n>__<description>.sql in strict increasing order; R__ for repeatable.
3. Make each migration forward-only and reviewable; add a new migration to fix a mistake.
4. Keep migrations idempotent where possible (IF NOT EXISTS) and environment-agnostic.
5. Run flyway:validate and migrate in CI; fail the build on checksum mismatch.
6. Separate DDL from large data backfills; keep migrations fast and lock-aware.

## Patterns
- Versioned forward-only scripts
- Repeatable scripts for views/procs
- CI validate + migrate gate
- New migration to correct a prior one (never edit applied)
- Lock-aware, fast migrations

## Anti-Patterns
- Editing an already-applied migration (checksum break)
- Out-of-order/duplicate version numbers
- Manual prod schema changes outside Flyway
- Mixing huge data backfills into DDL migrations
- Skipping flyway:validate in CI

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): flyway_migration_generator, migration_lint

## Validation Rules
- Scripts follow V<n>__ / R__ naming
- No applied script is ever edited
- CI runs validate + migrate
- DDL and data backfills separated

## RAG Sources
- flyway-docs
- tsa.artifacts.database_migrations
