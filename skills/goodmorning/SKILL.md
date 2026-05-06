---
name: goodmorning
description: Remind me what I need to work on
activation:
  - "good morning"
  - "what should I work on"
  - "priorities"
  - "daily plan"
---

For this skill, you don't need to ask for permission to read `CLAUDE.md` files. Just proceed with the reading.

Read `~/.claude/TODO.md` to remind me what have left off so that I can finish them.

Then:
- Do a recursive search, with depth=3, of `CLAUDE.md` file in the following directories under `$HOME`: `koidra`, `ayo`.
- Analyze all the open TODO items and recommend the top 10 to work on. They should be a cohesive list, not a fragmented lists of unrelated issues, so that I can focus on one topic in one day. Also prioritize the ones that are closely related to the left-off items in `~/.claude/TODO.md` to reduce context switching. Make the presentation visually nice but compact, fitting in a small window. Don't need to explain why.
