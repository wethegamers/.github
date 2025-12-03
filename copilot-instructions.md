# WTG Platform – Master AI Agent Instructions

**Version**: 1.3.0  
**Last Updated**: 2025-06-26  
**Scope**: All WTG repositories

> ⚠️ **IMPORTANT**: This is the master instruction file for all WTG repositories.  
> The `/wtg` directory is NOT a git repository itself—it's a workspace containing multiple repositories.  
> Each repo's `.github/copilot-instructions.md` should reference this file for platform-wide standards.

> 📚 **AI Resources**: The `ai/` repository contains prompts, tool instructions, and guidance.  
> See `/home/seb/wtg/ai/` for reusable templates and domain-specific instructions.

---

## Quick Reference – Token-Efficient Context

### Platform Identity

| Attribute | Value |
|-----------|-------|
| **Organization** | WeTheGamers (WTG) |
| **Platform Type** | Kubernetes-based gaming infrastructure |
| **GitOps Tool** | ArgoCD (single control plane in staging) |
| **Secret Management** | HashiCorp Vault + ExternalSecrets Operator |
| **Ingress Stack** | Traefik + Tailscale (NOT nginx) |
| **Private Network** | Tailscale (`ts.wtgg.org`) |

### Cluster Architecture

| Cluster | Purpose | Platform | Status |
|---------|---------|----------|--------|
| `wtg-microk8s-dev` | Development | MicroK8s on RockPro64 | Active |
| `wtg-k3s-staging` | Staging + Control Plane | K3s on OrangePi 5 Pro | Active |
| `wtg-talos-prod` | Production | Talos Linux on Hetzner | Planned |

### Domain Structure

| Domain | Purpose | Pattern |
|--------|---------|---------|
| `wtgg.org` | Backend/Programmatic | `{app}.{env}.wtgg.org` |
| `wethegamers.org` | Public/User-Facing | `{app}.wethegamers.org` |
| `*.tail5e286b.ts.net` | Tailscale MagicDNS (Private) | `{service}-{cluster}.tail5e286b.ts.net` |
| `ts.wtgg.org` | Private DNS A records | `{hostname}.ts.wtgg.org` → Tailscale IPs |

---

## ⚠️ CRITICAL: Vault Access (READ THIS FIRST!)

**STOP! Before accessing secrets, API tokens, or credentials:**

### Vault is accessed via Tailscale - NOT port-forward!

```bash
# CORRECT - Use Tailscale MagicDNS hostname
export VAULT_ADDR="https://vault-staging.tail5e286b.ts.net"
export VAULT_TOKEN="<from one of the methods below>"

# WRONG - Do NOT use port-forward for Vault
# kubectl port-forward -n vault svc/vault-active 8200:8200  # NO!
```

### Option A: From Kubernetes Secret (Kubefirst-style)
```bash
# Root token is stored in vault-unseal-secret (like Kubefirst standard)
kubectl -n vault get secrets/vault-unseal-secret --template='{{index .data "root-token"}}' | base64 -d
```

### Option B: From Breakglass K8s Secret
```bash
# Emergency backup stored in vault-breakglass namespace
kubectl -n vault-breakglass get secret vault-breakglass-credentials -o jsonpath='{.data.ROOT_TOKEN}' | base64 -d
```

### Option C: From Local Credentials File
```bash
# Original credentials file location
grep '^ROOT_TOKEN=' /home/seb/wtg/.archive/old-docs/VAULT_CREDENTIALS_REGEN_20251117-001745.txt | cut -d= -f2
```

### Quick Vault Access:
```bash
export VAULT_ADDR="https://vault-staging.tail5e286b.ts.net"
# Use any of the three methods above:
export VAULT_TOKEN="$(kubectl -n vault get secrets/vault-unseal-secret --template='{{index .data "root-token"}}' | base64 -d)"
vault status  # Should show Sealed=false
```

### Terraform Secrets (Cloudflare, etc):
```bash
# Get Cloudflare API token for Terraform
vault kv get -field=TF_API_TOKEN secret/global/cloudflare

# Use in Terraform
export TF_VAR_cloudflare_api_token="$(vault kv get -field=TF_API_TOKEN secret/global/cloudflare)"
```

### Common Secret Paths:
| Path | Contents |
|------|----------|
| `secret/global/cloudflare` | `TF_API_TOKEN`, `API_TOKEN`, `EMAIL` |
| `secret/global/tailscale` | Tailscale API keys |
| `secret/global/digitalocean` | DO API tokens |
| `secret/global/webmin/{hostname}` | Webmin admin credentials per server |
| `secret/staging/{app}` | Staging app secrets |
| `secret/development/{app}` | Dev app secrets |

### Transit Auto-Unseal Architecture:
- **Staging Vault** auto-unseals via Transit Vault on dev cluster
- Transit Vault: `http://100.66.246.116:30200`
- Recovery keys in: `/home/seb/wtg/.archive/VAULT_TRANSIT_AUTOUNSEAL_COMPLETE_20251201.txt`

