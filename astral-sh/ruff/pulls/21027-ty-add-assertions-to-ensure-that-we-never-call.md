```yaml
number: 21027
title: "[ty] Add assertions to ensure that we never call `KnownClass::Tuple.to_instance()` or similar"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-to-instance
created_at: 2025-10-22T10:49:04Z
updated_at: 2025-10-22T11:07:02Z
url: https://github.com/astral-sh/ruff/pull/21027
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Add assertions to ensure that we never call `KnownClass::Tuple.to_instance()` or similar

---

_@AlexWaygood_

## Summary

This came up in https://github.com/astral-sh/ruff/pull/20988. It's usually a mistake to call `KnownClass::Tuple.to_instance()`; it will usually be more efficient and more correct to call `Type::homogeneous_tuple()` or `Type::heterogeneous_tuple()` instead. Add some debug assertions to ensure that we don't make this mistake.

I considered making these non-debug assertions, as the equality check _should_ be pretty cheap, but `KnownClass::to_instance()` is a pretty hot method, and I think it's exceedingly likely that debug assertions will catch this bug cropping up in the future.

I also removed the private helper method `Type::non_tuple_instance()` from `instance.rs`. It wasn't a zero-cost abstraction, it didn't save that much code, and it had to have a big warning doc-comment above it telling us never to make it public to code outside the module. All things considered, it was no longer worth it to keep it around.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-10-22 10:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-22 10:49_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-22 10:49_

---

_Label `internal` added by @AlexWaygood on 2025-10-22 10:49_

---

_Label `ty` added by @AlexWaygood on 2025-10-22 10:49_

---

_Comment by @github-actions[bot] on 2025-10-22 10:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@sharkdp approved on 2025-10-22 10:51_

Thank you

---

_Comment by @github-actions[bot] on 2025-10-22 10:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-10-22 11:07_

---

_Closed by @AlexWaygood on 2025-10-22 11:07_

---

_Branch deleted on 2025-10-22 11:07_

---
