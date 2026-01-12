```yaml
number: 20528
title: "[ty] bidirectional type inference using function return type annotations"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: bidi-return-type
created_at: 2025-09-23T05:57:09Z
updated_at: 2025-10-11T02:52:05Z
url: https://github.com/astral-sh/ruff/pull/20528
synced_at: 2026-01-12T15:57:04Z
```

# [ty] bidirectional type inference using function return type annotations

---

_@mtshiba_

## Summary

Implements bidirectional type inference using function return type annotations.

This PR was originally proposed to solve astral-sh/ty#1167, but this does not fully resolve it on its own.
Additionally, I believe we need to allow dataclasses to generate their own `__new__` methods, [use constructor return types ​​for inference](https://github.com/mtshiba/ruff/blob/5844c0103d30de4f8890414b38ce1a8d92d6f2d2/crates/ty_python_semantic/src/types.rs#L5326-L5328), and a mechanism to discard type narrowing like `& ~AlwaysFalsy` if necessary (at a more general level than this PR).

## Test Plan

`mdtest/bidirectional.md` is added.

---

_Renamed from "[ty] propagate the annotated return type of functions to the inference of expressions in return statements" to "[ty] bidirectional type inference using function return type annotations" by @mtshiba on 2025-09-23 05:57_

---

_Comment by @github-actions[bot] on 2025-09-23 05:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-23 06:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/installer.py:2185:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `set[str] | None`
+ lib/spack/spack/installer.py:2185:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str]`, found `set[str] | None`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/local.py:511:24: error[invalid-return-type] Return type does not match returned value: expected `T@LocalProxy`, found `~None | Unknown`
+ src/werkzeug/local.py:511:24: error[invalid-return-type] Return type does not match returned value: expected `T@LocalProxy`, found `T@LocalProxy | ~None | Unknown`
+ src/werkzeug/routing/exceptions.py:107:20: error[no-matching-overload] No overload of function `max` matches arguments
- Found 382 diagnostics
+ Found 383 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/python/raises_group.py:33:12: error[invalid-return-type] Return type does not match returned value: expected `RaisesExc[Failed]`, found `RaisesExc[BaseException]`
- Found 490 diagnostics
+ Found 489 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/codecs/g711.py:65:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
+ src/aiortc/codecs/g711.py:65:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[bytes], None]`
- src/aiortc/codecs/g722.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
+ src/aiortc/codecs/g722.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[bytes], None]`
- src/aiortc/codecs/opus.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
+ src/aiortc/codecs/opus.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[bytes], None]`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/mesh_status.py:117:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, list[str]]`, found `tuple[Unknown | None, list[Unknown | str]]`
+ paasta_tools/cli/cmds/mesh_status.py:117:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, list[str]]`, found `tuple[Unknown | None, list[str]]`
- paasta_tools/tron_tools.py:547:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, FieldSelectorConfig]`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/tron_tools.py:547:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, FieldSelectorConfig]`, found `dict[str, FieldSelectorConfig | dict[Unknown | str, Unknown | str]]`
- paasta_tools/utils.py:3416:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `(Unknown & ~None) | dict[str, Any]`
+ paasta_tools/utils.py:3416:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `InstanceConfigDict | (Unknown & ~None) | dict[str, Any]`

