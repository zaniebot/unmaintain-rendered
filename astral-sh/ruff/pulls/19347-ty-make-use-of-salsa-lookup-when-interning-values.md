```yaml
number: 19347
title: "[ty] Make use of salsa `Lookup` when interning values"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-lookup
created_at: 2025-07-15T07:41:51Z
updated_at: 2025-07-15T12:52:13Z
url: https://github.com/astral-sh/ruff/pull/19347
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Make use of salsa `Lookup` when interning values

---

_Pull request opened by @MichaReiser on 2025-07-15 07:41_

## Summary

Interning a value doesn't require an owned value in Salsa. Instead, a value can implement the `Lookup` trait
so that the owned value is only allocated if the value-to-intern doesn't exist yet.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-07-15 07:41_

---

_Label `ty` added by @MichaReiser on 2025-07-15 07:41_

---

_Comment by @github-actions[bot] on 2025-07-15 07:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-07-15 07:54_

---

_Review requested from @carljm by @MichaReiser on 2025-07-15 07:54_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-15 07:54_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-15 07:54_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-15 07:54_

---

_Merged by @MichaReiser on 2025-07-15 07:54_

---

_Closed by @MichaReiser on 2025-07-15 07:54_

---

_Branch deleted on 2025-07-15 07:54_

---

_@dcreager reviewed on 2025-07-15 12:52_

:+1: nice!

---
