```yaml
number: 22322
title: "[ty] Inform literal promotion with variance of inferred argument types"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/variance-in-argument
created_at: 2026-01-01T01:37:14Z
updated_at: 2026-01-09T22:12:53Z
url: https://github.com/astral-sh/ruff/pull/22322
synced_at: 2026-01-12T15:57:47Z
```

# [ty] Inform literal promotion with variance of inferred argument types

---

_@ibraheemdev_

## Summary

This also indirectly implements https://github.com/astral-sh/ruff/pull/21905. Similar how to we perform reverse inference, we can perform regular inference by recursing one generic layer at a time, and keep track of the variance manually.

Resolves https://github.com/astral-sh/ty/issues/1815.

---

_Label `ty` added by @ibraheemdev on 2026-01-01 01:37_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 01:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-01 01:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Argument type `str | _T0 | bytes` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Argument type `str | _T0 | bytes` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Argument type `str | _T0 | bytes` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/inference.py:379:72: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[str]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (RestrictLexicon & Top[dict[Unknown, Unknown]]) | dict[str, RestrictLexicon]`
- Found 416 diagnostics
+ Found 417 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

mypy (https://github.com/python/mypy)
- mypy/config_parser.py:707:56: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- mypy/config_parser.py:713:57: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 1756 diagnostics
+ Found 1754 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ expression/collections/maptree.py:113:25: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
+ expression/collections/maptree.py:121:27: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
+ expression/collections/maptree.py:136:29: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
+ expression/collections/maptree.py:141:61: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
- Found 205 diagnostics
+ Found 209 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/history/datahandlers/jsondatahandler.py:42:9: error[no-matching-overload] No overload of bound method `to_json` matches arguments
- Found 681 diagnostics
+ Found 680 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/clients.py:2801:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> Coroutine[Any, Any, _T@get_callback_override] | _T@get_callback_override) | None`, found `Unknown | ((...) -> Coroutine[Any, Any, _T@get_callback_override | Coroutine[Any, Any, _T@get_callback_override]] | _T@get_callback_override) | None`
+ tanjun/clients.py:2801:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> Coroutine[Any, Any, _T@get_callback_override] | _T@get_callback_override) | None`, found `Unknown | ((...) -> Coroutine[Any, Any, Coroutine[Any, Any, _T@get_callback_override] | _T@get_callback_override] | _T@get_callback_override) | None`
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, Coroutine[Any, Any, _T@cached_inject] | _T@cached_inject] | _T@cached_inject`

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
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
- src/user_profile.py:145:44: error[invalid-assignment] Invalid assignment to key "screenshot_on" with declared type `list[Literal["split", "start", "pause", "reset", "skip", "undo"]]` on TypedDict `UserProfileDict`: value of type `list[str]`
- Found 63 diagnostics
+ Found 62 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
+ python/egglog/egraph.py:2260:35: error[invalid-argument-type] Argument to function `__new__` is incorrect: Argument type `COST@_CostModel` does not satisfy upper bound `_Cost` of type variable `_COST`
+ python/egglog/egraph.py:2260:63: error[invalid-argument-type] Argument to function `__new__` is incorrect: Argument type `COST@_CostModel` does not satisfy upper bound `_Cost` of type variable `_COST`
- Found 1486 diagnostics
+ Found 1488 diagnostics