pyjwt (https://github.com/jpadilla/pyjwt)
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_signature' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'require' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'strict_aud' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_aud' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_exp' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_iat' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_iss' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_jti' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_nbf' in TypedDict `FullOptions` constructor
+ jwt/api_jwt.py:77:16: error[missing-typed-dict-key] Missing required key 'verify_sub' in TypedDict `FullOptions` constructor
- Found 23 diagnostics
+ Found 33 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/data_io.py:1149:62: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[tuple[int | float | None, int | float | None]] | None`
+ sockeye/data_io.py:1149:62: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[tuple[int | float | None, int | float | None]]`, found `list[tuple[int | float | None, int | float | None]] | None`
- sockeye/model.py:816:56: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[int] | None`
+ sockeye/model.py:816:56: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[int]`, found `list[int] | None`
- test/unit/test_inference.py:177:114: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
+ test/unit/test_inference.py:177:114: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Any]`, found `list[Any] | None`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[str]`
+ pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[bytes | str]`

pylox (https://github.com/sco1/pylox)
+ pylox/builtins/py_builtins.py:175:32: error[invalid-argument-type] Argument to function `mean` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/builtins/py_builtins.py:192:34: error[invalid-argument-type] Argument to function `median` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/builtins/py_builtins.py:216:32: error[invalid-argument-type] Argument to function `mode` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/builtins/py_builtins.py:326:33: error[invalid-argument-type] Argument to function `stdev` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
- Found 22 diagnostics
+ Found 26 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/webserver/lib/transactions.py:131:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, str]`, found `dict[Unknown | str, Unknown | None]`
+ dragonchain/webserver/lib/transactions.py:131:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, str]`, found `dict[str, str | Unknown | None]`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/contrib/pyopenssl.py:397:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[Any]] | None`, found `dict[Unknown | str, Unknown | tuple[tuple[tuple[str, Unknown]]] | list[tuple[str, str]]]`
+ src/urllib3/contrib/pyopenssl.py:397:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[Any]] | None`, found `dict[str, list[Any] | tuple[tuple[tuple[str, Unknown]]] | list[tuple[str, str]]]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/cpp/_symbol.py:1183:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Symbol] | None, str]`, found `tuple[list[Unknown], None]`
+ sphinx/domains/cpp/_symbol.py:1183:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Symbol] | None, str]`, found `tuple[list[Symbol | (Unknown & ~None)], None]`

Expression (https://github.com/cognitedata/Expression)
- expression/core/result.py:198:24: error[invalid-return-type] Return type does not match returned value: expected `dict[str, _TSourceOut@Result | _TErrorOut@Result | Literal["ok", "error"]]`, found `dict[Unknown | str, Unknown | str | _TSourceOut@Result]`
- expression/core/result.py:203:24: error[invalid-return-type] Return type does not match returned value: expected `dict[str, _TSourceOut@Result | _TErrorOut@Result | Literal["ok", "error"]]`, found `dict[Unknown | str, Unknown | str | _TErrorOut@Result]`
- expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some[_T1](value: _T1@Some) -> Option[_T1@Some], (Any, /) -> Option[Any], /) -> def Some[_T1](value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any], (Any, /) -> Result[Any, Any], /) -> def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
- Found 211 diagnostics
+ Found 207 diagnostics

