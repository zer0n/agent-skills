---
name: perf
description: Analyze a hotspot for performance, complexity, and design issues
activation:
  - "performance"
  - "hotspot"
  - "slow"
  - "optimize"
  - "benchmark"
---

You will analyze the code referenced by $ARGUMENTS as a potential hotspot.

Steps:
1. Locate and read the relevant function(s) and surrounding context.
2. Map out:
   - Call graph / dependencies.
   - External systems touched (DB, APIs, queues, disk).
3. Identify:
   - Time complexity for critical operations.
   - Potential N+1 queries, unnecessary loops, or sync I/O in hot paths.
   - Data structure choices that may be suboptimal.

If profiling or benchmarking commands exist (check package.json or docs),
suggest a concrete command and explain how to interpret the results.

Output:
- A concise explanation of how the hotspot works today.
- 3-5 specific, realistic optimization ideas with tradeoffs.
- Any recommended instrumentation or logging for observability.
