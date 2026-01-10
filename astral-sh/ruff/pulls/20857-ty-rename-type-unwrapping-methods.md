```yaml
number: 20857
title: "[ty] Rename Type unwrapping methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/rename-type-unwrapping-functions
created_at: 2025-10-14T07:45:18Z
updated_at: 2025-10-14T07:53:31Z
url: https://github.com/astral-sh/ruff/pull/20857
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Rename Type unwrapping methods

---

_Pull request opened by @sharkdp on 2025-10-14 07:45_

## Summary

Rename "unwrapping" methods on `Type` from e.g. `Type::into_class_literal` to `Type::as_class_literal`. I personally find that name more intuitive, since no transformation of any kind is happening. We are just unwrapping from certain enum variants. An alternative would be `try_as_class_literal`, which would follow the [`strum` naming scheme](https://docs.rs/strum/latest/strum/derive.EnumTryAs.html), but is slightly longer.

Also rename `Type::into_callable` to `Type::try_upcast_to_callable`. Note that I intentionally kept names like `FunctionType::into_callable_type`, because those return `CallableType`, not `Option<Type<…>>`.

## Test Plan

Pure refactoring

---

_Label `internal` added by @sharkdp on 2025-10-14 07:45_

---

_Review requested from @carljm by @sharkdp on 2025-10-14 07:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-14 07:45_

---

_Label `ty` added by @sharkdp on 2025-10-14 07:45_

---

_Review requested from @dcreager by @sharkdp on 2025-10-14 07:45_

---

_Comment by @github-actions[bot] on 2025-10-14 07:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 07:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-10-14 07:50_

---

_Merged by @sharkdp on 2025-10-14 07:53_

---

_Closed by @sharkdp on 2025-10-14 07:53_

---

_Branch deleted on 2025-10-14 07:53_

---