antidote (https://github.com/Finistere/antidote)
+ tests/lib/interface/test_conditions.py:193:59: error[invalid-argument-type] Argument to bound method `when` is incorrect: Expected `Predicate[Weighted] | Predicate[NeutralWeight] | Weighted | ... omitted 3 union elements`, found `Weight | None`
- Found 300 diagnostics
+ Found 301 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/optimize_reports/optimize_reports.py:492:9: error[invalid-argument-type] Argument to function `generate_tag_metrics` is incorrect: Expected `Literal["enter_tag", "exit_reason"] | list[Literal["enter_tag", "exit_reason"]]`, found `list[Unknown | str]`
- Found 402 diagnostics
+ Found 401 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/commands/menu.py:181:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- tanjun/commands/menu.py:308:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 115 diagnostics
+ Found 113 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/scipy_.py:326:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | PathLike[Any] | ReadBuffer[Unknown] | ... omitted 3 union elements`
+ xarray/backends/scipy_.py:326:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | ReadBuffer[Unknown] | AbstractDataStore | ... omitted 3 union elements`
- xarray/conventions.py:415:17: error[invalid-argument-type] Argument to function `decode_cf_variable` is incorrect: Expected `bool`, found `bool | Mapping[str, bool] | Unknown`
- xarray/conventions.py:416:17: error[invalid-argument-type] Argument to function `decode_cf_variable` is incorrect: Expected `bool`, found `bool | Mapping[str, bool] | Unknown`
- xarray/conventions.py:420:17: error[invalid-argument-type] Argument to function `decode_cf_variable` is incorrect: Expected `bool`, found `bool | (Mapping[str, bool] & ~AlwaysTruthy) | (Unknown & ~AlwaysTruthy)`
- xarray/conventions.py:421:17: error[invalid-argument-type] Argument to function `decode_cf_variable` is incorrect: Expected `bool | None`, found `bool | Mapping[str, bool] | None | Unknown`
- xarray/conventions.py:422:17: error[invalid-argument-type] Argument to function `decode_cf_variable` is incorrect: Expected `bool | CFTimedeltaCoder | None`, found `bool | CFTimedeltaCoder | Mapping[str, bool | CFTimedeltaCoder] | None | Unknown`
+ xarray/conventions.py:406:30: error[invalid-argument-type] Argument to function `_item_or_default` is incorrect: Expected `Mapping[Any, Literal[True] | Unknown] | Literal[True]`, found `bool | Mapping[str, bool]`
+ xarray/conventions.py:415:52: error[invalid-argument-type] Argument to function `_item_or_default` is incorrect: Expected `Mapping[Any, Literal[True] | Unknown] | Literal[True]`, found `bool | Mapping[str, bool]`
+ xarray/conventions.py:416:49: error[invalid-argument-type] Argument to function `_item_or_default` is incorrect: Expected `Mapping[Any, Literal[True] | Unknown] | Literal[True]`, found `bool | Mapping[str, bool]`
+ xarray/conventions.py:418:62: error[invalid-argument-type] Argument to function `_item_or_default` is incorrect: Expected `Mapping[Any, Literal[True] | Unknown] | Literal[True]`, found `bool | CFDatetimeCoder | Mapping[str, bool | CFDatetimeCoder]`
+ xarray/conventions.py:421:45: error[invalid-argument-type] Argument to function `_item_or_default` is incorrect: Expected `Mapping[Any, None | Unknown] | None`, found `bool | Mapping[str, bool] | None`
+ xarray/conventions.py:422:51: error[invalid-argument-type] Argument to function `_item_or_default` is incorrect: Expected `Mapping[Any, None | Unknown] | None`, found `bool | CFTimedeltaCoder | Mapping[str, bool | CFTimedeltaCoder] | None`
- Found 1611 diagnostics
+ Found 1612 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/scripts/symbolextractor.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[str], str]`, found `tuple[list[Unknown | (str & ~AlwaysFalsy)], None]`
+ mesonbuild/scripts/symbolextractor.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[str], str]`, found `tuple[list[str], None]`
- mesonbuild/utils/universal.py:1692:12: error[invalid-return-type] Return type does not match returned value: expected `list[str]`, found `list[Any | Sequence[Any]]`
- Found 886 diagnostics
+ Found 885 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[no-matching-overload] No overload of bound method `pipe` matches arguments
- tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[SchemaOut] | DataFrame[Schema]`
- Found 1592 diagnostics
+ Found 1593 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_testing_raisesgroup.py:28:12: error[invalid-return-type] Return type does not match returned value: expected `RaisesExc[AssertionError]`, found `RaisesExc[BaseException]`
- Found 649 diagnostics
+ Found 648 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/federation/object_type.py:90:9: error[invalid-argument-type] Argument to function `type` is incorrect: Expected `T@_impl_type`, found `T@_impl_type | None`
- Found 381 diagnostics
+ Found 382 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/wrapper/_implementations.py:945:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo] | ListConfig | DictConfig`, found `@Todo | (((...) -> Any) & Top[dict[Unknown, Unknown]]) | (DataClass_ & Top[dict[Unknown, Unknown]]) | ... omitted 8 union elements`
+ src/hydra_zen/wrapper/_implementations.py:945:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo] | ListConfig | DictConfig`, found `@Todo | DataClass_ | type[@Todo] | ... omitted 6 union elements`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/upstream/models.py:587:28: error[invalid-return-type] Return type does not match returned value: expected `list[Image]`, found `list[Unknown | (Edition & ~AlwaysTruthy & ~AlwaysFalsy)]`
+ openlibrary/plugins/upstream/models.py:587:28: error[invalid-return-type] Return type does not match returned value: expected `list[Image]`, found `list[Image | (Edition & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]`
- openlibrary/plugins/worksearch/code.py:436:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[str] | None`
+ openlibrary/plugins/worksearch/code.py:436:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `list[str] | None`
- openlibrary/plugins/worksearch/code.py:436:61: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[str] | None`
+ openlibrary/plugins/worksearch/code.py:436:61: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `list[str] | None`

altair (https://github.com/vega/altair)
+ tests/vegalite/v6/test_theme.py:453:17: error[invalid-key] Invalid key access on TypedDict `StyleConfigIndexKwds`: Unknown key "group-title"
+ tests/vegalite/v6/test_theme.py:454:17: error[invalid-key] Invalid key access on TypedDict `StyleConfigIndexKwds`: Unknown key "guide-label"
+ tests/vegalite/v6/test_theme.py:455:17: error[invalid-key] Invalid key access on TypedDict `StyleConfigIndexKwds`: Unknown key "guide-title"
- Found 1336 diagnostics
+ Found 1339 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/server/models/block_documents.py:462:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3249 diagnostics
+ Found 3250 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/arithmetic/timedeltaindex/test_mul.py:21:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `Index[Any]`
+ tests/indexes/arithmetic/timedeltaindex/test_mul.py:21:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `TimedeltaIndex | Index[Any]`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/property/alias.py:89:16: error[invalid-return-type] Return type does not match returned value: expected `list[PropertyDescriptor[T@Alias]]`, found `list[Unknown | AliasPropertyDescriptor[Unknown]]`
+ src/bokeh/core/property/alias.py:89:16: error[invalid-return-type] Return type does not match returned value: expected `list[PropertyDescriptor[T@Alias]]`, found `list[PropertyDescriptor[T@Alias] | AliasPropertyDescriptor[Unknown]]`
- src/bokeh/core/property/alias.py:103:16: error[invalid-return-type] Return type does not match returned value: expected `list[PropertyDescriptor[T@DeprecatedAlias]]`, found `list[Unknown | DeprecatedAliasPropertyDescriptor[Unknown]]`
+ src/bokeh/core/property/alias.py:103:16: error[invalid-return-type] Return type does not match returned value: expected `list[PropertyDescriptor[T@DeprecatedAlias]]`, found `list[PropertyDescriptor[T@DeprecatedAlias] | DeprecatedAliasPropertyDescriptor[Unknown]]`
- src/bokeh/layouts.py:117:26: error[invalid-argument-type] Argument to function `_handle_child_sizing` is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
- src/bokeh/layouts.py:118:16: error[invalid-argument-type] Argument is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
- src/bokeh/layouts.py:152:26: error[invalid-argument-type] Argument to function `_handle_child_sizing` is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
- src/bokeh/layouts.py:153:19: error[invalid-argument-type] Argument is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
- Found 469 diagnostics
+ Found 465 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/botocore/services/bedrock.py:303:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[str]]`, found `dict[Unknown | str, Unknown | list[Unknown] | list[Unknown | str | None]]`
+ ddtrace/contrib/internal/botocore/services/bedrock.py:303:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[str]]`, found `dict[str, list[str] | (@Todo & Top[list[Unknown]]) | list[Unknown] | list[Unknown | str | None]]`
- ddtrace/internal/coverage/instrumentation_py3_12.py:119:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `tuple[str, ...] | None`
+ ddtrace/internal/coverage/instrumentation_py3_12.py:119:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str]`, found `tuple[str, ...] | None`
- ddtrace/internal/coverage/instrumentation_py3_12.py:132:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `tuple[str, ...] | None`
+ ddtrace/internal/coverage/instrumentation_py3_12.py:132:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str]`, found `tuple[str, ...] | None`
- scripts/freshvenvs.py:230:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, list[str]]`, found `tuple[None, list[Unknown]]`
+ scripts/freshvenvs.py:230:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, list[str]]`, found `tuple[None, list[str]]`
+ tests/contrib/langgraph/conftest.py:344:16: error[missing-typed-dict-key] Missing required key 'which' in TypedDict `State` constructor
- Found 7576 diagnostics
+ Found 7577 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/examples/gen_registry.py:329:27: warning[possibly-missing-attribute] Attribute `get` on type `dict[str, str] | None` may be missing
- ibis/expr/types/relations.py:4868:37: error[invalid-argument-type] Argument to function `_to_selector` is incorrect: Expected `Sequence[str | Selector | Column] | Selector | Column`, found `list[Iterable[str] | Selector]`
- ibis/util.py:109:16: error[invalid-return-type] Return type does not match returned value: expected `list[V@promote_list]`, found `list[Unknown | (V@promote_list & Top[dict[Unknown, Unknown]]) | (Iterable[V@promote_list] & Top[dict[Unknown, Unknown]])]`
+ ibis/util.py:109:16: error[invalid-return-type] Return type does not match returned value: expected `list[V@promote_list]`, found `list[V@promote_list | (Iterable[V@promote_list] & Top[dict[Unknown, Unknown]])]`
- ibis/util.py:115:16: error[invalid-return-type] Return type does not match returned value: expected `list[V@promote_list]`, found `list[Unknown | (V@promote_list & ~Top[list[Unknown]] & ~Top[dict[Unknown, Unknown]] & ~None) | (Iterable[V@promote_list] & ~Top[list[Unknown]] & ~Top[dict[Unknown, Unknown]])]`
+ ibis/util.py:115:16: error[invalid-return-type] Return type does not match returned value: expected `list[V@promote_list]`, found `list[V@promote_list | (Iterable[V@promote_list] & ~Top[list[Unknown]] & ~Top[dict[Unknown, Unknown]])]`

jax (https://github.com/google/jax)
- jax/_src/pallas/fuser/block_spec.py:933:7: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:933:7: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/pallas/fuser/block_spec.py:963:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:963:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/pallas/fuser/block_spec.py:1021:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:1021:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/pallas/fuser/block_spec.py:1080:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:1080:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/pallas/fuser/block_spec.py:1177:7: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:1177:7: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/pallas/fuser/block_spec.py:1336:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:1336:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/pallas/fuser/block_spec.py:1578:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[@Todo | int | None] | None`
+ jax/_src/pallas/fuser/block_spec.py:1578:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo | int | None]`, found `Sequence[@Todo | int | None] | None`
- jax/_src/tree_util.py:440:29: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(T@tree_reduce & ~Unspecified, Unknown, /) -> T@tree_reduce & ~Unspecified`, found `(T@tree_reduce, Any, /) -> T@tree_reduce`
- Found 2443 diagnostics
+ Found 2442 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/io/common.py:1167:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | BaseBuffer, bool, list[BaseBuffer]]`, found `tuple[_IOWrapper, Literal[True], list[Unknown | _IOWrapper]]`
+ pandas/io/common.py:1167:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | BaseBuffer, bool, list[BaseBuffer]]`, found `tuple[_IOWrapper, Literal[True], list[BaseBuffer | _IOWrapper]]`
- pandas/tests/frame/methods/test_replace.py:24:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[int | str]]`, found `dict[Unknown | str, Unknown | list[int] | list[Unknown]]`
+ pandas/tests/frame/methods/test_replace.py:24:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[int | str]]`, found `dict[str, list[int | str] | list[int] | list[Unknown]]`
- pandas/tests/frame/methods/test_replace.py:29:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[int | float | str]]`, found `dict[Unknown | str, Unknown | list[int] | list[Unknown] | list[Unknown | str | int | float]]`
+ pandas/tests/frame/methods/test_replace.py:29:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[int | float | str]]`, found `dict[str, list[int | float | str] | list[int] | list[Unknown]]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/assets/asset.py:172:16: error[invalid-return-type] Return type does not match returned value: expected `AssetWithSymbol`, found `AssetWithNameAndType`
+ rotkehlchen/assets/asset.py:184:16: error[invalid-return-type] Return type does not match returned value: expected `EvmToken`, found `CryptoAsset`
+ rotkehlchen/assets/asset.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `SolanaToken`, found `CryptoAsset`
+ rotkehlchen/assets/asset.py:196:16: error[invalid-return-type] Return type does not match returned value: expected `AssetWithOracles`, found `AssetWithNameAndType`
- rotkehlchen/chain/evm/decoding/beefy_finance/decoder.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[Unknown, Literal["beefy_finance"]]`
- rotkehlchen/chain/evm/decoding/compound/v3/decoder.py:407:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[@Todo, Literal["compound-v3"]]`
- rotkehlchen/chain/evm/decoding/llamazip/decoder.py:81:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[Unknown, Literal["llamazip"]]`
- rotkehlchen/chain/evm/decoding/morpho/decoder.py:335:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[Unknown, Literal["morpho"]]`
- rotkehlchen/chain/evm/decoding/uniswap/v3/decoder.py:419:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[Unknown, Literal["uniswap-v3-router"]]`
- rotkehlchen/chain/evm/decoding/zerox/decoder.py:294:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[Unknown, Literal["0x"]]`
- rotkehlchen/tests/db/test_db.py:454:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `list[EvmToken] | None`
+ rotkehlchen/tests/db/test_db.py:454:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[EvmToken]`, found `list[EvmToken] | None`
- rotkehlchen/tests/unit/globaldb/test_asset_updates.py:405:9: error[invalid-argument-type] Argument to bound method `_apply_single_version_update` is incorrect: Expected `dict[Asset, Literal["remote", "local"]] | None`, found `dict[Unknown | Asset, Unknown | str]`
- Found 1702 diagnostics
+ Found 1699 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~40MB
+     memo metadata = ~42MB

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-09-23 07:44_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/generics.rs`:1094 on 2025-09-24 07:00_

I decided that this case requires elaborate special handling, that falling back to the case below would lead to false positive errors, and that we should do nothing at this time.

In the case below, specialization is performed only when there is only one type variable in `formal`, but even so, the following code will not be handled correctly.


```python
def h[T](x: T, cond: bool) -> T | list[T]:
    # error: [invalid-return-type] "Return type does not match returned value: expected `T@h | list[T@h]`, found `T@h | list[T@h] | list[T@h | list[T@h]]`"
    return i(x, cond)

