```yaml
number: 20330
title: "[ty] Ensure various special-cased bound methods are understood as assignable to `Callable`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/builtin-method-callability-2
created_at: 2025-09-10T13:16:04Z
updated_at: 2025-09-10T18:58:57Z
url: https://github.com/astral-sh/ruff/pull/20330
synced_at: 2026-01-12T15:56:59Z
```

# [ty] Ensure various special-cased bound methods are understood as assignable to `Callable`

---

_@AlexWaygood_

## Summary

This PR improves the implementation of `Type::bindings()` and `Type::into_callable()` for our `Type::KnownBoundMethod` variant so that all subvariants of `Type::KnownBoundMethod` are understood as assignable to `Callable`. The question "is this variant assignable to `Callable`?" is currently answered by our `Type::into_callable()` method, which currently always returns `None` for this variant. This PR ensures that it always returns `Some()` by creating a new `KnownBoundMethodType::signatures()` method that can be used from both `Type::bindings()` and `Type::into_callable()`. This both fixes the bug and increases our internal consistency for these types.

## Test Plan

mdtests updated


---

_Review requested from @carljm by @AlexWaygood on 2025-09-10 13:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-10 13:16_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-10 13:16_

---

_Label `ty` added by @AlexWaygood on 2025-09-10 13:16_

---

_Comment by @github-actions[bot] on 2025-09-10 13:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 13:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-09-10 18:02_

Nice!

---

_Merged by @AlexWaygood on 2025-09-10 18:58_

---

_Closed by @AlexWaygood on 2025-09-10 18:58_

---

_Branch deleted on 2025-09-10 18:58_

---