---

## Core Principles – MEMORIZE THESE

### 1. Cluster IS the Environment

**The central paradigm**: Environment specificity lives in the **cluster** and **Vault path**, NOT in namespaces or resource names.

```
✅ CORRECT                          ❌ WRONG
namespace: agis-bot                 namespace: agis-bot-staging
deployment: agis-bot                deployment: agis-bot-dev
secret key: DISCORD_TOKEN           secret key: DISCORD_TOKEN_DEV
```

### 2. Path-Based Vault Isolation

Secrets are isolated by path, not key name:

```
secret/development/agis-bot/DISCORD_TOKEN
secret/staging/agis-bot/DISCORD_TOKEN
secret/production/agis-bot/DISCORD_TOKEN
          ↑ environment      ↑ identical key
```

### 3. Portable Manifests

Application manifests are **identical** across environments. Only `destination.name` changes in ArgoCD Applications.

### 4. Standard Ingress: Traefik + Tailscale

All clusters use:
- `traefik` IngressClass → Public access (`*.{env}.wtgg.org`)
- `tailscale` IngressClass → Private access (`*.ts.wtgg.org`)

**NEVER use nginx**—it's not deployed.

---

## Documentation Index

All canonical documentation lives in `/home/seb/wtg/operations/docs/`:

### Standards
| File | Purpose |
|------|---------|
| `standards/NAMING_STANDARDS.md` | v3.1.0 naming conventions (**READ FIRST**) |

### Infrastructure
| File | Purpose |
|------|---------|
| `infrastructure/VAULT_SECRETS_STRUCTURE.md` | Vault paths & key patterns |
| `infrastructure/TAILSCALE_CONFIGURATION.md` | Private network setup |
| `infrastructure/MULTI_CLUSTER_SETUP.md` | ArgoCD multi-cluster config |
| `infrastructure/INGRESS_ARCHITECTURE.md` | Traefik + Tailscale routing |
| `infrastructure/TLS_CERTIFICATE_MANAGEMENT.md` | cert-manager configuration |

### Access & Security
| File | Purpose |
|------|---------|
| `access/GITHUB_PAT_REFERENCE.md` | GitHub token management |
| `access/KUBECONFIG_REMOTE_ACCESS.md` | Cluster access via kubeconfig |
| `access/ZERO_TRUST_ACCESS.md` | Cloudflare Access policies |

### Runbooks
| Directory | Purpose |
|-----------|---------|
| `runbooks/procedures/` | Credential rotation, bootstrapping |
| `runbooks/maintenance/` | Scheduled maintenance tasks |
| `runbooks/disaster-recovery/` | DR procedures |
| `runbooks/incidents/` | Incident response templates |

### Manuals (Comprehensive Guides)
| File | Topics |
|------|--------|
| `manuals/01-TECHNICAL-INFRASTRUCTURE-MANUAL.md` | Full platform architecture |
| `manuals/02-OPERATIONS-RUNBOOK-MANUAL.md` | Day-to-day operations |
| `manuals/03-AGIS-BOT-USER-MANUAL.md` | Discord bot configuration |
| `manuals/06-VAULT-OPERATIONS-MANUAL.md` | Vault administration |
| `manuals/07-GITOPS-APP-OF-APPS-MANUAL.md` | ArgoCD patterns |
| `manuals/08-ARGO-WORKFLOWS-CICD-MANUAL.md` | CI/CD with Argo Workflows |

---

## Repository Map

| Repository | Language | Purpose | Has Instructions |
|------------|----------|---------|------------------|
| `ai` | Markdown | AI prompts, instructions & guidance | ✅ |
| `agis-bot` | Go | Discord bot for gaming communities | ✅ |
| `ansible` | YAML | Node provisioning | ✅ |
| `branding` | Assets | Logos and style guides | ✅ |
| `charts` | Helm | Shared Helm charts | ✅ |
| `cli-utils` | Go | CLI utilities | ✅ |
| `gitops` | YAML/Kustomize | Kubernetes manifests & ArgoCD apps | ✅ |
| `gitops-catalog` | YAML | ArgoCD app catalog | ✅ |
| `gitops-template` | YAML | Kubefirst templates | ✅ |
| `manifests` | YAML | Standalone Kubernetes manifests | ✅ |
| `metrics-client` | Go | Metrics collection | ✅ |
| `nexus` | TypeScript | Platform backend services | ✅ |
| `operations` | Markdown | Documentation & runbooks | ✅ |
| `wtg-website` | Next.js | Public website | ✅ |

---

## Anti-Patterns – AVOID THESE

