---
description: Debug and troubleshoot WTG Kubernetes platform issues systematically
---

# Troubleshooting - WTG Platform Issues

Debug and troubleshoot WTG platform issues systematically.

## Diagnostic Approach

### 1. Gather Information

```bash
# Check cluster context
kubectl config current-context

# Check component pods
kubectl get pods -n {{namespace}} -o wide

# Check recent events
kubectl get events -n {{namespace}} --sort-by='.lastTimestamp' | tail -20

# Check logs
kubectl logs -n {{namespace}} {{pod}} --tail=100
```

### 2. Common Issue Patterns

#### Pod Issues
| Symptom | Likely Cause | First Check |
|---------|--------------|-------------|
| `ImagePullBackOff` | Image not accessible | Check image name and registry auth |
| `CrashLoopBackOff` | App crashing on start | Check logs for startup errors |
| `Pending` | No schedulable node | Check node resources and taints |
| `ContainerCreating` | Volume/secret issues | Check PVC status and ExternalSecrets |

#### ArgoCD Issues
| Symptom | Likely Cause | First Check |
|---------|--------------|-------------|
| `OutOfSync` | Manifest drift | `argocd app diff {{app}}` |
| `Degraded` | Resource unhealthy | Check health of child resources |
| `Unknown` | Cluster unreachable | Check cluster connection |
| `SyncFailed` | Manifest error | Check sync error in ArgoCD UI |

#### Vault/ExternalSecrets Issues
| Symptom | Likely Cause | First Check |
|---------|--------------|-------------|
| `SecretSyncedError` | Path not found | Verify Vault path exists |
| `InvalidClusterSecretStore` | Auth failure | Check ESO credentials |
| Permission denied | Policy missing | Check Vault policy for path |

#### Ingress Issues
| Symptom | Likely Cause | First Check |
|---------|--------------|-------------|
| 502/503 | Backend unavailable | Check backend service/pod health |
| 404 | Route not matching | Check ingress rules and paths |
| TLS errors | Cert not ready | Check Certificate resource status |

### 3. WTG-Specific Checks

#### Cluster Access
```bash
# Staging cluster
kubectl --context wtg-k3s-staging get nodes

# Dev cluster  
kubectl --context wtg-microk8s-dev get nodes
```

#### Vault Access
```bash
export VAULT_ADDR="https://vault-staging.tail5e286b.ts.net"
export VAULT_TOKEN=$(kubectl -n vault get secrets/vault-unseal-secret --template='{{index .data "root-token"}}' | base64 -d)
vault status
```

#### ArgoCD Access
```bash
argocd app get {{app}} --refresh
argocd app sync {{app}} --prune
```

### 4. Quick Fixes

```bash
# Restart a deployment
kubectl rollout restart deployment/{{name}} -n {{namespace}}

# Force ArgoCD sync
argocd app sync {{app}} --force --prune

# Refresh ExternalSecret
kubectl annotate externalsecret {{name}} -n {{namespace}} force-sync=$(date +%s) --overwrite
```

## Task

Diagnose and resolve the issue. Provide:
1. Diagnostic commands to run
2. Likely root causes based on symptoms
3. Resolution steps
4. Verification that the issue is resolved
