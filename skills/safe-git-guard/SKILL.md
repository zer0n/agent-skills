---
name: safe-git-guard
description: Prevent destructive git operations. Require backup and confirmation for risky commands.
activation:
  - "git reset"
  - "git push --force"
  - "git push -f"
  - "git clean"
  - "git checkout ."
  - "force push"
  - "hard reset"
  - "delete branch"
  - "rebase"
---

# Safe Git Guard: Destructive Operation Protection

Intercept and protect against accidental data loss.

## Activation Triggers

Engage when detecting: reset --hard, push --force, clean -f, checkout ., rebase, delete.

## Blocked Operations

| Command | Risk | NEVER Execute Without |
|---------|------|----------------------|
| `git reset --hard` | Loses uncommitted work | Stash backup first |
| `git push --force` on main/master/dev | Destroys remote history | EXPLICIT user override |
| `git clean -fd` | Removes untracked files | Dry-run first |
| `git checkout .` | Discards all changes | Stash backup first |

## Protected Branches
- `main`, `master`
- `develop`, `dev`
- `release/*`, `hotfix/*`

## Safety Protocol

### Before ANY Risky Operation:

1. **Create Backup**
```bash
# Uncommitted changes
git stash push -m "backup-$(date +%Y%m%d-%H%M%S)"

# Branch backup
git branch backup-$(git branch --show-current)-$(date +%Y%m%d-%H%M%S)

# Tag backup (for resets)
git tag backup-$(date +%Y%m%d-%H%M%S)
```

2. **Show Impact**
```
Operation: [what will happen]
Files affected: [count/list]
Reversible: YES/NO
Backup: [created ref]
```

3. **Require Explicit Confirmation**
Only proceed after user explicitly confirms.

## Recovery Commands

```bash
# Recover stash
git stash list && git stash pop

# Recover from backup tag
git reset --hard backup-YYYYMMDD-HHMMSS

# Reflog recovery
git reflog
git checkout -b recovered HEAD@{n}
```

## Behavior

- ALWAYS intercept destructive commands
- ALWAYS create backup before proceeding
- NEVER force-push to protected branches without explicit "I confirm force push to [branch]"
- For rebases: create backup branch FIRST
