```yaml
number: 22351
title: "[ty] emit diagnostics for method definitions and other invalid statements in `TypedDict` class bodies"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: typeddict_method_errors
created_at: 2026-01-03T05:11:27Z
updated_at: 2026-01-05T19:28:06Z
url: https://github.com/astral-sh/ruff/pull/22351
synced_at: 2026-01-12T15:57:47Z
```

# [ty] emit diagnostics for method definitions and other invalid statements in `TypedDict` class bodies

---

_@oconnor663_

Fixes https://github.com/astral-sh/ty/issues/2277.

Example:

```py
class Foo(TypedDict):
    42
    a: int = 99
    def baz(self) -> None: ...
```
```
error[invalid-typed-dict-statement]: invalid statement in TypedDict class body
 --> test.py:4:5
  |
3 | class Foo(TypedDict):
4 |     42
  |     ^^
5 |     a: int = 99
6 |     def baz(self) -> None: ...
  |
info: Only annotated declarations (`<name>: <type>`) are allowed.
info: rule `invalid-typed-dict-statement` is enabled by default

error[invalid-typed-dict-statement]: TypedDict items cannot have a value
 --> test.py:5:14
  |
3 | class Foo(TypedDict):
4 |     42
5 |     a: int = 99
  |              ^^
6 |     def baz(self) -> None: ...
  |
info: rule `invalid-typed-dict-statement` is enabled by default

error[invalid-typed-dict-statement]: TypedDict classes cannot have methods
 --> test.py:6:5
  |
4 |     42
5 |     a: int = 99
6 |     def baz(self) -> None: ...
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `invalid-typed-dict-statement` is enabled by default
```

---

_Review requested from @carljm by @oconnor663 on 2026-01-03 05:11_

---

_Review requested from @AlexWaygood by @oconnor663 on 2026-01-03 05:11_

---

_Review requested from @sharkdp by @oconnor663 on 2026-01-03 05:11_

---

_Review requested from @dcreager by @oconnor663 on 2026-01-03 05:11_

---

_Review requested from @MichaReiser by @oconnor663 on 2026-01-03 05:11_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 05:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-05 19:14:41.071627798 +0000
+++ new-output.txt	2026-01-05 19:14:41.439629687 +0000
@@ -946,6 +946,9 @@
 tuples_type_form.py:15:6: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""]]` is not assignable to `tuple[int, int]`
 tuples_type_form.py:25:7: error[invalid-assignment] Object of type `tuple[Literal[1]]` is not assignable to `tuple[()]`
 tuples_type_form.py:36:7: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
+typeddicts_class_syntax.py:29:5: error[invalid-typed-dict-statement] TypedDict class cannot have methods
+typeddicts_class_syntax.py:33:5: error[invalid-typed-dict-statement] TypedDict class cannot have methods
+typeddicts_class_syntax.py:38:5: error[invalid-typed-dict-statement] TypedDict class cannot have methods
 typeddicts_extra_items.py:14:37: error[invalid-key] Unknown key "novel_adaptation" for TypedDict `Movie`: Unknown key "novel_adaptation"
 typeddicts_extra_items.py:15:37: error[invalid-key] Unknown key "year" for TypedDict `Movie`: Unknown key "year"
 typeddicts_extra_items.py:29:5: error[type-assertion-failure] Type `bool` does not match asserted type `str`
@@ -1020,4 +1023,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1022 diagnostics
+Found 1025 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-03 05:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/sanic/context.py:14:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 358 diagnostics
+ Found 357 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
+ src/user_profile.py:50:5: error[invalid-typed-dict-statement] TypedDict class cannot have methods
- Found 62 diagnostics
+ Found 63 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

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
- Found 5527 diagnostics
+ Found 5532 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | Top[Yarn[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Yarn[Any], generic[object]]`

jax (https://github.com/google/jax)
+ jax/_src/tpu_custom_call.py:128:35: error[invalid-typed-dict-statement] TypedDict item cannot have a value
+ jax/_src/tpu_custom_call.py:130:3: error[invalid-typed-dict-statement] TypedDict class cannot have methods
- Found 2796 diagnostics
+ Found 2798 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5156 diagnostics
+ Found 5155 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-03 07:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-03 07:57_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 08:02_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 1 | 8 |
| `invalid-argument-type` | 4 | 0 | 3 |
| `invalid-assignment` | 0 | 0 | 5 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `invalid-typed-dict-statement` | 3 | 0 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **13** | **3** | **19** |


**[Full report with detailed diff](https://afba1b20.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://afba1b20.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @AlexWaygood on 2026-01-03 09:02_

Nice! I think we should allow `...` in `TypedDict` class bodies, though. A strict reading of the spec would imply that they're not allowed, but I think the spec should just be amended here. Other type checkers allow `...` in `TypedDict` class bodies, and it's commonly used in Python code (especially stub files) as an alternative to `pass`.

---

_@AlexWaygood reviewed on 2026-01-03 14:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1 on 2026-01-03 14:34_

Could you add a test for a `TypedDict` class that has "attribute docstrings" in it? I nearly suggested that we should only allow a string-literal node if it's the very first statement in the class body -- but that would be overly restrictive, because it would ban things like this, which I think are reasonable:

```py
class DocumentedTypedDict(TypedDict):
    """Class docstring"""

    x: int
    """docs for the x field"""

    y: int
    """docs for the y field"""
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2256 on 2026-01-05 17:58_

nit: this feels slightly more grammatical to me

```suggestion
    # error: [invalid-typed-dict-statement] "TypedDict item cannot have a value"
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:2178 on 2026-01-05 17:59_

```suggestion
    /// `TypedDict` class bodies aren't allowed to contain any other types of statements. For
    /// example, method definitions and field values aren't allowed. None of these will be available
    /// on "instances of the TypedDict" at runtime (as `dict` is the runtime class of all
    /// "TypedDict instances").
```

---

_@AlexWaygood approved on 2026-01-05 18:00_

LGTM!

---

_Comment by @oconnor663 on 2026-01-05 19:13_

(Sorry for all the CI churn here. I've been sloppy about running Clippy etc. before I push today.)

---

_Merged by @oconnor663 on 2026-01-05 19:28_

---

_Closed by @oconnor663 on 2026-01-05 19:28_

---

_Branch deleted on 2026-01-05 19:28_

---
