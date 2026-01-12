```yaml
number: 20037
title: "[ty] rename BareTypeAliasType to ManualPEP695TypeAliasType"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/rename-bare
created_at: 2025-08-22T05:53:39Z
updated_at: 2025-08-22T14:40:32Z
url: https://github.com/astral-sh/ruff/pull/20037
synced_at: 2026-01-12T15:56:53Z
```

# [ty] rename BareTypeAliasType to ManualPEP695TypeAliasType

---

_@carljm_

## Summary

Rename `TypeAliasType::Bare` to `TypeAliasType::ManualPEP695`, and `BareTypeAliasType` to `ManualPEP695TypeAliasType`.

Why?

Both existing variants of `TypeAliasType` are specific to features added in PEP 695 (which introduced both the `type` statement and `types.TypeAliasType`), so it doesn't make sense to name one with the name `PEP695` and not the other.

A "bare" type alias, in my mind, is a legacy type alias like `IntOrStr = int | str`, which is "bare" in that there is nothing at all distinguishing it as a type alias. I will want to use the "bare" name for this variant, in a future PR.

The renamed variant here describes a type alias created with `IntOrStr = types.TypeAliasType("IntOrStr", int | str)`, which is not "bare", it's just "manually" instantiated instead of using the `type` statement syntax sugar. (This is useful when using the `typing_extensions` backport of `TypeAliasType` on older Python versions.)

## Test Plan

Pure rename, existing tests pass.


---

_Label `internal` added by @carljm on 2025-08-22 05:53_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-22 05:53_

---

_Review requested from @sharkdp by @carljm on 2025-08-22 05:53_

---

_Label `ty` added by @carljm on 2025-08-22 05:53_

---

_Review requested from @dcreager by @carljm on 2025-08-22 05:53_

---

_Comment by @github-actions[bot] on 2025-08-22 05:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-22 05:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-08-22 06:34_

---

_@AlexWaygood approved on 2025-08-22 07:57_

---

_Merged by @carljm on 2025-08-22 14:40_

---

_Closed by @carljm on 2025-08-22 14:40_

---

_Branch deleted on 2025-08-22 14:40_

---
