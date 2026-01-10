```yaml
number: 1433
title: Avoid cycle recovery based on iteration count
type: issue
state: closed
author: MichaReiser
labels: []
assignees: []
created_at: 2025-10-24T10:53:37Z
updated_at: 2025-11-26T16:50:29Z
url: https://github.com/astral-sh/ty/issues/1433
synced_at: 2026-01-10T01:58:59Z
```

# Avoid cycle recovery based on iteration count

---

_Issue opened by @MichaReiser on 2025-10-24 10:53_

We currently rely on the iteration count for cycle recovery. There are two issues with it:

1. Recovery functions aren't guaranteed to be called for every iteration count if they participate in an outer cycle. The recovery function can be skipped for two reasons: a) Because *this* query did converge in this iteration, but there might be another cycle that didn't converge, enforcing another iteration and b) if the cycle head only conditionally participates in the outer cycle head (e.g. only every other iteration). That's why we want to avoid testing when the iteration count is **equal**. At least, test if the iteration count is larger than.
2. The number of iterations can vary based from which query the cycle is entered. However, the execution order of queries is non-deterministic in a multithreaded context, risking that our error recovery becomes non-deterministic too.

That's why we should prefer cycle recovery functions that recover based on the values rather than the exact iteration count

---

_Added to milestone `GA` by @MichaReiser on 2025-10-24 10:53_

---

_Closed by @carljm on 2025-11-26 16:50_

---
