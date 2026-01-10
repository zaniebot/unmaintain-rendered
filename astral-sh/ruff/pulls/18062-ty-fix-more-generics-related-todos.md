```yaml
number: 18062
title: "[ty] Fix more generics-related TODOs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generics-todos
created_at: 2025-05-13T02:34:47Z
updated_at: 2025-05-14T16:26:54Z
url: https://github.com/astral-sh/ruff/pull/18062
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Fix more generics-related TODOs

---

_Pull request opened by @AlexWaygood on 2025-05-13 02:34_

## Summary

Address various generics-related TODOs by converting `.to_instance()` calls into `.to_specialized_instance()` calls.

## Test Plan

Existing mdtests updated.


---

_Review requested from @carljm by @AlexWaygood on 2025-05-13 02:34_

---

_Label `ty` added by @AlexWaygood on 2025-05-13 02:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-13 02:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-13 02:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2242 on 2025-05-13 02:35_

this was necessary or some custom-typeshed tests started panicking because they couldn't find `Exception` and `ExceptionGroup` in the custom typeshed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:2551 on 2025-05-13 02:36_

using `is_subtype_of` rather than `is_assignable_to` here because I think we should infer `BaseExceptionGroup` rather than `ExceptionGroup` if `symbol_ty` is `Unknown` or similar

---

_@AlexWaygood reviewed on 2025-05-13 02:36_

---

_Comment by @github-actions[bot] on 2025-05-13 02:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Actual type `FullBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 650 diagnostics

```
</details>


---

_@dcreager approved on 2025-05-14 16:26_

---

_Merged by @AlexWaygood on 2025-05-14 16:26_

---

_Closed by @AlexWaygood on 2025-05-14 16:26_

---

_Branch deleted on 2025-05-14 16:26_

---
