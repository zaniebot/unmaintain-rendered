```yaml
number: 22134
title: "[ty] Add support for narrowing on tuple match cases"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/narrow
created_at: 2025-12-22T02:36:06Z
updated_at: 2025-12-30T22:58:23Z
url: https://github.com/astral-sh/ruff/pull/22134
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Add support for narrowing on tuple match cases

---

_Pull request opened by @charliermarsh on 2025-12-22 02:36_

## Summary

Closes https://github.com/astral-sh/ty/issues/561.


---

_Label `ty` added by @charliermarsh on 2025-12-22 02:36_

---

_Comment by @astral-sh-bot[bot] on 2025-12-22 02:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-30 22:56:38.132743009 +0000
+++ new-output.txt	2025-12-30 22:56:38.463750070 +0000
@@ -929,8 +929,8 @@
 tuples_type_compat.py:101:13: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
 tuples_type_compat.py:106:13: error[type-assertion-failure] Type `tuple[str, str] | tuple[int, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
 tuples_type_compat.py:111:13: error[type-assertion-failure] Type `tuple[int, str, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
-tuples_type_compat.py:126:13: error[type-assertion-failure] Type `tuple[int | str, str]` does not match asserted type `tuple[int | str, int | str]`
-tuples_type_compat.py:129:13: error[type-assertion-failure] Type `tuple[int | str, int]` does not match asserted type `tuple[int | str, int | str]`
+tuples_type_compat.py:127:13: error[type-assertion-failure] Type `tuple[int | str, int | str]` does not match asserted type `tuple[int | str, str]`
+tuples_type_compat.py:130:13: error[type-assertion-failure] Type `tuple[int | str, int | str]` does not match asserted type `tuple[int | str, int]`
 tuples_type_compat.py:157:6: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[""]]` is not assignable to `tuple[int, str]`
 tuples_type_compat.py:162:6: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[1], Literal[""]]` is not assignable to `tuple[int, *tuple[str, ...]]`
 tuples_type_compat.py:163:6: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[1]]` is not assignable to `tuple[int, *tuple[str, ...]]`

```

</details>




---

_Comment by @charliermarsh on 2025-12-22 02:38_

Given the motivating example:
```python
def func(subj: tuple[int | str, int | str]):
    match subj:
        case x, str():
            reveal_type(subj)  # tuple[int | str, str]
        case y:
            reveal_type(subj)  # tuple[int | str, int]
```

This PR doesn't fully solve this case, because `case y:` is revealed as `tuple[int | str, int | str] & ~tuple[int | str, str]`, but the type isn't "simplified".

---

_Comment by @astral-sh-bot[bot] on 2025-12-22 02:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/utils.py:2110:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 216 diagnostics
+ Found 217 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1228:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5027 diagnostics
+ Found 5026 diagnostics


```

</details>


No memory usage changes detected ✅



---
