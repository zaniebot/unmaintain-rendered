```yaml
number: 20240
title: "[ty] Minor cleanups"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/class-ctx-refactor
created_at: 2025-09-04T15:42:27Z
updated_at: 2025-09-04T17:25:50Z
url: https://github.com/astral-sh/ruff/pull/20240
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Minor cleanups

---

_Pull request opened by @AlexWaygood on 2025-09-04 15:42_

## Summary

Two minor cleanups:
- Return `Option<ClassType>` rather than `Option<ClassLiteral>` from `TypeInferenceBuilder::class_context_of_current_method`. Now that `ClassType::is_protocol` exists as a method as well as `ClassLiteral::is_protocol`, this simplifies most of the call-sites of the `class_context_of_current_method()` method.
- Make more use of the `MethodDecorator::try_from_fn_type` method in `class.rs`. Under the hood, this method uses the new methods `FunctionType::is_classmethod()` and `FunctionType::is_staticmethod()` that @sharkdp recently added, so it gets the semantics more precisely correct than the code it's replacing in `infer.rs` (by accounting for implicit staticmethods/classmethods as well as explicit ones). By using these methods we can delete some code elsewhere (the `FunctionDecorators::from_decorator_types()` constructor)

## Test Plan

Existing tests


---

_Label `internal` added by @AlexWaygood on 2025-09-04 15:42_

---

_Label `ty` added by @AlexWaygood on 2025-09-04 15:42_

---

_Comment by @github-actions[bot] on 2025-09-04 15:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-04 15:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-09-04 16:01_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-04 16:01_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-04 16:01_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-04 16:01_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-09-04 16:01_

---

_@carljm approved on 2025-09-04 17:25_

---

_Merged by @carljm on 2025-09-04 17:25_

---

_Closed by @carljm on 2025-09-04 17:25_

---

_Branch deleted on 2025-09-04 17:25_

---
