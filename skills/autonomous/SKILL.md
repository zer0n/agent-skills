---
name: autonomous
description: Execute complex multi-step tasks with progress tracking and checkpointing for resumption.
activation:
  - "refactor"
  - "migrate"
  - "upgrade"
  - "large change"
  - "across the codebase"
  - "all files"
  - "entire"
  - "comprehensive"
---

# Autonomous: Long-Running Task Execution

For large-scale changes that span multiple files or require extended execution.

## Activation Triggers

Engage for: refactors, migrations, upgrades, or any task mentioning "all", "entire", "across the codebase".

## Workflow

### 1. Task Decomposition
Break into checkpointed stages:
```
Stage 1: [Description]
  Checkpoint: [verifiable state]
  Files: [affected]

Stage 2: [Description]
  Checkpoint: [verifiable state]
  Files: [affected]
```

### 2. Progress Tracking
Use TodoWrite extensively. Each stage = one todo item.

### 3. Execution Rules
- Complete one stage fully before next
- Run validation after each stage (`make lint && make test`)
- If blocked, document blocker and STOP (don't guess)
- Commit after each major stage if user permits

### 4. Resumption Pattern
If conversation resumes mid-task:
- Check todo list state
- Report completed stages
- Ask: "Continue from Stage N?"

### 5. Completion
- Full validation suite
- Summary of all changes with file list
- Cleanup any temp artifacts

## Behavior

- Present staged breakdown before executing
- Estimate blast radius per stage
- Prefer "delete code" approaches when refactoring
- Flag any [TECH DEBT] discovered during execution