altair (https://github.com/vega/altair)
+ altair/utils/schemapi.py:1132:20: warning[redundant-cast] Value is already of type `Self@copy`
- Found 1061 diagnostics
+ Found 1062 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/embed/bundle.py:184:37: error[invalid-argument-type] Argument to bound method `clone` is incorrect: Expected `list[Literal["bokeh", "bokeh-gl", "bokeh-widgets", "bokeh-tables", "bokeh-mathjax", "bokeh-api"]] | None`, found `list[str]`
- src/bokeh/plotting/_figure.py:213:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[Any | Literal["below"]]`
+ src/bokeh/plotting/_figure.py:213:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[Any | str]`
- src/bokeh/plotting/_figure.py:214:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[Any | Literal["left"]]`
+ src/bokeh/plotting/_figure.py:214:55: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `Literal["above", "below", "left", "right"] | None`, found `Unknown | Nullable[Any | str]`
- src/bokeh/plotting/_figure.py:221:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Drag | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
+ src/bokeh/plotting/_figure.py:221:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Drag | str | None`, found `Unknown | Nullable[Any | str]`
- src/bokeh/plotting/_figure.py:222:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `list[InspectTool] | InspectTool | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
+ src/bokeh/plotting/_figure.py:222:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `list[InspectTool] | InspectTool | str | None`, found `Unknown | Nullable[Any | str]`
- src/bokeh/plotting/_figure.py:223:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Scroll | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
+ src/bokeh/plotting/_figure.py:223:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Scroll | str | None`, found `Unknown | Nullable[Any | str]`
- src/bokeh/plotting/_figure.py:224:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Tap | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
+ src/bokeh/plotting/_figure.py:224:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `Tap | str | None`, found `Unknown | Nullable[Any | str]`
- src/bokeh/plotting/_figure.py:225:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `GestureTool | str | None`, found `Unknown | Nullable[Any | Literal["auto"]]`
+ src/bokeh/plotting/_figure.py:225:13: error[invalid-argument-type] Argument to function `process_active_tools` is incorrect: Expected `GestureTool | str | None`, found `Unknown | Nullable[Any | str]`
- Found 880 diagnostics
+ Found 879 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/bigquery/tests/unit/test_compiler.py:273:26: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/bigquery/tests/unit/test_compiler.py:273:26: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str | Unknown]`
- ibis/backends/databricks/tests/test_datatypes.py:19:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/databricks/tests/test_datatypes.py:19:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str | Struct]`
- ibis/backends/flink/tests/test_ddl.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Unknown]`
- ibis/backends/flink/tests/test_ddl.py:456:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:456:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Int64 | Float64]`
- ibis/backends/flink/tests/test_ddl.py:484:34: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:484:34: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Int64]`
- ibis/backends/flink/tests/test_ddl.py:486:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:486:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Int64 | Float64]`
- ibis/backends/oracle/tests/test_datatypes.py:29:45: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/oracle/tests/test_datatypes.py:29:45: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str | Unknown]`
- ibis/backends/oracle/tests/test_datatypes.py:53:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/oracle/tests/test_datatypes.py:53:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str | Unknown]`
- ibis/backends/tests/test_join.py:207:57: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/tests/test_join.py:207:57: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str | Unknown]`
- ibis/expr/tests/test_api.py:20:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_api.py:20:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str | Unknown]`
- ibis/expr/tests/test_format.py:366:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_format.py:366:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[str, Unknown]`
- ibis/expr/tests/test_newrels.py:66:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:66:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Boolean | Int64 | Float64 | String]`
- ibis/expr/tests/test_newrels.py:86:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:86:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64]`
- ibis/expr/tests/test_newrels.py:91:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:91:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64]`
- ibis/expr/tests/test_newrels.py:96:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:96:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64]`
- ibis/expr/tests/test_newrels.py:107:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:107:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64]`
- ibis/expr/tests/test_newrels.py:116:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:116:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int8 | Float64 | Int32]`
- ibis/expr/tests/test_schema.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Float64 | Boolean]`
- ibis/expr/tests/test_schema.py:172:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:172:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/expr/tests/test_schema.py:224:5: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:224:5: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/expr/tests/test_schema.py:317:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:317:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | Array[Unknown]]`
- ibis/expr/tests/test_schema.py:325:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:325:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Int64 | Float64]`
- ibis/expr/tests/test_schema.py:326:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:326:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Float64 | Boolean | Date]`
- ibis/expr/tests/test_schema.py:327:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:327:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | Float64 | String]`
- ibis/expr/tests/test_schema.py:328:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:328:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | Float64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:330:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:330:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Float64]`
- ibis/expr/tests/test_schema.py:332:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:332:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, String | Int64 | Float64 | Boolean | Date]`
- ibis/expr/tests/test_schema.py:334:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:334:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64]`
- ibis/expr/tests/test_schema.py:335:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:335:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Boolean | Date]`
- ibis/expr/tests/test_schema.py:336:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:336:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | Boolean | Date]`
- ibis/expr/tests/test_schema.py:365:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:365:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:376:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:376:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:393:38: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:393:38: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:419:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:419:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Timestamp]`
- ibis/expr/tests/test_schema.py:581:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:581:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | Int32 | Int16 | ... omitted 13 union elements]`
- ibis/expr/types/generic.py:1553:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown]`
+ ibis/expr/types/generic.py:1553:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Value[Unknown, Any]]`
- ibis/expr/types/generic.py:1754:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown]`
+ ibis/expr/types/generic.py:1754:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Value[Unknown, Any]]`
- ibis/formats/pandas.py:158:55: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/pandas.py:158:55: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Unknown]`
- ibis/formats/polars.py:156:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/polars.py:156:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, DataType]`
- ibis/formats/polars.py:162:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/polars.py:162:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, DataType]`
- ibis/formats/tests/test_pandas.py:120:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:120:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Boolean | Float64]`
- ibis/formats/tests/test_pandas.py:147:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:147:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Int64 | String | Boolean | Float64]`
- ibis/formats/tests/test_pandas.py:424:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:424:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, Unknown]`
- ibis/formats/tests/test_pandas.py:440:25: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:440:25: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/tests/expr/test_table.py:1087:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/tests/expr/test_table.py:1087:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/tests/expr/test_table.py:1150:29: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/tests/expr/test_table.py:1150:29: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`

