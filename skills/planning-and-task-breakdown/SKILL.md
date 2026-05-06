---
name: spec-kit
description: Constitution-based 7-phase development workflow. Enforces systematic planning before implementation.
activation:
  - "implement"
  - "build"
  - "add feature"
  - "create"
  - "design"
  - "new feature"
  - "how should I"
  - "architecture"
---

# Spec-Kit: Constitution-Based Development

When activated, enforce this 7-phase workflow before writing code.

## Activation Triggers

Engage when user requests implementation of non-trivial features, architectural decisions, or asks "how should I" questions about design.

## The 7 Phases

### Phase 1: Establish Principles
Define non-negotiable constraints:
- Security requirements (auth, validation, OWASP)
- Performance budgets (latency, memory)
- Compatibility constraints (APIs, backwards-compat)
- Code style (match existing patterns)

### Phase 2: Define Requirements
Break into deliverables:
- **Must have**: Core functionality
- **Should have**: Important but not blocking
- **Won't have**: Explicit scope boundaries (prevents scope creep)

### Phase 3: Resolve Ambiguities
- List assumptions being made
- Flag decisions needing user input
- Document edge cases

### Phase 4: Technical Plan
Present 2-3 options with tradeoffs:
```
Option A: [Name]
- Approach: ...
- Pros: ...
- Cons: ...
- Blast radius: [files/systems affected]

Option B: [Name]
...

Recommendation: Option [X] because...
```

### Phase 5: Generate Tasks
Atomic, testable tasks with affected files listed.

### Phase 6: Validate Consistency
Cross-check: Do tasks cover requirements? Conflicts? Blast radius acceptable?

### Phase 7: Implement
Only after phases 1-6 approved. Run `make lint && make test` after changes.

## Behavior

- Pause after Phase 4 for user approval
- If user says "just do it" - skip to minimal Phase 5 + Phase 7
- For trivial tasks (<10 lines), acknowledge but proceed directly
