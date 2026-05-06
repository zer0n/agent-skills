---
name: quality-gate
description: 5-stage quality validation pipeline before deployments and merges.
activation:
  - "deploy"
  - "merge"
  - "push"
  - "release"
  - "production"
  - "before I ship"
  - "ready to"
  - "ci failed"
  - "pipeline failed"
---

# Quality Gate: 5-Stage Validation

Enforce quality checks before deployment.

## Activation Triggers

Engage for: deploy requests, merge preparation, CI failures, "before I ship".

## The 5 Gates

### Gate 1: Pre-Commit (Local)
```bash
make lint      # ruff check . --fix
make format    # ruff format .
make typecheck # pyright / mypy
```
**Must pass before**: `git commit`

### Gate 2: MR-Check (GitLab CI)
```bash
glab ci status
```
Checks: lint, type, unit tests, build
**Must pass before**: MR merge

### Gate 3: Preview/Staging
- Deploy to staging environment
- Smoke test critical paths
**Must pass before**: Production deploy

### Gate 4: E2E/Integration
```bash
pytest tests/e2e/ --base-url=$STAGING_URL
```
**Must pass before**: Production deploy

### Gate 5: Production Deploy
Only after gates 1-4 pass.
```bash
# Rollback ready
git revert HEAD --no-commit  # prepare rollback
glab ci run --branch main
```

## Quick Checks

```bash
# All local gates
make lint && make test

# CI status
glab ci status

# View failed job
glab ci view
```

## Behavior

When user mentions deploy/merge/push:
1. Ask: "Have you run local checks?" (if not evident)
2. Check `glab ci status` if MR exists
3. Report gate status:
   ```
   Gate 1 (Local):     PASS/FAIL
   Gate 2 (CI):        PASS/FAIL/PENDING
   Gate 3 (Staging):   PASS/FAIL/SKIP
   Gate 4 (E2E):       PASS/FAIL/SKIP
   Gate 5 (Prod):      BLOCKED/READY
   ```
4. Block on failures, suggest fixes
