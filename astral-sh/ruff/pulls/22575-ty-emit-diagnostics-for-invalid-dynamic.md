```yaml
number: 22575
title: "[ty] Emit diagnostics for invalid dynamic namedtuple fields"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/func-diag
created_at: 2026-01-14T16:04:29Z
updated_at: 2026-01-14T18:28:25Z
url: https://github.com/astral-sh/ruff/pull/22575
synced_at: 2026-01-14T18:48:06Z
```

# [ty] Emit diagnostics for invalid dynamic namedtuple fields

---

_@charliermarsh_

## Summary

Removes some TODOs from the dynamic namedtuple implementation.

---

_Label `ty` added by @charliermarsh on 2026-01-14 16:04_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 16:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-14 18:23:58.093901341 +0000
+++ new-output.txt	2026-01-14 18:23:58.442904532 +0000
@@ -762,6 +762,10 @@
 namedtuples_define_functional.py:37:21: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
 namedtuples_define_functional.py:42:18: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["1"]`
 namedtuples_define_functional.py:43:15: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
+namedtuples_define_functional.py:52:25: error[invalid-named-tuple] Duplicate field name `a` in `namedtuple()`: Field `a` already defined; will raise `ValueError` at runtime
+namedtuples_define_functional.py:53:25: error[invalid-named-tuple] Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime
+namedtuples_define_functional.py:54:25: error[invalid-named-tuple] Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime
+namedtuples_define_functional.py:55:25: error[invalid-named-tuple] Field name `_d` in `namedtuple()` cannot start with an underscore: Will raise `ValueError` at runtime
 namedtuples_define_functional.py:69:1: error[missing-argument] No argument provided for required parameter `a`
 namedtuples_type_compat.py:22:23: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, int]`
 namedtuples_type_compat.py:23:28: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, str, str]`
@@ -1049,4 +1053,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1051 diagnostics
+Found 1055 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-14 16:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5391 diagnostics
+ Found 5396 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1826 diagnostics
+ Found 1824 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14506 diagnostics
+ Found 14505 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2026-01-14 16:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6830 on 2026-01-14 16:17_

Looks like claude still doesn't know how to do let chains 😅 

---

_@MichaReiser reviewed on 2026-01-14 16:18_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6829 on 2026-01-14 16:18_

I think you can use `seen_names.insert(name_str)` here. This adds the name if it doesn't exist yet and otherwise does nothing.

I'd also be somewhat inclined to not build the hash set and instead use a window iterating over `field_names` to see if the same name appears anywhere later in `field_names`. Unless we assume that `field_names` is very large, in which case we want to avoid the `O(n^2)` search

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6842 on 2026-01-14 16:21_

I don't find those comments useful at all. They just repeat the if condition

---

_@MichaReiser reviewed on 2026-01-14 16:21_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6908 on 2026-01-14 16:21_

NIt: let chains

---

_@MichaReiser reviewed on 2026-01-14 16:21_

---

_@charliermarsh reviewed on 2026-01-14 16:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6842 on 2026-01-14 16:28_

I like them :(

---

_Marked ready for review by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @carljm by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-14 16:34_

---

_Converted to draft by @charliermarsh on 2026-01-14 16:51_

---

_Comment by @charliermarsh on 2026-01-14 17:06_

(Will mark for review once upstream merges + rebased + addressed Micha's initial comments.)

---

_Marked ready for review by @charliermarsh on 2026-01-14 18:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6866 on 2026-01-14 18:21_

that'll be quite a long message; let's split it into two

```suggestion
                            diagnostic.set_primary_message(format_args!(
                                "Got {defaults_count} default values but only {num_fields} field names"
                            ));
                            diagnostic.info("This will raise `TypeError` at runtime");
```

---

_@AlexWaygood approved on 2026-01-14 18:22_

---
