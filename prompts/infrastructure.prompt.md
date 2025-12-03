---
description: Create Kubernetes and ArgoCD resources following WTG platform standards
---

# Infrastructure - Create WTG Resources

Create infrastructure resources following WTG platform standards.

## Standards to Follow

### Naming Conventions
- Namespaces: `{app}` (NO environment suffix)
- Resources: `{app}-{resource}` (e.g., `agis-bot-deployment`)
- Secrets: `{app}-secrets`
- Ingress hosts: `{app}.{env}.wtgg.org`

### File Locations
- ArgoCD Applications: `gitops/registry/environments/{environment}/{app}.yaml`
- Base manifests: `gitops/apps/{app}/base/`
- Overlays: `gitops/apps/{app}/overlays/{environment}/`
- ExternalSecrets: `gitops/registry/clusters/{cluster}/components/{app}/externalsecret.yaml`

### Required Labels
```yaml
metadata:
  labels:
    app.kubernetes.io/name: {{appName}}
    app.kubernetes.io/part-of: wtg-platform
    app.kubernetes.io/managed-by: argocd
```

### Ingress Configuration
- IngressClass: `traefik` (public) or `tailscale` (private)
- TLS: Always use `cert-manager.io/cluster-issuer: letsencrypt-dns`
- DNS: `external-dns.alpha.kubernetes.io/target: 217.155.156.21`

### Vault Secrets Path
```
secret/{environment}/{appName}/{KEY}
```

### ExternalSecret Template
```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: {{app}}-secrets
  namespace: {{app}}
spec:
  secretStoreRef:
    name: vault-kv-secret
    kind: ClusterSecretStore
  target:
    name: {{app}}-secrets
    creationPolicy: Owner
  data:
    - secretKey: SOME_SECRET
      remoteRef:
        key: {{env}}/{{app}}
        property: SOME_SECRET
```

### ArgoCD Application Template
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: {{app}}
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/wethegamers/gitops
    path: apps/{{app}}/overlays/{{env}}
    targetRevision: HEAD
  destination:
    name: wtg-{platform}-{env}
    namespace: {{app}}
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## Task

Create the requested resource following all standards above. Include:
1. Complete YAML manifest with all required fields
2. Proper labels and annotations
3. Comments explaining non-obvious configurations
4. Any required supporting resources
