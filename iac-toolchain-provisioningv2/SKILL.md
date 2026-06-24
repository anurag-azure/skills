---
name: "iac-toolchain-provisioningv2"
description: "Technology-agnostic rule for provisioning the IaC validation CLI on demand (not installed by default) and running BASIC OFFLINE validation in the terminal WITHOUT any cloud login — resolved from cloud_provider + iac_format. Detects the OS/package manager, installs the right tool (terraform, bicep, cfn-lint, spectral, deck, helm/kubeconform...), then runs login-free checks (e.g. terraform fmt -check; terraform init -backend=false; terraform validate)."
version: 1
created: "2026-06-25"
updated: "2026-06-25"
---
## When to Use
Use before reporting an IaC artifact deployable, in ANY cloud and ANY format. The validation CLI is usually NOT
pre-installed, so this skill PROVISIONS it on demand and then runs **basic, offline, login-free** validation.
Stack-independent: resolve the tool from `cloud_provider` + `iac_format` (and the artifact itself), install it,
verify it, run it. This mirrors how the JVM build path uses `build-toolchain-alignmentv2` — but for IaC.

## Core Principle
"Validation" here means **static / offline** checks that need NO cloud credentials and touch NO real account:
syntax, formatting, schema, type, and provider-plugin resolution. NEVER `login`, NEVER `apply`/`deploy`/`plan`
against a live cloud, NEVER configure credentials, NEVER use a remote backend. If a check would require auth,
skip it and mark it `review-only` — do not fake a pass.

## ⛔ TOOL-CALL DISCIPLINE (operating constraint)
The runtime hard-terminates the agent at ~100 `command_line` calls. Stay far below it.
- READ files with the native `read_file` tool; CREATE/edit with native `write_file` — NOT shell cat/echo/heredoc.
- Use `command_line` ONLY for genuine shell needs: install a CLI, run a validator.
- ≤ ~20 `command_line` calls per invocation. BATCH installs; run each validator ONCE; never re-run for a
  different result; no sleep/poll loops. Scope every find/grep to the artifact dir; never scan from `/` or `~`.

## Procedure
1. **Resolve the tool(s)** from `cloud_provider` + `iac_format` (see matrix below).
2. **Probe first** — `command -v <tool>`; if already present and `--version` works, skip install.
3. **Detect OS + package manager** (run once):
   - Debian/Ubuntu → `apt-get`  · Alpine (musl) → `apk`  · RHEL/Fedora → `dnf`/`yum`  · macOS → `brew`
   - Detect Alpine via `/etc/alpine-release` (or `ldd --version` reporting musl).
   - Python linters → `pip`/`pipx`; Node linters → `npm`/`npx`.
4. **Install** the tool with the detected manager, or download the official static binary for the platform.
   - **Alpine/musl caution:** prefer `apk add` packages; if downloading a binary, get the musl/static build —
     a glibc-only binary fails on musl with loader/`Exec format` errors. (Same trap as glibc JDKs on Alpine.)
5. **Verify** the install (`<tool> --version`) before using it.
6. **Run the offline validators** (matrix below); capture exit code + stdout/stderr for the report.
7. **Network-locked fallback** — if no network / locked registry blocks the install, do NOT fake a pass: mark
   the gate `review-only` (or `BLOCKED-ON-ENV`), name the exact missing tool, and fall back to the
   documented-API audit from `iac-deployability-verificationv2`.

## Tool + Offline-Validation Matrix (resolve from cloud_provider × iac_format)
- **Terraform / OpenTofu** (any cloud):
  install: `apt-get install -y terraform` (HashiCorp apt repo) · `apk add terraform` · or download the
  `terraform_<ver>_linux_<arch>.zip` from releases.hashicorp.com and unzip to PATH (`tofu` for OpenTofu).
  validate (no login): `terraform fmt -check -recursive` ; `terraform init -backend=false` (downloads
  providers from the registry — network, but NO cloud creds) ; `terraform validate`.
  If `init` cannot reach the registry, run `terraform fmt -check` + `terraform validate -no-color` after a
  best-effort `init -backend=false`, and mark provider-dependent checks review-only.
- **Bicep** (Azure):
  install: standalone `bicep` CLI binary (`az bicep install` if az present, else download `bicep-linux-x64`).
  validate (no login): `bicep build <file>.bicep` (compiles to ARM JSON) ; `bicep lint <file>.bicep`.
- **ARM templates** (Azure JSON):
  install: arm-ttk (PowerShell) via `pwsh` + the arm-ttk module, or use `bicep decompile` to sanity-check.
  validate (no login): arm-ttk `Test-AzTemplate` ; JSON schema check. (`az deployment validate` needs login → skip.)
- **CloudFormation / SAM** (AWS):
  install: `pip install cfn-lint` (and `aws-sam-cli` if SAM).
  validate (no login): `cfn-lint <template>` ; for SAM `sam validate --lint`. (`aws cloudformation validate-template`
  needs creds → skip.)
- **OpenAPI / API specs** (any gateway contract):
  install: `npm i -g @stoplight/spectral-cli` (or `npx`), or `pip install openapi-spec-validator`.
  validate (no login): `spectral lint <spec>` ; `openapi-spec-validator <spec>`.
- **Azure APIM policy XML**:
  no public offline semantic validator → `xmllint --noout <file>` for well-formedness, then the documented-API
  audit (APIM policy-expression surface) from `iac-deployability-verificationv2`. Mark semantic gate review-only.
- **GCP API Gateway / Apigee**:
  Terraform path → use the Terraform row. Raw OpenAPI api-config → spectral. Apigee proxy XML → `xmllint` +
  apigeelint (`npm i -g apigeelint`; `apigeelint -s <proxydir>`).
- **Kong**: `deck` CLI — `deck gateway validate` / `deck file lint` (offline).
- **Kubernetes / Gateway-API / Helm**:
  install: `helm`, `kubeconform`. validate (no cluster): `helm lint <chart>` ;
  `kubectl apply --dry-run=client -f <manifest>` ; `kubeconform -strict <manifest>`.
- **Pulumi**: offline limited — run the language compiler/type-check; `pulumi preview` needs a stack/login → skip,
  mark review-only.

## Patterns
- Probe → detect OS/pkg-mgr → install → verify `--version` → run validator → capture output.
- Always pass the login-free flags: terraform `-backend=false`; never `plan`/`apply`; no `az/aws/gcloud` auth.
- Prefer the distro package manager; fall back to the official static binary for the right libc/arch.
- One install batch, one run per validator; record exit codes for the deployability report.

## Anti-Patterns
- Assuming the CLI is pre-installed (it is not) and skipping validation.
- Logging in / configuring credentials / running `plan`/`apply`/`deploy` to "validate".
- Reporting deployable from well-formedness when a real validator could have run.
- On Alpine/musl, installing a glibc-only binary (won't execute).
- Using `command_line` for file reads/writes (burns the tool budget); blind full-filesystem scans.

## Validation Rules
- TOOL-RESOLVED: the validator matching cloud_provider × iac_format is identified.
- INSTALLED-OR-BLOCKED: tool installed and `--version` verified, OR network-locked → review-only/BLOCKED-ON-ENV.
- LOGIN-FREE: no auth/login/credentials; no plan/apply/deploy; no remote backend.
- RAN-AND-CAPTURED: each applicable validator executed once; exit code + output captured.
- HONEST-COVERAGE: each gate marked tool-verified vs review-only — never a false pass.

## RAG Sources
When use_rag is on, call `get_rag_context` to confirm the exact install command / offline-validation flags for
the resolved tool and OS, and any org-approved internal mirror/registry for air-gapped installs.
