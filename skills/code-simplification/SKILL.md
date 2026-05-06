---
name: refactor
description: Refactor code with project-specific constraints
activation:
  - "refactor"
  - "restructure"
  - "clean up code"
---

You will perform a **constrained refactor** for the target provided in $ARGUMENTS

Requirements:
- Obey all architecture and style rules defined in `CLAUDE.md`.
- Avoid changing public APIs unless explicitly justified.
- Prefer small, reversible steps.

Process:
1. Locate the code referenced by $ARGUMENTS.
2. Briefly restate the current behavior and responsibilities.
3. Propose a refactor plan in numbered steps (read-only, no edits yet).
4. Await user confirmation if the plan is large or touches multiple modules.
5. Implement the refactor step by step, explaining:
   - Which files are modified.
   - How responsibilities moved.
   - How invariants and tests are preserved.

End with a short summary and mention any tests that must be run.