def i[T](x: T, cond: bool) -> T | list[T]:
    return x if cond else [x]
```

---

_@mtshiba reviewed on 2025-09-24 07:28_

---

_Marked ready for review by @mtshiba on 2025-09-24 07:28_

---

_Review requested from @carljm by @mtshiba on 2025-09-24 07:28_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-09-24 07:28_

---

_Review requested from @sharkdp by @mtshiba on 2025-09-24 07:28_

---

_Review requested from @dcreager by @mtshiba on 2025-09-24 07:28_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-24 07:41_

---

_Comment by @github-actions[bot] on 2025-09-24 07:46_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 12 | 15 | 16 |
| `invalid-return-type` | 4 | 11 | 26 |
| `missing-typed-dict-key` | 11 | 0 | 0 |
| `invalid-key` | 3 | 0 | 0 |
| `no-matching-overload` | 3 | 0 | 0 |
| `possibly-missing-attribute` | 1 | 0 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **35** | **26** | **42** |


---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-24 08:55_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-24 08:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:48 on 2025-09-24 15:31_

I think that's maybe not an awful type to infer here, actually? `(list[int] & ~AlwaysFalsy) | list[Unknown]` is assignable to `list[int]` (and the upper bound of `list[int]` is the only constraint here), so it seems reasonable to infer the more precise type here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:60 on 2025-09-24 15:33_

I suggest giving this snippet an explicit filename, or it will be implicitly merged with the previous snippet by the mdtest framework, which might produce some surprising results:


````suggestion

`typed_dict.py`:

```py
````

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:97 on 2025-09-24 15:38_

