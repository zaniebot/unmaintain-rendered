```yaml
number: 22480
title: "[ty] Support own instance members for `type(...)` classes"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: charlie/dyn
head: charlie/dyn-members
created_at: 2026-01-09T15:56:47Z
updated_at: 2026-01-11T02:47:47Z
url: https://github.com/astral-sh/ruff/pull/22480
synced_at: 2026-01-12T02:12:03Z
```

# [ty] Support own instance members for `type(...)` classes

---

_Pull request opened by @charliermarsh on 2026-01-09 15:56_

## Summary

Addresses https://github.com/astral-sh/ruff/pull/22291#discussion_r2674467950.


---

_Comment by @astral-sh-bot[bot] on 2026-01-09 15:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-09 16:00_

---

_Label `ty` added by @charliermarsh on 2026-01-09 16:00_

---

_Renamed from "Support own instance members for `type(...)` classes" to "[ty] Support own instance members for `type(...)` classes" by @charliermarsh on 2026-01-09 16:00_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 16:06_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 4 | 0 |
| `invalid-return-type` | 1 | 0 | 2 |
| `invalid-argument-type` | 0 | 0 | 1 |
| **Total** | **1** | **4** | **3** |


**[Full report with detailed diff](https://b3e85137.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://b3e85137.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2026-01-09 16:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
- src/black/linegen.py:775:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- src/black/linegen.py:785:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- src/black/linegen.py:794:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- src/black/linegen.py:796:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- Found 58 diagnostics
+ Found 54 diagnostics

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
- Found 5362 diagnostics
+ Found 5367 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5169 diagnostics
+ Found 5170 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @carljm by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-09 17:03_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-09 17:03_

---

_@AlexWaygood reviewed on 2026-01-09 17:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:107 on 2026-01-09 17:21_

If the class has a dynamic namespace dictionary (either the argument is not a dictionary literal, or not all the keys in the dictionary literal are string literals), should we just store a marker on the `DynamicClassLiteral` that records this, and say that it's a dynamic type that has arbitrary attributes available?

```py
from typing import Any
from ty_extensions import reveal_mro

class Base: ...

def f(attributes: dict[str, Any]):
    X = type("X", (Base,), attributes)

    reveal_mro(X)  # revealed: (<class 'Base'>, <class 'object'>)
    X().foo  # maybe this should be allowed?
```

I'd also be interested in this test, where a `TypedDict` is passed as the namespace argument... we know that an instance of a `TypedDict` can only ever have string-literal keys, but we don't know that it _only_ has those keys ([by default `TypedDict`s are "open"](https://typing.python.org/en/latest/spec/typeddict.html)):

```py
from typing import TypedDict

class Foo(TypedDict):
    z: int

def g(attributes: Foo):
    Y = type("Y", (), attributes)
```

Maybe in this case, we should infer that instances of `Y` have a `z` attribute of type `int`, but they also might have arbitrary other attributes as well (which are all `Unknown`), reflecting the fact that we know the `z` key in `Foo` instances are always of type `int` but that `Foo` instances can also have arbitrary other keys too?

As always, we don't have to tackle _all_ these questions _right_ now, but it would be great to add these tests so we record our current behaviour. I'm just trying to think through some edge cases

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-09 21:00_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-09 21:00_

---

_Converted to draft by @charliermarsh on 2026-01-09 22:04_

---

_Marked ready for review by @charliermarsh on 2026-01-10 01:04_

---

_@AlexWaygood reviewed on 2026-01-10 13:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6069 on 2026-01-10 13:07_

Rather than checking whether any key is not a string-literal AST node, what we would usually do in this situation is check if the inferred _type_ of any key is not a string-literal _type_

---

_@AlexWaygood reviewed on 2026-01-10 13:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:152 on 2026-01-10 13:09_

We don't support the PEP-728 `closed=True` keyword argument to `TypedDict` yet (https://typing.python.org/en/latest/spec/typeddict.html#openness) but we should add some x-failing tests here so we remember to update our behaviour when we do add support for them.

I.e., the namespace for `X` should not be marked as dynamic in this situation:

```py
from typing import TypedDict

class MyNamespace(TypedDict, closed=True):
    x: int
    y: str

def _(ns: MyNamespace):
    X = type("X", (), ns)
```

---
