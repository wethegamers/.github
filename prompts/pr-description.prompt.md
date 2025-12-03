---
description: Generate consistent pull request descriptions with type and testing info
---

# PR Description

Generate consistent pull request descriptions following WTG standards.

## Template

```markdown
## Summary

[1-2 sentence description of what this PR does]

## Changes

- [Change 1]
- [Change 2]
- [Change 3]

## Type of Change

- [ ] Bug fix (non-breaking change fixing an issue)
- [ ] New feature (non-breaking change adding functionality)
- [ ] Breaking change (fix or feature causing existing functionality to change)
- [ ] Documentation update
- [ ] Infrastructure/configuration change

## Related Issues

Closes #[issue]

## Testing

- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manually tested in dev environment

### Test Steps
1. [How to test this change]
2. [Expected result]

## Breaking Changes (if applicable)

⚠️ **This PR contains breaking changes:**

[Description of breaking changes]

### Migration Steps
1. [Step 1]
2. [Step 2]

## Checklist

- [ ] Code follows WTG style guidelines
- [ ] Self-reviewed my code
- [ ] Added/updated documentation
- [ ] No secrets or credentials in code
- [ ] Kubernetes manifests follow naming standards
```

## Title Format

```
type(scope): short description

Examples:
feat(agis-bot): add guild treasury commands
fix(gitops): correct vault path for staging secrets
docs(operations): update naming standards to v3.1
chore(ci): update GitHub Actions to use Argo Workflows
```

## Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code change that neither fixes nor adds |
| `test` | Adding tests |
| `chore` | Maintenance, CI, dependencies |

## Scopes

Use repository or component name: `agis-bot`, `gitops`, `vault`, `argocd`, `ci`

## Task

Generate a PR description for the provided changes. Include summary, list of changes, type, and testing information.