pandas (https://github.com/pandas-dev/pandas)
+ pandas/core/apply.py:1762:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[((...) -> Unknown) | str]`, found `(((...) -> Unknown) & Top[list[Unknown]]) | list[((...) -> Unknown) | str] | (MutableMapping[Hashable, ((...) -> Unknown) | str | list[((...) -> Unknown) | str]] & Top[list[Unknown]])`
- Found 3789 diagnostics
+ Found 3790 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/mosaic_gpu/lowering.py:1511:10: error[invalid-return-type] Return type does not match returned value: expected `tuple[RefOrTmemType@_handle_transforms, Sequence[Transform]]`, found `tuple[Unknown | MultimemRef, list[Unknown]]`
+ jax/_src/pallas/mosaic_gpu/lowering.py:1511:10: error[invalid-return-type] Return type does not match returned value: expected `tuple[RefOrTmemType@_handle_transforms, Sequence[Transform]]`, found `tuple[RefOrTmemType@_handle_transforms | TMEMRef | Unknown | MultimemRef, list[Unknown]]`

zulip (https://github.com/zulip/zulip)
+ zerver/lib/test_classes.py:3011:5: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(**_P@contextmanager) -> Iterator[MagicMock]`, found `def mock_fcm(self, for_legacy: bool = False) -> Iterator[MagicMock]`
+ zerver/lib/test_classes.py:3030:5: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(**_P@contextmanager) -> Iterator[AsyncMock]`, found `def mock_apns(self, for_legacy: bool = False) -> Iterator[AsyncMock]`
+ zerver/lib/test_helpers.py:764:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(**_P@contextmanager) -> Iterator[MagicMock]`, found `def mock_queue_publish(method_to_patch: str, **kwargs: object) -> Iterator[MagicMock]`
+ zerver/tests/test_e2ee_push_notifications.py:662:27: error[invalid-argument-type] Argument is incorrect: Expected `((self, for_legacy: bool = False)) | MagicMock`, found `Literal[True]`
+ zerver/tests/test_e2ee_push_notifications.py:663:28: error[invalid-argument-type] Argument is incorrect: Expected `((self, for_legacy: bool = False)) | AsyncMock`, found `Literal[True]`
+ zerver/tests/test_email_notifications.py:243:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:258:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:292:32: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:321:32: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:344:32: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:361:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:388:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:406:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:430:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_email_notifications.py:452:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.send_email.queue_event_on_commit"]`
+ zerver/tests/test_event_queue.py:43:13: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.tornado.event_queue.queue_json_publish_rollback_unsafe"]`
+ zerver/tests/test_event_queue.py:49:13: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.tornado.event_queue.queue_json_publish_rollback_unsafe"]`
+ zerver/tests/test_event_queue.py:65:13: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.tornado.event_queue.queue_json_publish_rollback_unsafe"]`
+ zerver/tests/test_external.py:245:5: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(**_P@contextmanager) -> Iterator[Mock]`, found `def tor_mock(self, side_effect: Exception | None = None, read_data: Sequence[str] = ...) -> Iterator[Mock]`
+ zerver/tests/test_external.py:296:28: error[invalid-argument-type] Argument is incorrect: Expected `((self, side_effect: Exception | None = None, read_data: Sequence[str] = ...)) | Mock`, found `list[Unknown | str]`
+ zerver/tests/test_external.py:313:27: error[invalid-argument-type] Argument is incorrect: Expected `((self, side_effect: Exception | None = None, read_data: Sequence[str] = ...)) | Mock`, found `list[Unknown]`
+ zerver/tests/test_external.py:333:27: error[invalid-argument-type] Argument is incorrect: Expected `((self, side_effect: Exception | None = None, read_data: Sequence[str] = ...)) | Mock`, found `FileNotFoundError`
+ zerver/tests/test_link_embed.py:351:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_edit.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:389:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:432:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:479:13: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_edit.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:495:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:611:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:648:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:707:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:739:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:775:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:816:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:854:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:882:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:929:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:966:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:1010:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_link_embed.py:1050:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_send.queue_event_on_commit"]`
+ zerver/tests/test_message_edit_notifications.py:143:13: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.tornado.event_queue.queue_json_publish_rollback_unsafe"]`
+ zerver/tests/test_message_edit_notifications.py:144:13: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `def fake_publish(queue_name: str, event: Mapping[str, Any] | str, *args: Any) -> None`
+ zerver/tests/test_push_notifications.py:2508:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.actions.message_flags.queue_event_on_commit"]`
+ zerver/tests/test_push_registration.py:256:33: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.views.push_notifications.queue_event_on_commit"]`
+ zerver/tests/test_queue_worker.py:552:25: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.queue.queue_json_publish_rollback_unsafe"]`
+ zerver/tests/test_queue_worker.py:553:25: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `def fake_publish(queue_name: str, event: dict[str, Any], processor: (Any, /) -> None) -> None`
+ zerver/tests/test_queue_worker.py:619:21: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `Literal["zerver.lib.queue.queue_json_publish_rollback_unsafe"]`
+ zerver/tests/test_queue_worker.py:619:76: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `def fake_publish(queue_name: str, event: dict[str, Any], processor: ((Any, /) -> None) | None) -> None`
+ zerver/tests/test_service_bot_system.py:453:37: error[invalid-argument-type] Argument is incorrect: Expected `((method_to_patch: str, **kwargs: object)) | MagicMock`, found `str`
- Found 3662 diagnostics
+ Found 3710 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/aggregator.py:392:38: error[invalid-argument-type] Argument to bound method `activate_module` is incorrect: Expected `Literal["makerdao_dsr", "makerdao_vaults", "uniswap", "loopring", "eth2", ... omitted 4 literals]`, found `str`
- rotkehlchen/chain/aggregator.py:1236:17: error[invalid-argument-type] Argument to bound method `check_chains_and_add_accounts` is incorrect: Expected `list[Literal[SupportedBlockchain.ETHEREUM, SupportedBlockchain.OPTIMISM, SupportedBlockchain.AVALANCHE, SupportedBlockchain.POLYGON_POS, SupportedBlockchain.ARBITRUM_ONE, ... omitted 5 literals]]`, found `list[SupportedBlockchain]`
- Found 2103 diagnostics
+ Found 2101 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/batch.py:778:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocCompound[Batch]`, found `InterGetItemILocCompound[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/batch.py:782:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Batch]`, found `InterfaceGetItemBLoc[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Self@iloc` does not satisfy upper bound `generic[Any]` of type variable `TVDtype`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any | TVContainer_co@loc, TVDtype@Index]`
- static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
+ static_frame/core/node_iter.py:866:16: error[invalid-return-type] Return type does not match returned value: expected `IterNodeDelegate[TContainerAny@IterNode]`, found `IterNodeDelegate[TContainerAny@IterNode | Bottom[Series[Any, Any]] | Any | ... omitted 3 union elements]`
+ static_frame/core/node_iter.py:872:16: error[invalid-return-type] Return type does not match returned value: expected `IterNodeDelegateReducible[TContainerAny@IterNode]`, found `IterNodeDelegateReducible[TContainerAny@IterNode | Bottom[Series[Any, Any]] | Any | ... omitted 3 union elements]`
+ static_frame/core/node_iter.py:878:16: error[invalid-return-type] Return type does not match returned value: expected `IterNodeDelegateMapable[TContainerAny@IterNode]`, found `IterNodeDelegateMapable[TContainerAny@IterNode | Bottom[Series[Any, Any]] | Any | ... omitted 3 union elements]`
+ static_frame/core/node_selector.py:456:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Hashable, /) -> TVContainer_co@InterfaceSelectDuo`, found `Unknown | TLocSelectorFunc@__init__`
+ static_frame/core/node_selector.py:492:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Hashable, /) -> TVContainer_co@InterfaceSelectTrio`, found `Unknown | TLocSelectorFunc@__init__`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]] | TVContainer_co@InterfaceSelectQuartet, Any]`
+ static_frame/core/node_selector.py:536:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Hashable, /) -> TVContainer_co@InterfaceSelectQuartet`, found `Unknown | TLocSelectorFunc@__init__`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1838 diagnostics
+ Found 1836 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/advantage_air/__init__.py:69:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/advantage_air/__init__.py:69:4

