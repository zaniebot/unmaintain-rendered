```yaml
number: 20331
title: "[ty] Ensure various special-cased builtin functions are understood as assignable to `Callable`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/builtin-method-callability-3
created_at: 2025-09-10T13:40:43Z
updated_at: 2025-09-10T19:04:31Z
url: https://github.com/astral-sh/ruff/pull/20331
synced_at: 2026-01-10T17:46:22Z
```

# [ty] Ensure various special-cased builtin functions are understood as assignable to `Callable`

---

_Pull request opened by @AlexWaygood on 2025-09-10 13:40_

## Summary

(Stacked on top of #20329 and #20330; review those two first.)

This PR improves the implementation of `Type::bindings()` and `Type::into_callable()` for our `Type::WrapperDescriptor` variant so that all subvariants of `Type::WrapperDescriptor` are understood as assignable to `Callable`. The question "is this variant assignable to `Callable`?" is currently answered by our `Type::into_callable()` method, which currently always returns `None` for this variant. This PR ensures that it always returns `Some()` by creating a new `WrapperDescriptorKind::signatures()` method that can be used from both `Type::bindings()` and `Type::into_callable()`. This both fixes the bug and increases our internal consistency for these types.

## Test Plan

mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-09-10 13:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-10 13:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-10 13:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-10 13:40_

---

_Renamed from "[ty] Ensure various special-cased builtin funcitons are understood as assignable to `Callable`" to "[ty] Ensure various special-cased builtin functions are understood as assignable to `Callable`" by @AlexWaygood on 2025-09-10 13:41_

---

_Comment by @github-actions[bot] on 2025-09-10 13:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 13:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-09-10 18:06_

---

_Merged by @AlexWaygood on 2025-09-10 19:03_

---

_Closed by @AlexWaygood on 2025-09-10 19:03_

---

_Branch deleted on 2025-09-10 19:03_

---
