---
description: Security review checklist for Kubernetes workloads and secrets management
---

# Security Review - WTG Platform

Perform security review following WTG platform standards.

## Security Checklist

### Secrets Management
- [ ] No hardcoded secrets in code or manifests
- [ ] Secrets stored in Vault at correct path: `secret/{env}/{app}/`
- [ ] ExternalSecrets used for Kubernetes secret injection
- [ ] Vault policies follow least-privilege principle

### Authentication & Authorization
- [ ] Service accounts have minimal RBAC permissions
- [ ] API endpoints require authentication
- [ ] Authorization checks on sensitive operations
- [ ] No default/weak credentials

### Network Security
- [ ] NetworkPolicies restrict pod-to-pod traffic
- [ ] Ingress uses TLS (cert-manager)
- [ ] Internal services use Tailscale (`tailscale` IngressClass)
- [ ] No unnecessary port exposure

### Container Security
- [ ] Non-root containers where possible
- [ ] Read-only root filesystem where possible
- [ ] Resource limits defined
- [ ] Images from trusted registries
- [ ] No `privileged: true` without justification

### Kubernetes Security
- [ ] No `hostNetwork`, `hostPID`, `hostIPC` without justification
- [ ] SecurityContext configured appropriately
- [ ] ServiceAccount tokens not auto-mounted unless needed
- [ ] Secrets mounted as files, not environment variables (preferred)

### Input Validation
- [ ] User input validated and sanitized
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (output encoding)
- [ ] Path traversal prevention

## WTG-Specific Security Patterns

### Vault Access
```yaml
# ExternalSecret for secure secret injection
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: {{app}}-secrets
spec:
  secretStoreRef:
    name: vault-kv-secret
    kind: ClusterSecretStore
  target:
    name: {{app}}-secrets
  data:
    - secretKey: API_KEY
      remoteRef:
        key: {{env}}/{{app}}
        property: API_KEY
```

### Secure Pod Configuration
```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  fsGroup: 1000
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop:
      - ALL
```

### Network Policy Example
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: {{app}}-network-policy
spec:
  podSelector:
    matchLabels:
      app: {{app}}
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: traefik
  egress:
    - to:
        - namespaceSelector:
            matchLabels:
              name: vault
```

## Severity Levels

| Level | Description | Examples |
|-------|-------------|----------|
| 🔴 Critical | Immediate exploit risk | Hardcoded secrets, SQL injection |
| 🟠 High | Significant security gap | Missing auth, weak permissions |
| 🟡 Medium | Defense-in-depth issue | Missing network policy |
| 🔵 Low | Best practice | Non-root recommendation |

## Task

Review the provided code/configuration for security issues. Provide:
1. Summary of security posture
2. List of findings by severity
3. Specific remediation steps with examples
4. Recommendations for security improvements
