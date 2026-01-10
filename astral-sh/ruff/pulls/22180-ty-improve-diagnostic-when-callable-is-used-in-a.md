```yaml
number: 22180
title: "[ty] Improve diagnostic when `callable` is used in a type expression instead of `collections.abc.Callable` or `typing.Callable`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/callable-hint
created_at: 2025-12-24T18:33:10Z
updated_at: 2025-12-24T19:18:53Z
url: https://github.com/astral-sh/ruff/pull/22180
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Improve diagnostic when `callable` is used in a type expression instead of `collections.abc.Callable` or `typing.Callable`

---

_Pull request opened by @AlexWaygood on 2025-12-24 18:33_

## Summary

I noticed when reviewing the ecosystem report for https://github.com/astral-sh/ruff/pull/22145 that ibis makes this mistake. Mypy [has a special-cased diagnostic for this](https://mypy-play.net/?mypy=latest&python=3.12&gist=6071f0cae0d7a46532070e819bb94a2b), and there's no reason we can't too.

## Test Plan

snapshot added


---

_Label `ty` added by @AlexWaygood on 2025-12-24 18:33_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-24 18:33_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-24 18:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-24 18:33_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-24 18:33_

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 18:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-24 18:34:45.142051595 +0000
+++ new-output.txt	2025-12-24 18:34:47.321063280 +0000
@@ -91,8 +91,8 @@
 annotations_forward_refs.py:55:11: error[invalid-type-form] Module `types` is not valid in a type expression
 annotations_forward_refs.py:80:14: error[unresolved-reference] Name `ClassF` used when not defined
 annotations_forward_refs.py:82:11: error[invalid-type-form] Variable of type `Literal[""]` is not allowed in a type expression
-annotations_forward_refs.py:87:9: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
-annotations_forward_refs.py:89:8: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
+annotations_forward_refs.py:87:9: error[invalid-type-form] Function `int` is not valid in a type expression
+annotations_forward_refs.py:89:8: error[invalid-type-form] Function `int` is not valid in a type expression
 annotations_forward_refs.py:95:1: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
 annotations_forward_refs.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 annotations_generators.py:86:21: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.GeneratorType`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-24 18:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/pytester.py:1578:36: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1578:36: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1582:26: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1582:26: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1590:33: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1590:33: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1590:48: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1590:48: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1590:75: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1590:75: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1597:53: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1597:53: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1602:54: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1602:54: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1608:32: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1608:32: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1608:60: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1608:60: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1608:65: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1608:65: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1622:39: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1622:39: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1622:56: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1622:56: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1636:28: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1636:28: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1640:32: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1640:32: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1655:32: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1655:32: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1677:26: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1677:26: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1678:31: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1678:31: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1678:36: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1678:36: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1679:25: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1679:25: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1742:36: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1742:36: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1750:37: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1750:37: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1761:20: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1761:20: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1761:47: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1761:47: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1761:52: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1761:52: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1761:81: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1761:81: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1783:26: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1783:26: error[invalid-type-form] Function `str` is not valid in a type expression
