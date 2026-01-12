```yaml
number: 20079
title: "[ty] Completely ignore typeshed's stub for `Any`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/remove-variant
created_at: 2025-08-25T09:55:53Z
updated_at: 2025-08-25T14:27:56Z
url: https://github.com/astral-sh/ruff/pull/20079
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Completely ignore typeshed's stub for `Any`

---

_@AlexWaygood_

## Summary

`Any` is a class (on most versions of Python), but it is not a normal class. Its many differences to normal classes include:
- It cannot be instantiated
- It cannot be used as a metaclass
- It cannot be used in `isinstance()` checks
- `Any` in a type expression does not mean "any object whose `__class__` is either the class `Any` or a class that has `Any` in its MRO".

These many differences to normal classes result in us having lots of special-case branches just for `Any`. It seems cleaner here if we just override typeshed's stubs completely for `Any`, similar to what we do for `NamedTuple`. Excluding the change I've made here to disallow `Any` as the second argument to `isinstance()` (which is new functionality), this PR is a net reduction in code, and improves our understanding of the semantics of the symbol `typing.Any` itself at runtime.

## Test Plan

mdtests


---

_Label `ty` added by @AlexWaygood on 2025-08-25 09:55_

---

_Renamed from "[ty] Completely ignore typeshed's stub for Any" to "[ty] Completely ignore typeshed's stub for `Any`" by @AlexWaygood on 2025-08-25 09:57_

---

_Comment by @github-actions[bot] on 2025-08-25 09:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-25 10:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-08-25 10:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:188 on 2025-08-25 10:51_

this comment was only accurate for `issubclass(x, Any)`, since `isinstance(x, Any)` always raises an exception

---

_Marked ready for review by @AlexWaygood on 2025-08-25 10:52_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-25 10:52_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-25 10:52_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-25 10:52_

---

_@sharkdp approved on 2025-08-25 13:40_

Very nice, thank you!

---

_Merged by @AlexWaygood on 2025-08-25 14:27_

---

_Closed by @AlexWaygood on 2025-08-25 14:27_

---

_Branch deleted on 2025-08-25 14:27_

---
