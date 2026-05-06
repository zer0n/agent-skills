---
name: tech-debt
description: Identify, quantify, and prioritize technical debt with ROI-focused remediation plans.
activation:
  - "tech debt"
  - "technical debt"
  - "code smell"
  - "refactor"
  - "cleanup"
  - "legacy"
  - "complexity"
  - "maintainability"
  - "why is this code"
---

# Tech Debt: Analysis and Remediation

Systematic debt identification with actionable remediation.

## Activation Triggers

Engage for: tech debt discussions, refactoring requests, "why is this code so...", complexity concerns.

## Debt Categories

### Code Debt
- Duplicated logic (>10 lines repeated)
- Cyclomatic complexity >10
- Functions >50 lines
- Classes >500 lines (God classes)

### Architecture Debt
- Circular dependencies
- Violated boundaries
- Missing abstractions
- Deprecated patterns

### Testing Debt
- Coverage <80%
- Missing edge cases
- Flaky tests
- No integration tests

### Dependency Debt
- Outdated major versions
- Security vulnerabilities
- Deprecated APIs

## Analysis Output

```markdown
## Debt Item: [Name]
Location: [files:lines]
Category: [Code/Architecture/Testing/Dependency]
Severity: CRITICAL/HIGH/MEDIUM/LOW

### Impact
- Time wasted: ~Xh/month
- Bug contribution: X bugs/month
- Risk: [what could go wrong]

### Remediation
Quick fix: [immediate action]
Proper fix: [right way to solve]
Effort: [hours/days]
ROI: [savings vs effort]

### [TECH DEBT] Comment
Add to code:
`# [TECH DEBT] [description] - [remediation hint]`
```

## Prioritization Matrix

| Effort \ Impact | High Impact | Low Impact |
|-----------------|-------------|------------|
| Low Effort      | DO NOW      | Quick wins |
| High Effort     | Plan sprint | Skip/defer |

## Behavior

- Flag [TECH DEBT] in code comments as instructed in CLAUDE.md
- Always provide "quick fix" AND "proper fix" options
- Quantify impact in hours/month when possible
- Prefer "delete code" solutions over "add abstraction"
