---
description: Create Architecture Decision Records (ADR) following WTG standards
---

# Architecture Decision Record

Create an ADR for the WTG platform. ADRs document significant architectural decisions with their context, rationale, and consequences.

## ADR Template

```markdown
# ADR-{NNN}: {Title}

**Status**: Proposed | Accepted | Deprecated | Superseded by ADR-XXX  
**Date**: YYYY-MM-DD  
**Deciders**: [Names/Roles]

---

## Context

[Describe the situation that requires a decision:
- What problem are we trying to solve?
- What constraints exist?
- What forces are at play?
- Why is a decision needed now?]

## Decision

[State the decision clearly using active voice:
"We will use X for Y" not "X will be used for Y"]

## Rationale

[Explain why:
- What alternatives were considered?
- What criteria were used to evaluate?
- Why is this the best choice?]

## Consequences

### Positive
- [Benefit 1]
- [Benefit 2]

### Negative
- [Drawback 1 and mitigation]
- [Drawback 2 and mitigation]

### Neutral
- [Trade-offs]

## Alternatives Considered

### Alternative 1: [Name]
**Rejected Because**: [Reason]

### Alternative 2: [Name]
**Rejected Because**: [Reason]

## Implementation
1. [Step 1]
2. [Step 2]

## Related
- [ADR-XXX: Related Decision](./ADR-XXX-title.md)
```

## File Location

Save ADRs in: `operations/adr/ADR-{NNN}-{kebab-case-title}.md`

## Good ADR Characteristics

- **Immutable once accepted**: Don't edit old ADRs, supersede them
- **Specific**: Include versions, configurations, timelines
- **Honest about trade-offs**: Document negatives, not just positives
- **Actionable**: Reader should understand what to do next

## WTG Context

- Multi-cluster architecture (dev, staging, prod)
- GitOps with ArgoCD
- Vault for secrets
- Tailscale + Cloudflare for networking

## Task

Create an ADR for the specified decision. Include context, decision, rationale, consequences (positive AND negative), and at least 2 alternatives considered.