I don't think this quite accurately describes what's going on here. It's not that we infer one type, and then that type is later widened to a second type by bidirectional type inference. Instead, we use bidirectional type inference to ensure that we never infer the narrower type at all -- the outer context is used to ensure that we infer a broader type when solving the type variable in the second `list1()` call

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:122 on 2025-09-24 15:42_

```suggestion
    # `list[int] | list[str]` is disjoint from `list[int | str]`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4778 on 2025-09-24 15:43_

```suggestion
                // i.e. type variables in the return type are non-inferable,
                // and the return types of async functions are not wrapped in `CoroutineType[...]`.
```

---

_@AlexWaygood reviewed on 2025-09-24 15:48_

This looks pretty good to me! Some small comments below. @dcreager and/or @ibraheemdev will probably be better reviewers, though; I haven't done much work on our bidirectional inference yet

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:455 on 2025-09-25 00:50_

These flags are becoming fairly complex, which I consider a code smell. Could they instead become methods on `Signature`, that the caller can decide to call if they need to? There would still be a new `raw_signature`, but that's what `Signature::from_function` would return, and then `OverloadLiteral::signature` would call new `mark_typevars_inferable` and `wrap_coroutine_return_type` methods on the result before returning.

---

_@dcreager reviewed on 2025-09-25 00:51_

---

_Converted to draft by @mtshiba on 2025-09-27 04:18_

---

_@mtshiba reviewed on 2025-09-29 17:13_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:48 on 2025-09-29 17:13_

Yes, this typing is acceptable in this case.
The reason I marked this as TODO is because I think bidirectional inference could be used for inferring boolean operations. Here the expected type of `l2` is `list[int]`, so the type of `list()` should be inferred to be `list[int]`.

---

_@AlexWaygood reviewed on 2025-09-29 17:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:48 on 2025-09-29 17:25_

But I think we'd only use bidirectional inference to ensure that the inferred type is _assignable_ to the declared type. Here the "natural" inferred type (`(list[int] & ~AlwaysFalsy) | list[Unknown]`) [is already assignable](https://play.ty.dev/71778f42-1772-4e62-b233-2d9e9737cdd7) to the declared type (`list[int]`), so I don't think there's any reason for bidirectional inference to alter our inferred type. It's the same principle by which we infer `Literal[3]` here, even though the declared type is `int` -- `Literal[3]` is more precise than `int`, and the type `Literal[3]` is assignable to `int`, so it's acceptable here for our inferred type to be narrower than the declared type:

```py
x: int = 3
reveal_type(x)  # revealed: Literal[3]
```

---

_Comment by @mtshiba on 2025-09-29 17:38_

~~local mdtest is now failing as shown below, and I don't know why (CI reports no issues).~~
~~I think it might be related to #20533, do you know anything about it ? @dcreager~~

```
running 1 test

