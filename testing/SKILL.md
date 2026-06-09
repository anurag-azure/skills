---
name: "testing"
description: "Application test strategy: unit, slice, and integration testing; coverage targets; test data; mocking discipline. Framework resolved from the TSA."
version: 1
created: "2026-06-09"
updated: "2026-06-09"
---
## When to Use
Use during the tests phase or whenever code is added that needs coverage. Activate for any service with business logic. Resolve the test framework from tsa.testing / tsa.technology.

## Procedure
1. Read tsa.testing (frameworks, coverage_target, focus_areas) and target those first.
2. Write unit tests for business logic (strategies, services, validators) with mocks for collaborators.
3. Use slice tests for controllers and persistence layers per the stack idiom.
4. Add integration tests for end-to-end flows using ephemeral infra (in-memory/containers).
5. Name tests method_scenario_expectedBehavior; assert behavior, not implementation.
6. Enforce the coverage target in the build; never skip tests in CI.

## Patterns
- Test pyramid (many unit, fewer integration)
- Slice tests per layer
- AAA / given-when-then structure
- Ephemeral test infra (Testcontainers/in-memory)
- Deterministic tests (no sleeps, no shared mutable state)

## Anti-Patterns
- Asserting on internal implementation details
- Skipping/ignoring tests to make CI pass
- Loading the full app context for pure unit tests
- Order-dependent or time-dependent tests
- Mocking value objects / types you own trivially

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): coverage_reporter, test_gap_analyzer

## Validation Rules
- Coverage meets tsa.testing.coverage_target
- Each focus_area in TSA has tests
- No @Disabled/skip in committed tests
- Tests pass deterministically in CI

## RAG Sources
- junit5-docs
- mockito-docs
- testcontainers-docs
- tsa.testing
