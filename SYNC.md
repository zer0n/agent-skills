# Sync Workflow

This is a one-way fork of [`addyosmani/agent-skills`](https://github.com/addyosmani/agent-skills). Upstream commits flow in; nothing flows back.

## Remotes

- `origin` → `git@github.com:zer0n/agent-skills.git` (this fork — push here)
- `upstream` → `git@github.com:addyosmani/agent-skills.git` (read-only source)

If a fresh clone is missing the upstream remote:

```sh
git remote add upstream git@github.com:addyosmani/agent-skills.git
git config branch.main.mergeOptions "--ff-only"
```

## Routine sync

```sh
git checkout main
git fetch upstream
git merge upstream/main --ff-only
git push origin main
```

`--ff-only` is enforced via `branch.main.mergeOptions` so a non-FF merge fails loudly. That failure is a signal, not a problem — see below.

## When `--ff-only` fails

Two causes:

1. **Local commits on `main`** (e.g. a Tier A replacement PR was merged). Expected. Resolve by:
   ```sh
   git fetch upstream
   git merge upstream/main          # strip --ff-only for this one merge
   ```
   Inspect any conflict — upstream may have re-edited a file we replaced. Decide: keep ours, take theirs, or merge by hand.

2. **Upstream history rewrite** (force-push). Rare. Inspect `git log upstream/main` and `git reflog` before reconciling.

## Tier A replacements

The fork intentionally overwrites a few upstream `skills/<name>/SKILL.md` files with personal versions. When upstream re-edits one of those, the merge will conflict — that's the *only* time upstream's version of those files becomes visible again, and it's the right time to compare.

Find them via:

```sh
git log --diff-filter=M --name-only --pretty=format: -- skills/ | sort -u
```
