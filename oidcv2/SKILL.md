---
name: "oidcv2"
description: "OpenID Connect authentication on top of OAuth2: ID tokens, userinfo, discovery, and login/logout flows."
version: 1
created: "2026-06-09T03:34:24+05:30"
updated: "2026-06-12T03:34:24+05:30"
---
## When to Use
Use when authentication (identity) is required via an OIDC provider (Cognito/Keycloak/Azure AD). Activate with oauth2 for login flows.

## Procedure
1. Use OIDC discovery (.well-known/openid-configuration) to resolve endpoints and JWKS.
2. Authenticate users via Authorization Code + PKCE; obtain and validate the ID token.
3. Validate ID-token claims (iss, aud, exp, nonce) and the at_hash where present.
4. Fetch profile data from the userinfo endpoint rather than overloading the ID token.
5. Implement RP-initiated logout / back-channel logout per the provider.
6. Map OIDC claims (sub, roles/groups) to application identity and authorities.

## Patterns
- Discovery-driven endpoint/JWKS resolution
- Auth Code + PKCE login
- ID-token + nonce validation
- Userinfo for profile data
- RP-initiated logout

## Anti-Patterns
- Treating the access token as proof of identity
- Skipping nonce/at_hash validation
- Hardcoding endpoints instead of discovery
- Stuffing all profile data into the ID token
- No logout/session-termination flow

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): oidc_config_generator, id_token_validator

## Validation Rules
- ID token validated (iss/aud/exp/nonce)
- Discovery used for endpoints/JWKS
- Identity claims mapped to authorities
- Logout flow implemented

## RAG Sources
- openid-connect-core
- provider-docs (cognito/keycloak/azuread)
