```yaml
number: 17622
title: "[red-knot] remove fixpoint handling from try_mro query"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
draft: true
base: main
head: cjm/nomrofix
created_at: 2025-04-25T01:43:34Z
updated_at: 2025-05-04T15:29:20Z
url: https://github.com/astral-sh/ruff/pull/17622
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] remove fixpoint handling from try_mro query

---

_@carljm_

## Summary

Fixpoint handling isn't really safe for the `try_mro` query, because if we actually hit a cycle, it can result in an ever-growing MRO that never converges.

No tests or ecosystem projects break from removing this -- I think other changes have improved our behavior here since I added this.

## Test Plan

Existing CI.


---

_Label `red-knot` added by @carljm on 2025-04-25 01:43_

---

_Comment by @github-actions[bot] on 2025-04-25 01:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Closed by @carljm on 2025-05-04 15:29_

---
