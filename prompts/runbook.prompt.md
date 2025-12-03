---
description: Create operational runbooks with step-by-step procedures and rollback
---

# Runbook

Create an operational runbook for the WTG platform with step-by-step instructions.

## Template

```markdown
# {Procedure Name}

**Version**: 1.0.0  
**Last Updated**: YYYY-MM-DD  
**Estimated Time**: X minutes  
**Category**: procedures | maintenance | disaster-recovery | incidents

---

## Purpose

[What this procedure accomplishes and when to use it]

## Prerequisites

- [ ] Access to [required cluster/system]
- [ ] [Required role/permissions]
- [ ] [Required tool] installed

## Required Access

| Resource | Access Level | How to Obtain |
|----------|--------------|---------------|
| Vault | Admin token | See Vault Access Guide |
| Kubernetes | cluster-admin | Via kubeconfig |

## Procedure

### Step 1: [Action Title]

**Purpose**: [Why this step is necessary]

```bash
# Command with explanation
command --flag value
```

**Expected Output**:
```
[What you should see]
```

**If this fails**: [What to do]

---

### Step 2: [Action Title]

[Continue pattern...]

---

## Verification

```bash
# Verification command
verification-command
```

**Expected Result**: [What indicates success]

- [ ] [Verification item 1]
- [ ] [Verification item 2]

## Rollback

If the procedure fails:

1. [Rollback step 1]
   ```bash
   rollback-command
   ```

2. [Rollback step 2]

## Troubleshooting

### Issue: [Common Problem]

**Symptom**: [What you observe]
**Solution**:
```bash
fix-command
```

## Escalation

Escalate if:
- Procedure fails after 3 attempts
- Rollback does not restore service
- Data loss is suspected
```

## File Location

| Category | Path |
|----------|------|
| `procedures` | `operations/runbooks/procedures/` |
| `maintenance` | `operations/runbooks/maintenance/` |
| `disaster-recovery` | `operations/runbooks/disaster-recovery/` |
| `incidents` | `operations/runbooks/incidents/` |

## Naming Convention

Use kebab-case: `{action}-{subject}.md`
- `rotate-vault-token.md`
- `restore-vault-from-backup.md`

## Good Runbook Characteristics

- **Atomic steps**: Each step has one action
- **Copy-paste ready**: Commands work without modification
- **Idempotent**: Running twice doesn't break things
- **Explicit expectations**: Every step says what should happen
- **Fail-safe**: Every step has failure handling

## WTG Access Patterns

```bash
# Staging cluster
kubectl --context wtg-k3s-staging [command]

# Vault access
export VAULT_ADDR="https://vault-staging.tail5e286b.ts.net"
export VAULT_TOKEN=$(kubectl -n vault get secrets/vault-unseal-secret \
  --template='{{index .data "root-token"}}' --context wtg-k3s-staging | base64 -d)
```

## Task

Create a runbook for the specified procedure. Include purpose, prerequisites, step-by-step procedure with commands and expected output, verification, rollback, and troubleshooting.
