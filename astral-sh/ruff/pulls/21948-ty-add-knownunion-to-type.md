```yaml
number: 21948
title: "[ty] Add `KnownUnion::to_type()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/knownunion
created_at: 2025-12-12T14:01:33Z
updated_at: 2025-12-12T14:06:36Z
url: https://github.com/astral-sh/ruff/pull/21948
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Add `KnownUnion::to_type()`

---

_@AlexWaygood_

## Summary

A small refactor followup to https://github.com/astral-sh/ruff/pull/21886. Now that we have a `KnownUnion` enum, we can use it in more places to make our code a bit more DRY

## Test Plan

pure refactor; no added tests


---

_Review requested from @carljm by @AlexWaygood on 2025-12-12 14:01_

---

_Label `internal` added by @AlexWaygood on 2025-12-12 14:01_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-12 14:01_

---

_Label `ty` added by @AlexWaygood on 2025-12-12 14:01_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-12 14:01_

---

_@AlexWaygood reviewed on 2025-12-12 14:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7290 on 2025-12-12 14:02_

whether or not the class is a `TypedDict` is also checked in the `Type::instance()` call. Checking whether the class-literal/generic-alias is a `TypedDict` before calling `Type::instance()` is redundant.

---

_@MichaReiser approved on 2025-12-12 14:02_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 14:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 14:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-12-12 14:06_

---

_Closed by @AlexWaygood on 2025-12-12 14:06_

---

_Branch deleted on 2025-12-12 14:06_

---
