---
name: "rest-api-designv2"
description: "REST/HTTP API design: resource modeling, URI and versioning conventions, status-code and error-contract standards, OpenAPI documentation."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when tsa.technology.api.style = REST. Activate when generating controllers/routers or an OpenAPI contract. Resolve framework specifics from the TSA (Spring MVC, Express, FastAPI, ...).

## Procedure
1. Read tsa.technology.api (style, contract, versioning, content_type, documentation).
2. Model resources as nouns; use HTTP verbs for actions; keep controllers thin (delegate to services).
3. Apply the TSA versioning rule (e.g. URI /api/v1/...); do not invent a different scheme.
4. Standardize the response envelope and error contract across all endpoints (status, code, message, data, errors).
5. Map domain/exception codes to correct HTTP status codes; never leak stack traces.
6. Annotate endpoints for the OpenAPI contract named in the TSA (operation, responses, schemas).
7. Validate request DTOs at the edge before invoking business logic.

## Patterns
- Resource-oriented URIs + verb semantics
- Consistent error envelope with correlation id
- URI versioning for breaking changes; header/content-negotiation for minor
- Idempotency keys for unsafe retried operations
- Pagination/filtering conventions for collections

## Anti-Patterns
- Verbs in URIs (/getUser) instead of resources
- Returning 200 for errors
- Leaking internal exceptions / stack traces to clients
- Inconsistent field casing or envelope between endpoints
- Business logic inside controllers

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): openapi_generator, openapi_contract_validator

## Validation Rules
- Every endpoint documented in the OpenAPI contract from the TSA
- All error responses use the standard envelope + correct HTTP status
- Versioning matches tsa.technology.api.versioning
- No controller contains business logic

## RAG Sources
- openapi-3-spec
- rest-api-style-guide
- tsa.technology.api
