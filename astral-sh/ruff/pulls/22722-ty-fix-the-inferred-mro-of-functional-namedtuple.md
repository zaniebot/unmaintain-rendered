```yaml
number: 22722
title: "[ty] Fix the inferred MRO of functional namedtuple classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/namedtuple-mro
created_at: 2026-01-19T13:37:35Z
updated_at: 2026-01-19T13:48:52Z
url: https://github.com/astral-sh/ruff/pull/22722
synced_at: 2026-01-19T14:36:03Z
```

# [ty] Fix the inferred MRO of functional namedtuple classes

---

_@AlexWaygood_

## Summary

This is a bugfix PR pulled out of https://github.com/astral-sh/ruff/pull/22718. It fixes an issue in https://github.com/astral-sh/ruff/pull/22327 that I missed during review of that PR.

Currently for a class constructed as follows:

```py
from collections import namedtuple

Point = namedtuple("Point", "x y")
```

we infer that class's MRO as being `(<class 'Point'>, <class 'tuple[Any, Any]'>, <class 'object'>)`. That's actually the MRO that `Point` will have at runtime (which is maybe why I didn't spot the bug before), but it's not the MRO we should infer, because typeshed pretends that `tuple` has a much richer MRO than it does at runtime: typeshed (for very good reasons) says that `tuple` inherits from `Sequence`. Our current MRO inference for functional namedtuples is inconsistent with the MRO we'd infer for a class-based `NamedTuple`, inconsistent with the MRO we infer for the `tuple` class itself, and means that we don't currently consider functional `namedtuple` classes to be subtypes of `Sequence` (but they are!).

This PR fixes those issues.

## Test Plan

mdtests updated and extended


---

_Review requested from @carljm by @AlexWaygood on 2026-01-19 13:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-19 13:37_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-19 13:37_

---

_Label `bug` added by @AlexWaygood on 2026-01-19 13:37_

---

_Label `ty` added by @AlexWaygood on 2026-01-19 13:37_

---

_Review request for @dcreager removed by @AlexWaygood on 2026-01-19 13:39_

---

_Review request for @carljm removed by @AlexWaygood on 2026-01-19 13:39_

---

_Review request for @sharkdp removed by @AlexWaygood on 2026-01-19 13:39_

---

_Review requested from @charliermarsh by @AlexWaygood on 2026-01-19 13:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 13:39_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_@charliermarsh approved on 2026-01-19 13:40_

Oh TIL, thanks.

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 13:40_


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

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-19 13:43_

We'll need to fix the false positives on prefect before merging.

---

_Comment by @charliermarsh on 2026-01-19 13:44_

Sorry that was a joke.

---

_Merged by @AlexWaygood on 2026-01-19 13:48_

---

_Closed by @AlexWaygood on 2026-01-19 13:48_

---

_Branch deleted on 2026-01-19 13:48_

---