- src/_pytest/pytester.py:1789:22: error[invalid-type-form] Variable of type `def str(self) -> Unknown` is not allowed in a type expression
+ src/_pytest/pytester.py:1789:22: error[invalid-type-form] Function `str` is not valid in a type expression

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/schemas.py:687:25: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
- src/schemathesis/schemas.py:732:82: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
- src/schemathesis/schemas.py:763:15: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
- src/schemathesis/schemas.py:807:10: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
+ src/schemathesis/schemas.py:687:25: error[invalid-type-form] Function `Case` is not valid in a type expression
+ src/schemathesis/schemas.py:732:82: error[invalid-type-form] Function `Case` is not valid in a type expression
+ src/schemathesis/schemas.py:763:15: error[invalid-type-form] Function `Case` is not valid in a type expression
+ src/schemathesis/schemas.py:807:10: error[invalid-type-form] Function `Case` is not valid in a type expression

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pandera (https://github.com/pandera-dev/pandera)
- pandera/backends/pyspark/checks.py:59:49: error[invalid-type-form] Variable of type `def is_table(obj) -> Unknown` is not allowed in a type expression
+ pandera/backends/pyspark/checks.py:59:49: error[invalid-type-form] Function `is_table` is not valid in a type expression

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

Expression (https://github.com/cognitedata/Expression)
- tests/test_tagged_union.py:331:23: error[invalid-type-form] Variable of type `def Face(suit: Suit, face: Unknown) -> Card` is not allowed in a type expression
+ tests/test_tagged_union.py:331:23: error[invalid-type-form] Function `Face` is not valid in a type expression
- tests/test_tagged_union.py:336:32: error[invalid-type-form] Variable of type `def Face(suit: Suit, face: Unknown) -> Card` is not allowed in a type expression
+ tests/test_tagged_union.py:336:32: error[invalid-type-form] Function `Face` is not valid in a type expression

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/_curses_panel.pyi:17:28: error[invalid-type-form] Variable of type `def window(self) -> Unknown` is not allowed in a type expression
+ mypy/typeshed/stdlib/_curses_panel.pyi:17:28: error[invalid-type-form] Function `window` is not valid in a type expression
- mypy/typeshed/stdlib/_curses_panel.pyi:22:25: error[invalid-type-form] Variable of type `def window(self) -> Unknown` is not allowed in a type expression
+ mypy/typeshed/stdlib/_curses_panel.pyi:22:25: error[invalid-type-form] Function `window` is not valid in a type expression
- mypy/typeshed/stdlib/multiprocessing/pool.pyi:53:66: error[invalid-type-form] Variable of type `def Process(ctx: DefaultContext, *args: Any, **kwds: Any) -> Unknown` is not allowed in a type expression
+ mypy/typeshed/stdlib/multiprocessing/pool.pyi:53:66: error[invalid-type-form] Function `Process` is not valid in a type expression

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/abc.py:1087:62: error[invalid-type-form] Variable of type `def float(self) -> Unknown` is not allowed in a type expression
+ tanjun/abc.py:1087:62: error[invalid-type-form] Function `float` is not valid in a type expression
- tanjun/abc.py:1106:24: error[invalid-type-form] Variable of type `def float(self) -> Unknown` is not allowed in a type expression
+ tanjun/abc.py:1106:24: error[invalid-type-form] Function `float` is not valid in a type expression
- tanjun/context/slash.py:113:62: error[invalid-type-form] Variable of type `def float(self) -> Unknown` is not allowed in a type expression
+ tanjun/context/slash.py:113:62: error[invalid-type-form] Function `float` is not valid in a type expression
- tanjun/context/slash.py:130:24: error[invalid-type-form] Variable of type `def float(self) -> Unknown` is not allowed in a type expression
+ tanjun/context/slash.py:130:24: error[invalid-type-form] Function `float` is not valid in a type expression
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

xarray (https://github.com/pydata/xarray)
- xarray/core/accessor_str.py:295:20: error[invalid-type-form] Variable of type `def slice(self, start: int | Any | None = None, stop: int | Any | None = None, step: int | Any | None = None) -> Unknown` is not allowed in a type expression
+ xarray/core/accessor_str.py:295:20: error[invalid-type-form] Function `slice` is not valid in a type expression

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/_geo_feature.pyi:103:21: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:103:37: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:109:27: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:109:49: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:112:30: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:113:30: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:114:21: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
- src/bokeh/plotting/_geo_feature.pyi:114:37: error[invalid-type-form] Variable of type `Overload[(object: Unknown, dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._core.multiarray._SupportsArray[Unknown], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: Literal[True], ndmin: Literal[0] = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> Unknown, (object: numpy._typing._array_like._SupportsArray[dtype[Unknown]] | _NestedSequence[numpy._typing._array_like._SupportsArray[dtype[Unknown]]], dtype: None = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Unknown] | dtype[Unknown] | _HasDType[dtype[Unknown]] | _HasNumPyDType[dtype[Unknown]], *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Unknown]], (object: Any, dtype: type[Any] | dtype[Any] | _HasDType[dtype[Any]] | ... omitted 6 union elements = None, *, copy: bool | _CopyMode | None = True, order: Literal["K", "A", "C", "F"] | None = "K", subok: bool = False, ndmin: int = 0, ndmax: int = 0, like: _SupportsArrayFunc | None = None) -> ndarray[tuple[Any, ...], dtype[Any]]]` is not allowed in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:103:21: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:103:37: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:109:27: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:109:49: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:112:30: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:113:30: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:114:21: error[invalid-type-form] Function `array` is not valid in a type expression
+ src/bokeh/plotting/_geo_feature.pyi:114:37: error[invalid-type-form] Function `array` is not valid in a type expression

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2803 diagnostics
+ Found 2800 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[Top[(...) -> object]]` is not allowed in a type expression
+ ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Function `callable` is not valid in a type expression: Did you mean `collections.abc.Callable`?
- ibis/backends/tests/test_client.py:456:31: error[invalid-type-form] Variable of type `def schema(pairs: Schema | Mapping[str, str | DataType] | Iterable[tuple[str, str | DataType]] | None = None, *, /, names: Iterable[str] | None = None, types: Iterable[str | DataType] | None = None) -> Schema` is not allowed in a type expression
+ ibis/backends/tests/test_client.py:456:31: error[invalid-type-form] Function `schema` is not valid in a type expression

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/_numba/extensions.py:74:48: error[invalid-type-form] Variable of type `def any(iterable: Iterable[object], /) -> bool` is not allowed in a type expression
+ pandas/core/_numba/extensions.py:74:48: error[invalid-type-form] Function `any` is not valid in a type expression

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

sympy (https://github.com/sympy/sympy)
- sympy/polys/rings.py:1359:44: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1359:44: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1374:49: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1374:49: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1399:44: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1399:44: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1443:49: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1443:49: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1471:54: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1471:54: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1641:29: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1641:29: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1695:34: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1695:34: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1785:42: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1785:42: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:1817:47: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:1817:47: error[invalid-type-form] Function `str` is not valid in a type expression
- sympy/polys/rings.py:2715:68: error[invalid-type-form] Variable of type `def str(self, printer, precedence, exp_pattern, mul_symbol) -> Unknown` is not allowed in a type expression
+ sympy/polys/rings.py:2715:68: error[invalid-type-form] Function `str` is not valid in a type expression

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5107 diagnostics
+ Found 5106 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/assist_pipeline/vad.py:52:28: error[invalid-type-form] Variable of type `def bytes(self) -> Unknown` is not allowed in a type expression
+ homeassistant/components/assist_pipeline/vad.py:52:28: error[invalid-type-form] Function `bytes` is not valid in a type expression
- homeassistant/components/assist_pipeline/vad.py:61:24: error[invalid-type-form] Variable of type `def bytes(self) -> Unknown` is not allowed in a type expression
+ homeassistant/components/assist_pipeline/vad.py:61:24: error[invalid-type-form] Function `bytes` is not valid in a type expression


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-24 18:37_

> ```diff
> - ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[Top[(...) -> object]]` is not allowed in a type expression
> + ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Function `callable` is not valid in a type expression: Did you mean `collections.abc.Callable`?
> ```

Cool that even with the new "Did you mean...?" suggestion, the diagnostic is still more concise than it was previously 😄

---

_@charliermarsh approved on 2025-12-24 19:17_

---

_Merged by @AlexWaygood on 2025-12-24 19:18_

---

_Closed by @AlexWaygood on 2025-12-24 19:18_

---

_Branch deleted on 2025-12-24 19:18_

---