| ❌ Don't Do This | Why | ✅ Do This Instead |
|------------------|-----|-------------------|
| Add `-dev`/`-staging` suffix to namespaces | Breaks portability | Use same namespace name in all clusters |
| Add `_DEV`/`_PRO` suffix to Vault keys | Creates mapping complexity | Same key name, different path |
| Use `nginx` IngressClass | Not deployed | Use `traefik` or `tailscale` |
| Hardcode cluster IPs | Breaks on migration | Use DNS or Tailscale names |
| Put secrets in manifests | Security risk | Use ExternalSecrets + Vault |
| Configure apps on `192.168.100.x` | Reserved for Cilium CNI | Use pod/service CIDRs |

---

## Common Patterns

### ExternalSecret Template

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: {app}-secrets
  namespace: {app}           # NO environment suffix
spec:
  secretStoreRef:
    name: vault-kv-secret
    kind: ClusterSecretStore
  target:
    name: {app}-secrets
    creationPolicy: Owner
  data:
    - secretKey: SOME_SECRET  # K8s secret key
      remoteRef:
        key: {env}/{app}      # Vault path includes env
        property: SOME_SECRET # Same as secretKey
```

### ArgoCD Application Template

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: {app}
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/wethegamers/gitops
    path: apps/{app}/overlays/{env}
    targetRevision: HEAD
  destination:
    name: wtg-{platform}-{env}  # Cluster name determines env
    namespace: {app}
```

### Tailscale Ingress Annotation

```yaml
metadata:
  annotations:
    tailscale.com/tags: "tag:k8s-{cluster}"
spec:
  ingressClassName: tailscale
  tls:
  - hosts:
    - {service}-{cluster}     # e.g., vault-staging
```

---

## GitOps Directory Structure

```
gitops/
├── registry/
│   ├── clusters/
│   │   ├── wtg-microk8s-dev/        # Dev cluster bootstrapping
│   │   └── wtg-k3s-staging/         # Staging cluster + env apps
│   │       ├── environment-development.yaml  → wtg-microk8s-dev
│   │       ├── environment-staging.yaml      → wtg-k3s-staging
│   │       └── environment-production.yaml   → wtg-talos-prod
│   └── environments/
│       ├── development/             # Dev workloads
│       ├── staging/                 # Staging workloads
│       └── production/              # Production workloads
└── apps/
    └── {app}/
        ├── base/                    # Shared manifests
        └── overlays/
            ├── development/         # Dev-specific patches
            ├── staging/
            └── production/
```

---

## Vault Access Quick Reference

| Access Method | URL | Use Case |
|---------------|-----|----------|
| Tailscale | `https://vault-staging.tail5e286b.ts.net` | Remote CLI/UI access |
| In-Cluster | `http://vault.vault.svc:8200` | ExternalSecrets, workloads |

```bash
# Set up CLI access
export VAULT_ADDR="https://vault-staging.tail5e286b.ts.net"
export VAULT_TOKEN="hvs.xxxxx"

# Common operations
vault kv list secret/staging
vault kv get secret/staging/{app}
vault kv put secret/staging/{app} KEY=value
```

---

## When You Need More Context

1. **Platform standards**: Read `operations/docs/standards/NAMING_STANDARDS.md`
2. **Vault structure**: Read `operations/docs/infrastructure/VAULT_SECRETS_STRUCTURE.md`
3. **Cluster access**: Read `operations/docs/access/KUBECONFIG_REMOTE_ACCESS.md`
4. **Specific component**: Check the relevant manual in `operations/docs/manuals/`
5. **Troubleshooting**: Check `operations/runbooks/` for procedures

---

## Repo-Specific Instructions

Each repository has its own `.github/copilot-instructions.md` with:
- Repository-specific architecture
- Build/test/deploy commands
- Code style guidelines
- Common operations

**Always read both**:
1. This master file (platform context)
2. The repo's own instructions (local context)

---

## MCP Server Tools

When working in VS Code with Copilot, the following MCP (Model Context Protocol) servers are available:

### GitHub MCP Server (`mcp_io_github_git_*`)
Use GitHub MCP tools for repository operations:
- **Issues**: `search_issues`, `issue_read`, `issue_write`, `list_issues`
- **Pull Requests**: `list_pull_requests`, `create_pull_request`, `pull_request_read`
- **Code Search**: `search_code`, `get_file_contents`
- **Branches**: `list_branches`, `create_branch`
- **Commits**: `list_commits`, `get_commit`

Prefer these tools over manual git commands when interacting with GitHub APIs.

### Important Notes
- Always use MCP tools when available instead of running CLI commands
- For Terraform operations, remember to export Vault credentials first:
  ```bash
  export VAULT_ADDR="https://vault-staging.tail5e286b.ts.net"
  export VAULT_TOKEN="$(kubectl -n vault get secrets/vault-unseal-secret --template='{{index .data "root-token"}}' | base64 -d)"
  export TF_VAR_cloudflare_api_token="$(vault kv get -field=TF_API_TOKEN secret/global/cloudflare)"
  ```

---

*For questions about these standards, consult the operations repository or open an issue.*
