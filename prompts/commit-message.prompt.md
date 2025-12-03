---
description: Generate conventional commit messages with proper type and scope
---

# Commit Message

Generate conventional commit messages following WTG standards.

## Format

```
type(scope): subject line (max 50 chars)

[optional body - wrap at 72 chars]
- Detail 1
- Detail 2

[optional footer]
Closes #123
BREAKING CHANGE: description
```

## Conventional Commit Types

| Type | When to Use | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(bot): add /balance command` |
| `fix` | Bug fix | `fix(api): handle nil pointer in user lookup` |
| `docs` | Documentation | `docs: update vault rotation runbook` |
| `style` | Formatting | `style: format go files with gofmt` |
| `refactor` | Code restructure | `refactor(db): extract query builder` |
| `test` | Tests | `test: add integration tests for pricing` |
| `chore` | Maintenance | `chore(deps): update discordgo to v0.27` |
| `ci` | CI/CD | `ci: add helm lint to workflow` |
| `perf` | Performance | `perf(cache): reduce TTL lookup overhead` |

## Rules

1. **Subject line**: Max 50 characters, imperative mood ("add" not "added")
2. **Body**: Wrap at 72 characters, explain what and why
3. **Footer**: Reference issues, note breaking changes

## Examples

### Simple
```
fix(vault): correct secret path for cloudflare token
```

### With Body
```
feat(agis-bot): add guild treasury pooling

Allow guilds to maintain a shared balance that members
can contribute to for purchasing Titan-tier servers.

- Add /treasury commands (balance, deposit, withdraw)
- Create guild_treasury table
- Implement permission checks for withdrawals
```

### Breaking Change
```
refactor(api): change pricing response format

BREAKING CHANGE: pricing endpoint now returns cents instead
of dollars. Update all clients to divide by 100 for display.

Closes #142
```

## Task

Generate a conventional commit message for the provided changes. Include type, scope (if applicable), and clear subject line.
