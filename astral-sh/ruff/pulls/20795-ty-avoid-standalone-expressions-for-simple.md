```yaml
number: 20795
title: "[ty] avoid standalone expressions for simple subscript targets"
type: pull_request
state: closed
author: carljm
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: cjm/subscript-no-standalone
created_at: 2025-10-09T22:59:23Z
updated_at: 2025-10-09T23:17:32Z
url: https://github.com/astral-sh/ruff/pull/20795
synced_at: 2026-01-12T15:57:10Z
```

# [ty] avoid standalone expressions for simple subscript targets

---

_@carljm_

## Summary

We don't need the RHS of a simple assignment statement of the form `x[0] = ...` to be a standalone expression, if there is just one target and no unpacking. Eliminate these standalone expressions to reduce Salsa memory usage.

## Test Plan

Existing tests.

---

_Label `internal` added by @carljm on 2025-10-09 22:59_

---

_Label `ty` added by @carljm on 2025-10-09 22:59_

---

_Comment by @github-actions[bot] on 2025-10-09 23:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-09 23:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @carljm on 2025-10-09 23:17_

No performance or memory impact, and I think this makes the code invariants slightly more confusing, so not going to merge it.

---

_Closed by @carljm on 2025-10-09 23:17_

---
