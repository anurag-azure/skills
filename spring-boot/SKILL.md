---
name: "spring-boot"
description: "Spring Boot 3.x/4.x implementation idioms: dependency injection, configuration, web/data starters, actuator, virtual threads, production-ready patterns."
version: 1
created: "2026-06-09"
updated: "2026-06-09"
---
## When to Use
Use when tsa.technology.application.framework = Spring Boot. Activate during the implementation phase for a JVM service. Defer to java21-springboot4-best-practices for language-level rules.

## Procedure
1. Resolve Spring Boot version + starters from the TSA artifacts.build_files (pom.xml/build.gradle).
2. Use constructor injection with final fields; never field injection.
3. Externalize config via @ConfigurationProperties; bind profiles (dev/staging/prod) from application.yml.
4. Expose health/metrics via Actuator; wire Micrometer per tsa.technology.observability.
5. Use @RestControllerAdvice for global exception handling.
6. Enable virtual threads for I/O-bound endpoints when supported (spring.threads.virtual.enabled).
7. Generate the @SpringBootApplication entry point even if the blueprint omits it.

## Patterns
- Constructor injection + @RequiredArgsConstructor
- Profile-based externalized configuration
- @RestControllerAdvice global error mapping
- Actuator health/readiness/liveness probes
- Starter-driven dependency management via parent POM/BOM

## Anti-Patterns
- Field injection (@Autowired on fields)
- Hardcoded URLs/credentials/timeouts
- Business logic in @Controller
- Mixing reactive and MVC stacks inconsistently
- Catching Exception and swallowing it

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): spring_boot_scaffolder, bean_wiring_validator

## Validation Rules
- Entry point annotated @SpringBootApplication exists
- All beans use constructor injection
- No secrets/URLs hardcoded
- Actuator + Micrometer configured per TSA observability

## RAG Sources
- spring-boot-docs
- spring-framework-docs
- java21-springboot4-best-practices skill
