---
description: Write technical documentation (manuals, guides, references) following WTG standards
---

# Documentation

Write technical documentation for the WTG platform that is clear, accurate, and actionable.

## Document Types

### Manual (Comprehensive Guide)
```markdown
# {Topic} Manual

**Version**: 1.0.0
**Last Updated**: YYYY-MM-DD
**Status**: Draft | Active | Deprecated

---

## Overview
[Brief description and role in platform]

## Prerequisites
[What reader needs before starting]

## Architecture
[How component fits into platform]

## Configuration
[Step-by-step with examples]

## Operations
[Day-to-day tasks]

## Troubleshooting
[Common issues and solutions]

## Related Documents
[Links to related docs]
```

### Reference (API/Configuration)
```markdown
# {Component} Reference

**Version**: 1.0.0
**Last Updated**: YYYY-MM-DD

---

## Overview

## Configuration Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `param1` | string | `"default"` | Description |

## Examples

### Basic Usage
```yaml
config:
  setting: value
```

### Advanced Usage
```yaml
config:
  setting: value
  advanced:
    option1: value1
```

## Troubleshooting
```

### Guide (How-To)
```markdown
# How to {Action}

**Estimated Time**: X minutes

## Prerequisites
- [ ] Requirement 1
- [ ] Requirement 2

## Steps

### Step 1: {Action}
[Instructions with code examples]

### Step 2: {Action}
[Instructions]

## Verification
[How to confirm success]

## Next Steps
[What to do after]
```

## Style Guidelines

1. **Active Voice**: "Run the command" not "The command should be run"
2. **Concise**: One idea per sentence, avoid filler
3. **Practical**: Include runnable examples
4. **Consistent**: Use WTG terminology
5. **Accessible**: Define acronyms on first use

## WTG Platform Context

- **Platform**: Kubernetes-based gaming infrastructure
- **GitOps**: ArgoCD with app-of-apps pattern
- **Secrets**: HashiCorp Vault + ExternalSecrets
- **Networking**: Traefik + Tailscale + Cloudflare
- **Clusters**: wtg-microk8s-dev, wtg-k3s-staging, wtg-talos-prod

## Task

Create documentation for the specified topic. Include proper versioning header, clear actionable content, working examples, and links to related documentation.
