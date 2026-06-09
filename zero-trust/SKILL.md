---
name: "zero-trust"
description: "Zero Trust security posture: never trust/always verify, least privilege, strong identity, mTLS, network segmentation, and continuous verification."
version: 1
created: "2026-06-09"
updated: "2026-06-09"
---
## When to Use
Use when the architecture requires defense-in-depth beyond perimeter security. Activate for cross-service and gateway security design.

## Procedure
1. Authenticate and authorize every request at every hop - no implicit trust by network location.
2. Apply least-privilege identities (per-service principals, scoped tokens, narrow IAM roles).
3. Use mTLS for service-to-service traffic; verify both peers.
4. Segment the network; default-deny and explicitly allow required flows.
5. Validate device/workload identity and token freshness continuously, not just at login.
6. Centralize policy (authorization service / OPA) and log all access decisions for audit.

## Patterns
- Per-request authN/authZ at every hop
- Least-privilege scoped identities
- mTLS between services
- Default-deny network segmentation
- Centralized policy + audit logging

## Anti-Patterns
- Trusting requests because they are "internal"
- Broad/shared service credentials
- Plaintext service-to-service traffic
- Perimeter-only security (hard shell, soft center)
- Authorize-once-then-trust for the whole session

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): policy_generator (OPA), mtls_config_validator

## Validation Rules
- No implicit network trust in the design
- Service identities least-privilege
- mTLS configured between services
- Access decisions centralized + logged

## RAG Sources
- nist-sp-800-207
- zero-trust-architecture-guides
