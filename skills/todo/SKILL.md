---
name: todo
description: Manage the global TODO at ~/TODO.md (Top 5, Backlog, Projects index)
activation:
  - "global TODO"
  - "todo recommend"
  - "todo discover"
---

The global TODO lives at `~/TODO.md` with three zones:
1. **Top 5** — between `<!-- BEGIN:top5 -->` and `<!-- END:top5 -->`. Curated; only `recommend` rewrites it.
2. **Backlog** — between the `<!-- BEGIN:backlog-marker -->` line and the `## Projects` header. Hand-maintained.
3. **Projects** — between `<!-- BEGIN:projects -->` and `<!-- END:projects -->`. An index of per-project `TODO.md` paths; only `discover` rewrites it.

Never write outside the marked blocks for a given subcommand. If a marker is missing, stop and ask.

## Subcommands

### `/todo` (no args)
Print `~/TODO.md` as-is.

### `/todo recommend`
Goal: surface and rank the 5 highest-value next items across the user's whole working life.

1. Read `~/TODO.md`. Extract the Backlog section (between the backlog marker line and `## Projects`) and the list of project TODO paths from the Projects block.
2. For each project TODO and for the Backlog, take the **top 3 open items** (lines matching `- [ ]`, in source order, skipping `- [x]` and section headers). Source order is treated as the user's existing priority.
3. Form the union of candidates (≤ 3 × N + 3 backlog items). Re-rank by:
   - **Business urgency** — explicit deadlines, blocking others, customer-facing.
   - **Long-term impact** — leverage, reduces future cost, unlocks a roadmap.
   - **Ease of execution** — small, well-scoped, low coordination cost.
   Surface ties toward higher impact over ease.
4. Present the proposed Top 5 as a numbered list. Each line: `**[source]** task — one-line rationale citing which of the three criteria drove it`. After the list, show the candidate pool you considered (collapsed) so the user can challenge.
5. Ask for confirmation. On approval, rewrite **only** the content between `<!-- BEGIN:top5 -->` and `<!-- END:top5 -->`. Do not touch any other zone.

### `/todo discover`
1. Glob for project TODO files: `find ~/koidra ~/ayo -maxdepth 3 -name TODO.md 2>/dev/null` (extend the search roots if the user names other workspaces).
2. Render each as a markdown link: `- [<repo-relative-label>](<path>)`. Sort by path. Use `~` (not `/home/one`) in the rendered path.
3. Rewrite **only** the content between `<!-- BEGIN:projects -->` and `<!-- END:projects -->`. Do not touch any other zone.
4. Report what was added/removed vs. the prior list.

## Notes
- Always preview the rewrite (diff or before/after of the affected zone) and ask for confirmation before writing.
- The per-project `/run` skill remains the way to execute tasks inside a repo. `/todo` is purely for the global view and prioritization.
