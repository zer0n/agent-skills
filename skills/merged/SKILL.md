---
name: merged
description: Sync current branch with main after an MR was merged (handles squash merges).
---

# Merged

The user's MR was merged into main. Sync the branch, preserving any new commits not part of the merged MR.

## Steps

1. Stash uncommitted changes if any (`git stash`).
2. Save the current `origin/main` ref: `OLD_MAIN=$(git rev-parse origin/main)`
3. `git fetch origin` (never checkout main — may be in another worktree)
4. Rebase only commits after the old fork point onto the new main:
   ```
   git rebase --onto origin/main $OLD_MAIN
   ```
   This replays only commits created *after* the last sync with main — the merged MR's commits (which were based on `$OLD_MAIN`) are dropped, and any new work is rebased onto the updated main.
5. If conflicts arise on commits that look like they were part of the merged MR, `git rebase --skip` them.
6. Pop stash if anything was stashed.
7. Report: current branch, commits ahead of origin/main, clean/dirty state.