constraints.md - Constraints - Intersection - Intersection of a ra… (27656aaba27b10e9)

  crates\ty_python_semantic\resources\mdtest\type_properties\constraints.md:309 unmatched assertion: revealed: ty_extensions.ConstraintSet[(¬(Sub ≤ T@_ ≤ Base) ∧ (SubSub ≤ T@_ ≤ Base))]
  crates\ty_python_semantic\resources\mdtest\type_properties\constraints.md:309 unexpected error: 17 [revealed-type] "Revealed type: `ty_extensions.ConstraintSet[((SubSub ≤ T@_ ≤ Base) ∧ ¬(Sub ≤ T@_ ≤ Base))]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='constraints.md - Constraints - Intersection - Intersection of a ra… (27656aaba27b10e9)'
MDTEST_TEST_FILTER='constraints.md - Constraints - Intersection - Intersection of a ra… (27656aaba27b10e9)' cargo test -p ty_python_semantic --test mdtest -- mdtest__type_properties_constraints

constraints.md - Constraints - Union - Union of two ranges (3b2c9218d1b1205)

  crates\ty_python_semantic\resources\mdtest\type_properties\constraints.md:421 unmatched assertion: revealed: ty_extensions.ConstraintSet[(Base ≤ T@_ ≤ Super) ∨ (Sub ≤ T@_ ≤ Base)]
  crates\ty_python_semantic\resources\mdtest\type_properties\constraints.md:421 unexpected error: 17 [revealed-type] "Revealed type: `ty_extensions.ConstraintSet[(Sub ≤ T@_ ≤ Base) ∨ (Base ≤ T@_ ≤ Super)]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='constraints.md - Constraints - Union - Union of two ranges (3b2c9218d1b1205)'
