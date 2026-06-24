---
name: "oauth2v2"
description: "OAuth2 authorization: grant-type selection, resource-server vs client roles, scopes, token validation, and Spring Security integration."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when tsa.technology.security.type includes OAuth2. Activate for authorization wiring on any API-exposing service.

## Procedure
1. Resolve the provider and flow from the TSA (e.g. Authorization Code + PKCE; client credentials for service-to-service).
2. Configure the service as an OAuth2 resource server validating access tokens against the issuer.
3. Define scopes/authorities and enforce them at method or endpoint level.
4. Validate issuer, audience, expiry, and signature on every request; cache JWKS.
5. Never store client secrets in code; source them from a secrets manager.
6. Keep the service stateless; do not hold server-side sessions for token auth.

## Patterns
- Authorization Code + PKCE for user flows
- Client credentials for service-to-service
- Resource server validating issuer/audience
- Scope/authority-based endpoint authorization
- JWKS caching with rotation

## Anti-Patterns
- Implicit grant (deprecated)
- Client secrets in code/config
- Skipping audience/issuer validation
- Stateful sessions alongside token auth
- Trusting tokens without signature verification

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): oauth2_config_generator, token_flow_validator

## Validation Rules
- Correct grant type per client type
- Issuer/audience/expiry/signature validated
- No secrets hardcoded
- Scopes enforced at endpoints

## RAG Sources
- oauth2-rfc6749
- spring-security-oauth2-docs
- tsa.technology.security
