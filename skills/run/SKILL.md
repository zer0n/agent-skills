---
name: run
description: Execute the next task in TODO.md
activation:
  - "next task"
  - "run task"
  - "TODO"
---

Execute the next task(s) in `TODO.md`. If $1 is `code`, execute only the tasks with `/code` in the title. Likewise for `test`/`/test`.

Iterate for each task:
1. Display the task to remind me of the context.
2. Show your plan or design and ask me for confirmation.
3. Once a task is completed, check with me to continue with the next task or to stop for reviewing.
