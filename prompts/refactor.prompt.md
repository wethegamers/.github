---
description: Guide code refactoring with clear objectives, risk assessment, and safety
---

# Refactoring

Guide code refactoring with clear objectives and safety.

## Output Format

```markdown
## Refactoring Plan: {file_or_component}

### Goal
[What improvement we're making and why]

### Current State
[Brief analysis of current code issues]

### Proposed Changes

#### 1. [Change Category]
**Before:**
```{language}
// Current code
```

**After:**
```{language}
// Refactored code
```

**Rationale:** [Why this change improves the code]

### Risk Assessment
| Risk | Likelihood | Mitigation |
|------|------------|------------|
| [Risk 1] | Low/Med/High | [How to mitigate] |

### Testing Strategy
- [ ] Existing tests still pass
- [ ] [Additional tests needed]

### Rollback Plan
[How to revert if issues arise]
```

## Common Refactoring Patterns

### Extract Function
When code is duplicated or a function is too long.

### Extract Interface
When you need to mock dependencies or support multiple implementations.

### Replace Magic Numbers
When literals appear without context.

### Simplify Conditionals
When nested if/else becomes hard to follow.

### Introduce Parameter Object
When functions have too many parameters.

## WTG-Specific Considerations

### Go Refactoring
- Keep functions under ~50 lines
- Extract interfaces for external dependencies
- Use table-driven tests
- Ensure errors are wrapped with context

### Kubernetes YAML
- Extract common labels to `_helpers.tpl`
- Use Kustomize patches for environment differences
- Keep base manifests environment-agnostic

### Database Migrations
- Migrations must be reversible
- Keep backward compatibility during rollout
- Test with production-like data volume

## Task

Provide a refactoring plan for the specified code. Include:
1. Clear goal and current state analysis
2. Proposed changes with before/after examples
3. Risk assessment
4. Testing strategy
5. Rollback plan
