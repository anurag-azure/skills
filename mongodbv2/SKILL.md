---
name: "mongodbv2"
description: "MongoDB document modeling: embedding vs referencing, indexing, schema validation, aggregation pipelines, and transaction boundaries."
version: 1
created: "2026-06-09T23:28:12+05:30"
updated: "2026-06-12T01:10:56+05:30"
---
## When to Use
Use when tsa.technology.database.type resolves to MongoDB/DocumentDB. Activate for document persistence.

## Procedure
1. Model documents around access patterns; embed for read-together data, reference for large/independent data.
2. Define indexes (single/compound/text) matching queries; watch the index working-set size.
3. Apply JSON schema validation rules on collections to enforce structure.
4. Use aggregation pipelines for analytics/transforms; avoid pulling data to the app to aggregate.
5. Bound document growth; avoid unbounded arrays that blow the 16MB doc limit.
6. Use multi-document transactions only when truly needed; prefer single-document atomicity.

## Patterns
- Embed read-together data; reference large/shared data
- Compound indexes for query+sort
- Schema validation on collections
- Aggregation pipeline for server-side transforms
- Single-document atomicity by design

## Anti-Patterns
- Normalizing like a relational DB (excessive references)
- Unbounded arrays in documents
- Missing indexes -> collection scans
- Overusing multi-doc transactions
- Storing huge blobs inside documents

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): mongo_schema_designer, index_advisor

## Validation Rules
- Document model matches access patterns
- Indexes cover top queries + sorts
- Schema validation enabled
- No unbounded array growth

## RAG Sources
- mongodb-docs
- mongo-data-modeling-guide
