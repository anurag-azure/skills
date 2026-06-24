---
name: "secrets-managementv2"
description: "Secrets management: externalized secrets, vaults (Azure Key Vault/AWS Secrets Manager/Vault), rotation, and least-privilege access."
version: 1
created: "2026-06-09T23:28:12+05:30"
updated: "2026-06-12T01:10:56+05:30"
---
## When to Use
Use for any service handling credentials, keys, or connection strings. Activate during configuration/crosscutting and security phases.

## Procedure
1. Externalize every secret to the platform vault named in the TSA (Key Vault/Secrets Manager/Vault).
2. Inject secrets at runtime via managed identity/IAM role; never bake them into images or config files.
3. Enable automatic rotation; design the app to pick up rotated secrets without redeploy.
4. Scope access least-privilege per service principal; audit secret access.
5. Redact secrets/PII from logs and error messages.
6. Keep no secrets in source control; scan commits for leaked credentials.

## Patterns
- Vault-backed runtime secret injection
- Managed identity / IAM role access (no static keys)
- Automatic rotation + hot reload
- Least-privilege per-service access
- Secret/PII log redaction

## Anti-Patterns
- Secrets in code, config files, or images
- Static long-lived credentials
- Sharing one secret across many services
- No rotation
- Logging secrets or connection strings

## Tool Selection
- Generic: read_file, write_file, command_line, skills
- Intended (roadmap): secret_scanner, vault_config_generator

## Validation Rules
- No secret in source/config/images
- Vault + managed identity used
- Rotation enabled
- Secrets redacted from logs

## RAG Sources
- azure-key-vault-docs
- aws-secrets-manager-docs
- hashicorp-vault-docs
