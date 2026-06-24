---
name: "jwtv2"
description: "JWT handling: claims design, signature algorithms, validation, expiry/rotation, and safe storage/transport of tokens."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when tokens are JWTs (stateless auth). Activate alongside oauth2/oidc whenever JWTs are issued or validated.

## Procedure
1. Validate signature with the correct algorithm (prefer asymmetric RS/ES); reject alg=none.
2. Verify standard claims: iss, aud, exp, nbf, iat; reject on any failure.
3. Keep tokens short-lived; use refresh tokens for renewal; support key rotation via JWKS/kid.
4. Put only non-sensitive, minimal claims in the token; never secrets/PII beyond what is needed.
5. Map claims to application authorities/roles deterministically.
6. Transport via Authorization: Bearer; never log full tokens (mask).

## Patterns
- Asymmetric signing + JWKS/kid rotation
- Short-lived access + refresh tokens
- Strict claim validation (iss/aud/exp)
- Claims-to-authorities mapping
- Token masking in logs

## Anti-Patterns
- Accepting alg=none or symmetric secret shared with clients
- Long-lived non-expiring tokens
- Storing sensitive PII/secrets in claims
- Logging raw tokens
- Skipping nbf/exp checks

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): jwt_validator, claims_linter

## Validation Rules
- Signature + iss/aud/exp validated
- alg=none rejected
- Tokens short-lived + rotatable
- No tokens/PII leaked to logs

## RAG Sources
- jwt-rfc7519
- jose-cookbook
- spring-security-docs
