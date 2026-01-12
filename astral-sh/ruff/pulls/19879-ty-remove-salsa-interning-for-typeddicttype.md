```yaml
number: 19879
title: "[ty] Remove Salsa interning for `TypedDictType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/typeddict-cache
created_at: 2025-08-12T13:12:36Z
updated_at: 2025-08-12T13:35:27Z
url: https://github.com/astral-sh/ruff/pull/19879
synced_at: 2026-01-12T15:56:49Z
```

# [ty] Remove Salsa interning for `TypedDictType`

---

_@AlexWaygood_

## Summary

It doesn't seem like Salsa interning is buying us much here: `TypedDictType` is just a thin wrapper around `ClassType`, which is already `Copy`.

## Test Plan

Existing tests


---

_Label `ty` added by @AlexWaygood on 2025-08-12 13:12_

---

_Comment by @github-actions[bot] on 2025-08-12 13:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-12 13:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-12 13:24_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-12 13:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-12 13:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-12 13:24_

---

_Label `internal` added by @AlexWaygood on 2025-08-12 13:24_

---

_@MichaReiser approved on 2025-08-12 13:27_

---

_Merged by @AlexWaygood on 2025-08-12 13:35_

---

_Closed by @AlexWaygood on 2025-08-12 13:35_

---

_Branch deleted on 2025-08-12 13:35_

---
