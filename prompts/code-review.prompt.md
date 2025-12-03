---
description: Perform code review with severity levels following WTG standards
---

# Code Review - WTG Standards

Perform code review following WTG platform standards.

## Review Criteria

### General Quality
- [ ] Code is readable and maintainable
- [ ] Functions/methods have single responsibility
- [ ] Error handling is appropriate
- [ ] No dead code or commented-out blocks
- [ ] Variable names are descriptive

### Security
- [ ] No hardcoded secrets or credentials
- [ ] Input validation present
- [ ] Proper error messages (no sensitive data leakage)
- [ ] Authentication/authorization checked

### Performance
- [ ] No obvious performance issues
- [ ] Resource cleanup (connections, files)
- [ ] Appropriate data structures

## Language-Specific Checks

### Go
- [ ] `gofmt` / `goimports` applied
- [ ] Error wrapping with context
- [ ] Interfaces defined by consumer

### TypeScript
- [ ] Strict TypeScript enabled
- [ ] No `any` types without justification
- [ ] Async/await over callbacks

### YAML (Kubernetes)
- [ ] Proper indentation (2 spaces)
- [ ] Required labels present
- [ ] Resource limits defined
- [ ] No deprecated API versions

### Terraform
- [ ] Provider versions pinned
- [ ] Resources have proper tags
- [ ] Variables have descriptions

## Feedback Format

For each issue:

```
**[Severity] Issue**: Brief description

**Location**: file:line

**Current Code**:
\`\`\`
// problematic code
\`\`\`

**Suggested Fix**:
\`\`\`
// improved code
\`\`\`

**Rationale**: Why this improves the code.
```

### Severity Levels

| Level | Description | Action |
|-------|-------------|--------|
| 🔴 Blocker | Must fix before merge | Required |
| 🟠 Major | Should fix | Required |
| 🟡 Minor | Nice to have | Optional |
| 🔵 Suggestion | Style preference | Optional |

## Task

Review the provided code. Provide:
1. Summary of overall code quality
2. List of issues by severity
3. Specific suggestions with code examples
4. Positive callouts for good patterns