MDTEST_TEST_FILTER='constraints.md - Constraints - Union - Union of two ranges (3b2c9218d1b1205)' cargo test -p ty_python_semantic --test mdtest -- mdtest__type_properties_constraints

--------------------------------------------------

test mdtest__type_properties_constraints ... FAILED
```

Seems to have been fixed in #20653.

---

_Marked ready for review by @mtshiba on 2025-09-29 17:39_

---

_Review requested from @dcreager by @mtshiba on 2025-09-29 17:39_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-09-29 17:39_

---

_@mtshiba reviewed on 2025-09-29 18:57_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:48 on 2025-09-29 18:57_

Hmm, you're right. This typing indeed seems consistent with the current behavior of ty.

However, it seems counterintuitive that even though we typed `l2` as the fully-static type `list[int]` here, a gradual type is mixed into the inferred type.

This is a related issue to astral-sh/ty#136. For now, I'll mark this comment as just a note, not a TODO.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-29 19:07_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-29 19:07_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:370 on 2025-10-02 00:46_

I _think_ this might be redundant with work that is happening in `raw_signature` and `Signature::from_function`. If you remove this snippet does everything still work? (Or is there something being modified in the generic context that I'm missing? If so, this deserves a comment describing that.)

---

_@dcreager approved on 2025-10-02 00:46_

One last comment, otherwise I think this is looking good!

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1058 on 2025-10-03 23:58_

Can you add a test [to this snippet](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/assignment/annotations.md#collection-literal-annotations-are-understood) that `l: list[int | str] | None = [1]` is inferred as `list[int | str]` with this change?

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:4767 on 2025-10-04 00:01_

I find the way this is written slightly confusing.
```suggestion
        let tcx = if ret.value.is_some() {
            nearest_enclosing_function(self.db(), self.index, self.scope()).map(|func| {
                // When inferring expressions within a function body,
                // the expected type passed should be the "raw" type,
                // i.e. type variables in the return type are non-inferable,
                // and the return types of async functions are not wrapped in `CoroutineType[...]`.
                TypeContext::new(func.last_definition_raw_signature(self.db()).return_ty)
            })
        } else {
            TypeContext::default()
        };
