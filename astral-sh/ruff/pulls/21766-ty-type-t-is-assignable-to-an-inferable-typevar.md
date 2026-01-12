```yaml
number: 21766
title: "[ty] `type[T]` is assignable to an inferable typevar"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/type-of-subtyping
created_at: 2025-12-02T20:57:38Z
updated_at: 2025-12-02T23:25:10Z
url: https://github.com/astral-sh/ruff/pull/21766
synced_at: 2026-01-12T15:57:33Z
```

# [ty] `type[T]` is assignable to an inferable typevar

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1712.

---

_Review requested from @carljm by @ibraheemdev on 2025-12-02 20:57_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-02 20:57_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-02 20:57_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-02 20:57_

---

_Label `ty` added by @ibraheemdev on 2025-12-02 20:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 20:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 21:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/flow_runs.py:291:19: error[unresolved-attribute] Object of type `(type[T@_in_process_pause] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@_in_process_pause] & ~AlwaysFalsy)` has no attribute `load`
+ src/prefect/flow_runs.py:291:19: error[unresolved-attribute] Object of type `type[T@_in_process_pause] & ~AlwaysFalsy` has no attribute `load`
- src/prefect/flow_runs.py:306:15: error[unresolved-attribute] Object of type `(type[T@_in_process_pause] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@_in_process_pause] & ~AlwaysFalsy)` has no attribute `save`
+ src/prefect/flow_runs.py:306:15: error[unresolved-attribute] Object of type `type[T@_in_process_pause] & ~AlwaysFalsy` has no attribute `save`
- src/prefect/flow_runs.py:318:34: error[unresolved-attribute] Object of type `(type[T@_in_process_pause] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@_in_process_pause] & ~AlwaysFalsy)` has no attribute `load`
+ src/prefect/flow_runs.py:318:34: error[unresolved-attribute] Object of type `type[T@_in_process_pause] & ~AlwaysFalsy` has no attribute `load`
- src/prefect/flow_runs.py:445:26: error[unresolved-attribute] Object of type `(type[T@suspend_flow_run] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@suspend_flow_run] & ~AlwaysFalsy)` has no attribute `load`
+ src/prefect/flow_runs.py:445:26: error[unresolved-attribute] Object of type `type[T@suspend_flow_run] & ~AlwaysFalsy` has no attribute `load`
- src/prefect/flow_runs.py:455:15: error[unresolved-attribute] Object of type `(type[T@suspend_flow_run] & ~AlwaysTruthy & ~AlwaysFalsy) | (type[T@suspend_flow_run] & ~AlwaysFalsy)` has no attribute `save`
+ src/prefect/flow_runs.py:455:15: error[unresolved-attribute] Object of type `type[T@suspend_flow_run] & ~AlwaysFalsy` has no attribute `save`
- src/prefect/server/services/base.py:102:21: error[invalid-argument-type] Argument to bound method `append` is incorrect: Argument type `list[type[Self@all_services]]` does not satisfy upper bound `list[_T@list]` of type variable `Self`
- src/prefect/utilities/collections.py:206:17: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- src/prefect/utilities/collections.py:210:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[type[T@extract_instances], list[Any]].__getitem__(key: type[T@extract_instances], /) -> list[Any]` cannot be called with key of type `Unknown` on object of type `dict[type[T@extract_instances], list[Any]]`
- Found 3399 diagnostics
+ Found 3396 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/util.py:708:24: error[not-iterable] Object of type `list[type[T@get_subclasses]]` is not iterable
- Found 4487 diagnostics
+ Found 4486 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- misc/python/materialize/util.py:46:12: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `set[_T@set]`, found `set[type[T@all_subclasses]]`
- misc/python/materialize/util.py:46:45: error[not-iterable] Object of type `list[type[T@all_subclasses]]` is not iterable
- misc/python/materialize/zippy/framework.py:75:50: error[unsupported-operator] Operator `not in` is not supported between objects of type `type[Capability]` and `set[type[T@_remove]]`
- Found 3380 diagnostics
+ Found 3377 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~76MB
+     memo metadata = ~80MB


```

</details>




---

_@AlexWaygood approved on 2025-12-02 22:12_

---

_Merged by @ibraheemdev on 2025-12-02 23:25_

---

_Closed by @ibraheemdev on 2025-12-02 23:25_

---

_Branch deleted on 2025-12-02 23:25_

---
