```yaml
number: 20124
title: "[ty] add cycle detection for find_legacy_typevars"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/findlegacycycle
created_at: 2025-08-28T02:42:03Z
updated_at: 2025-08-28T16:55:10Z
url: https://github.com/astral-sh/ruff/pull/20124
synced_at: 2026-01-12T15:56:55Z
```

# [ty] add cycle detection for find_legacy_typevars

---

_@carljm_

## Summary

Add cycle detection to the `find_legacy_typevars` type method.

## Test Plan

Added mdtest that stack overflowed without this.


---

_Label `ty` added by @carljm on 2025-08-28 02:42_

---

_Comment by @github-actions[bot] on 2025-08-28 02:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 02:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-08-28 03:05_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-28 03:05_

---

_Review requested from @sharkdp by @carljm on 2025-08-28 03:05_

---

_Review requested from @dcreager by @carljm on 2025-08-28 03:05_

---

_@AlexWaygood approved on 2025-08-28 10:16_

---

_@dcreager approved on 2025-08-28 14:32_

This is great, thank you!

---

_Merged by @carljm on 2025-08-28 16:55_

---

_Closed by @carljm on 2025-08-28 16:55_

---

_Branch deleted on 2025-08-28 16:55_

---
