# skills

Reusable agent skills (Anthropic `SKILL.md` format) for the SDLC microservice-generation workflow.
Each skill is a folder containing a `SKILL.md` with frontmatter (`name`, `description`, `version`,
`created`, `updated`) and body sections: **When to Use · Procedure · Patterns · Anti-Patterns ·
Tool Selection · Validation Rules · RAG Sources**.

Skills are stack-specific by design; agents stay tech-agnostic and activate the right skill from the
TSA (e.g. `messaging.broker = Kafka` → kafkav2 skill).

## Catalog

### Cross-cutting / framework
- `adr-microservices-blueprintv2` — architecture decision blueprint (stack resolved from the TSA)
- `java17-springboot3-best-practicesv2` — JVM/Spring Boot coding standards (Java 17 / Spring Boot 3.x)
- `build-toolchain-alignmentv2` — align the build/runtime toolchain to the TSA version
- `domain-service-coreframework-guidelinesv2` — domain-service core-framework rules
- `legacy-sql-to-modern-jpa-transformationv2` — legacy SQL → JPA migration
- `compilation-error-triagev2` — build/compile error triage
- `spring-boot-3-test-stabilizationv2` — test stabilization (Spring Boot 3.x / Java 17)
- `microservice-iteration-handoffv2` — iteration handoff

### Application agent
- `dddv2` · `rest-api-designv2` · `spring-bootv2` · `event-modelingv2` · `testingv2`

### Messaging agent
- `kafkav2` · `aws-sqsv2` · `azure-service-busv2` · `event-contractv2` · `outbox-patternv2`

### Database agent
- `postgresqlv2` · `oraclev2` · `mongodbv2` · `jpav2` · `flywayv2` · `data-modelingv2`

### Security agent
- `oauth2v2` · `jwtv2` · `oidcv2` · `zero-trustv2` · `secrets-managementv2`