```

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:40 on 2025-10-04 00:22_

I'd prefer to avoid the overlap with literal promotion here.

```suggestion
l4: list[int | str] | None = list1(1)
reveal_type(l4)  # revealed: list[int | str]
```

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:133 on 2025-10-04 00:22_

Similarly here.
```suggestion
async def g() -> list[int | str]:
    return list1(1)
```

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:34 on 2025-10-04 00:28_

Hmm.. it seems we need some form of post-specialization literal promotion here.

---

_@ibraheemdev approved on 2025-10-04 00:29_

---

_@mtshiba reviewed on 2025-10-06 17:09_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/function.rs`:370 on 2025-10-06 17:09_

We need to update `signature.generic_context` here, because type variables in `GenericContext::variables` are still non-inferable.

~~But I realized that this handling can be omitted at the `Signature::from_function` stage, so I did it (9507d7274b82567cd000bbfaefb4412251165ffb).~~
Hmm, this change seems to contrary make performance worse.

---

_Comment by @codspeed-hq[bot] on 2025-10-06 17:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Abidi-return-type)

### Merging #20528 will **not alter performance**

<sub>Comparing <code>mtshiba:bidi-return-type</code> (e7b5c14) with <code>main</code> (11a9e7e)</sub>



### Summary

`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Abidi-return-type?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Review requested from @dcreager by @mtshiba on 2025-10-06 17:38_

---

_Review requested from @ibraheemdev by @mtshiba on 2025-10-06 17:38_

---

_@ibraheemdev reviewed on 2025-10-06 19:30_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:329 on 2025-10-06 19:30_

Is this a regression?

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:329 on 2025-10-07 16:14_

This is revealed as `int | None` in main. The change is due to the addition of a [branch](https://github.com/mtshiba/ruff/blob/8834fec17728a1531821309ed2fa89c311fce242/crates/ty_python_semantic/src/types/generics.rs#L1167-L1168) that does nothing when matching in `SpecializationBuilder::infer`.
As [already mentioned](https://github.com/astral-sh/ruff/pull/20528/files#r2374660055), specialization between unions requires special handling, and falling back to the  `(Type::Union(_), _) => ...` branch could result in incorrect specialization. The default mapping  `T = Unknown` is suboptimal but not incorrect, and the mapping `T = int | None` is also suboptimal, so I'm not sure whether this should be considered a regression.

Specialization between unions will be a future work.

---

_@mtshiba reviewed on 2025-10-07 16:14_

---

_@dcreager reviewed on 2025-10-07 16:59_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:329 on 2025-10-07 16:59_

This is timely — I just added that same branch with an initial "better than nothing" heuristic in https://github.com/astral-sh/ruff/pull/20749.

---

_@dcreager reviewed on 2025-10-07 17:00_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:329 on 2025-10-07 17:00_

(Note that the heuristic in 20749 will not handle @mtshiba's example, since it only engages when `formal` has precisely one top-level typevar, and `actual` has none.)

---

_@ibraheemdev reviewed on 2025-10-07 18:13_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:329 on 2025-10-07 18:13_

I think that's fine, this should be eventually fixed by @dcreager's constraint solver work, so I wouldn't worry too much about it.

---

_@mtshiba reviewed on 2025-10-07 18:14_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:329 on 2025-10-07 18:14_

OK, I merged the latest changes and made a minor fix to prevent incorrect specialization.

https://github.com/mtshiba/ruff/blob/1b42e0576a4493b1c5576624e8764b2708de410c/crates/ty_python_semantic/src/types/generics.rs#L1191-L1201

---

_Comment by @mtshiba on 2025-10-07 19:25_

I think I addressed all review comments.

codspeed reports a slight performance regression, but for the same reason as https://github.com/astral-sh/ruff/pull/20635, I believe it is difficult to avoid this.

The ecosystem reports are generally good, some false positive errors are due to TODOs in `TypedDict` and the unification solver.

---

_@ibraheemdev approved on 2025-10-07 20:10_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-10 10:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-10 10:06_

---

_Comment by @carljm on 2025-10-10 23:53_

I think this might need minor updating now that I merged #20806 (since it should fix a test TODO added in that PR)? @ibraheemdev feel free to merge this if it looks ready to you.

---

_Comment by @ibraheemdev on 2025-10-11 00:34_

This looks great, thanks.

---

_Merged by @ibraheemdev on 2025-10-11 00:38_

---

_Closed by @ibraheemdev on 2025-10-11 00:38_

---

_Branch deleted on 2025-10-11 02:52_

---
