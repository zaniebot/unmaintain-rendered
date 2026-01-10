```yaml
number: 19430
title: "[ty] Remove the `scope` from `TypeInference` in release builds"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
base: main
head: micha/remove-scope-from-type-inference
created_at: 2025-07-19T15:10:03Z
updated_at: 2025-07-20T08:40:29Z
url: https://github.com/astral-sh/ruff/pull/19430
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Remove the `scope` from `TypeInference` in release builds

---

_Pull request opened by @MichaReiser on 2025-07-19 15:10_

## Summary

The `scope` is only used during inference and for debug assertions.

This won't save much memory but there's also no reason for storing it :) 

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-07-19 15:10_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-19 15:10_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-19 15:10_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-19 15:10_

---

_Label `internal` added by @MichaReiser on 2025-07-19 15:10_

---

_Label `ty` added by @MichaReiser on 2025-07-19 15:10_

---

_Comment by @github-actions[bot] on 2025-07-19 15:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-07-20 08:40_

I'll  roll this into another PR.  

---

_Closed by @MichaReiser on 2025-07-20 08:40_

---