... (truncated 52 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~3MB
+     struct fields = ~4MB
-     memo fields = ~49MB
+     memo fields = ~52MB

trio (https://github.com/python-trio/trio)
-     struct metadata = ~11MB
+     struct metadata = ~12MB
-     struct fields = ~11MB
+     struct fields = ~12MB
-     memo metadata = ~33MB
+     memo metadata = ~35MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~21MB
+     struct metadata = ~22MB
-     struct fields = ~20MB
+     struct fields = ~21MB
-     memo metadata = ~73MB
+     memo metadata = ~76MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~725MB
-     struct metadata = ~49MB
+     struct metadata = ~54MB
-     struct fields = ~54MB
+     struct fields = ~57MB
-     memo metadata = ~167MB
+     memo metadata = ~185MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2026-01-01 01:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 52.98%**

<sub>Comparing <code>ibraheem/variance-in-argument</code> (fba4ac1) with <code>main</code> (a0f2cd0)</sub>



### Summary

`❌ 9` regressed benchmarks  
`✅ 14` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 87.5 s | 137.1 s | -36.19% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.1 s | 44.9 s | -52.98% |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 50.6 s | 54.3 s | -6.82% |
| ❌ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.6 s | 4.8 s | -5.79% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 62.6 s | 78.3 s | -20.13% |
| ❌ | WallTime | [`` tanjun ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Atanjun&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.5 s | 2.7 s | -5.7% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.8 s | 9.8 s | -20.85% |
| ❌ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.1 ms | 6.6 ms | -7.77% |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1.3 s | -5.89% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fvariance-in-argument?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2026-01-08 22:13_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 22:20_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 16 | 9 | 63 |
| `invalid-return-type` | 3 | 12 | 7 |
| `invalid-assignment` | 0 | 6 | 7 |
| `possibly-missing-attribute` | 11 | 1 | 1 |
| `unresolved-attribute` | 0 | 11 | 0 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| `no-matching-overload` | 0 | 1 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `unsupported-operator` | 0 | 0 | 1 |
| **Total** | **31** | **42** | **79** |


**[Full report with detailed diff](https://56abb121.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://56abb121.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @ibraheemdev on 2026-01-09 22:08_

There seems to be some subtle changes in the ecosystem that I have yet to look into, but I think this is ready for an initial review.

The overall problem that this is trying to solve is: if we infer a type variable `T` in the formal type to some type `X` in the actual type, we have no way of knowing the variance of `X` with respect to the actual type, as there may be multiple occurences of `X`, and we have no way of distinguishing between them, but we need the variance to inform out literal promotion heuristics.

The approach taken in this PR transforms the formal and actual types to contain synthetic type variables and recursing one generic layer at a time, keeping track of the variance of the inferred type along the way. This is very similar to how we perform reverse inference. Alternatively, we might be able to transform the actual type such that each type is uniquely identified, e.g., wrapping each type in a synthetic type alias?.

This seems to have a quite significant performance and memory usage impact (though there are some optimizations that could be made here), but I'm wondering if there's a simpler approach that I am missing.

---

_Marked ready for review by @ibraheemdev on 2026-01-09 22:08_

---

_Review requested from @carljm by @ibraheemdev on 2026-01-09 22:08_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-09 22:08_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-09 22:08_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-09 22:08_

---

_@ibraheemdev reviewed on 2026-01-09 22:12_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/literal_promotion.md`:136 on 2026-01-09 22:12_

This is a regression from main because we don't currently support the synthetic type mapping on `Callable` types, but this shouldn't be too hard to fix.

---
