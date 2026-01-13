```yaml
number: 22503
title: "[ty] Fix `@Todo` type for starred expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/starred-tuples-2
created_at: 2026-01-11T16:35:18Z
updated_at: 2026-01-13T21:09:31Z
url: https://github.com/astral-sh/ruff/pull/22503
synced_at: 2026-01-13T21:36:29Z
```

# [ty] Fix `@Todo` type for starred expressions

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2071.

`(1, *(2, 3), 4)` should be inferred as `tuple[Literal[1], Literal[2], Literal[3], Literal[4]]`, but we currently infer it as `tuple[Literal[1], @Todo(StarredExpression), Literal[4]]`, which causes a variety of false positives on user code because we infer a tuple of the wrong length altogether.

This PR still leaves open a couple of TODOs, but they are much more narrowly scoped and low-impact:
- Starred expressions in annotations are still TODOs (PEP 646):
  ```py
  # generic over a typevartuple
  def f[*Ts](*args: *Ts): ...

  # not generic, but also TODO:
  def g(*args: *tuple[int, *tuple[str, ...]]): ...
  ```
- Starred expressions in class bases are still TODO:
  ```py
  bases = (int, object)
  class Foo(*bases): ...  # still not inferred correctly
  ```

## Test Plan

- Mdtests added and extended
- Lots of false positives going away in the ecosystem results
- Some new diagnostics being added in the ecosystem, but these mostly look like true positives to me


---

_Label `ty` added by @AlexWaygood on 2026-01-11 16:35_

---

_Comment by @astral-sh-bot[bot] on 2026-01-11 16:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-11 16:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_sockets.py:385:56: error[invalid-argument-type] Argument to function `setup_raw_socket` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `tuple[str | Unknown, int, @Todo(StarredExpression)]`
- Found 93 diagnostics
+ Found 92 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/_internal/robot/utils.py:232:25: warning[redundant-cast] Value is already of type `list[object] | None`
- Found 172 diagnostics
+ Found 173 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/link_tree.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo(StarredExpression)]`
- lib/spack/spack/llnl/util/link_tree.py:151:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo(StarredExpression)]`
- lib/spack/spack/spec.py:5857:69: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 4318 diagnostics
+ Found 4315 diagnostics

black (https://github.com/psf/black)
- src/black/cache.py:144:59: error[invalid-assignment] Object of type `dict[str | Unknown, tuple[@Todo(StarredExpression)] | Unknown]` is not assignable to `dict[str, tuple[int | float, int, str]]`
- Found 54 diagnostics
+ Found 53 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/reports.py:302:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, int, str]`, found `tuple[@Todo(StarredExpression), str]`
- src/_pytest/terminal.py:1583:23: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, str, int | None, str]`, found `tuple[int, @Todo(StarredExpression)]`
- Found 423 diagnostics
+ Found 421 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_downloaderslotssettings.py:44:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | (str & ~AlwaysFalsy) | (dict[Unknown | str, Unknown | int] & ~AlwaysFalsy)` and value of type `list[int | float]` on object of type `dict[str, list[int | float]]`
- Found 1785 diagnostics
+ Found 1786 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:4426:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Path | None | str | @Todo(StarredExpression)]`
+ mkosi/__init__.py:4426:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Path | None | str]`

poetry (https://github.com/python-poetry/poetry)
- src/poetry/utils/env/mock_env.py:34:28: error[invalid-assignment] Object of type `tuple[@Todo(StarredExpression), Literal["final"], Literal[0]]` is not assignable to `tuple[int, int, int] | tuple[int, int, int, str, int]`
+ src/poetry/utils/env/mock_env.py:34:28: error[invalid-assignment] Object of type `tuple[*tuple[int | str, ...], Literal["final"], Literal[0]]` is not assignable to `tuple[int, int, int] | tuple[int, int, int, str, int]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

vision (https://github.com/pytorch/vision)
+ test/test_datasets.py:698:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["trainval"]` and value of type `tuple[int, ...]` on object of type `dict[str, tuple[int, int, int] | tuple[int, int] | tuple[int]]`
- torchvision/datasets/utils.py:268:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None, str | None]`, found `tuple[@Todo, @Todo(StarredExpression)]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/commands/data_commands.py:172:21: error[invalid-assignment] Not enough values to unpack: Expected 6
- freqtrade/commands/data_commands.py:226:21: error[invalid-assignment] Not enough values to unpack: Expected 4
- Found 654 diagnostics
+ Found 652 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/ext/apidoc/_generate.py:43:21: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["__init__"]` and `Sized | @Todo(StarredExpression)`
+ sphinx/ext/apidoc/_generate.py:43:21: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["__init__"]` and `Sized`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/transformers.py:584:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- discord/ext/commands/converter.py:1241:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 548 diagnostics
+ Found 546 diagnostics

pyodide (https://github.com/pyodide/pyodide)
+ pyodide-build/pyodide_build/recipe/graph_builder.py:167:13: error[no-matching-overload] No overload of function `run` matches arguments
+ pyodide-build/pyodide_build/xbuildenv.py:307:18: error[no-matching-overload] No overload of function `run` matches arguments
- Found 935 diagnostics
+ Found 937 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/i18n/__init__.py:209:13: error[invalid-assignment] Not enough values to unpack: Expected 6
- openlibrary/i18n/__init__.py:209:13: error[invalid-assignment] Not enough values to unpack: Expected 6
- openlibrary/plugins/worksearch/code.py:309:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, str]]`
+ openlibrary/plugins/worksearch/code.py:309:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, str] | tuple[str, Unknown | int]]`
- openlibrary/utils/isbn.py:112:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | None, str | None, str | None]`, found `tuple[str | None, @Todo(StarredExpression)]`
- Found 1149 diagnostics
+ Found 1146 diagnostics

apprise (https://github.com/caronc/apprise)
- tests/test_plugin_email.py:634:5: error[invalid-assignment] Object of type `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` is not assignable to attribute `EMAIL_TEMPLATES` of type `tuple[tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]`
+ tests/test_plugin_email.py:634:5: error[invalid-assignment] Object of type `tuple[tuple[Literal["Testing Lookup"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]` is not assignable to attribute `EMAIL_TEMPLATES` of type `tuple[tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:83:17: error[invalid-assignment] Object of type `list[@Todo(StarredExpression) | <NewType pseudo-class 'FederationAny'>]` is not assignable to `Iterable[type]`
+ strawberry/federation/schema.py:83:17: error[invalid-assignment] Object of type `list[type | <NewType pseudo-class 'FederationAny'>]` is not assignable to `Iterable[type]`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/coverage/instrumentation_py3_11.py:338:46: error[invalid-argument-type] Argument to bound method `from_bytes` is incorrect: Expected `Iterable[SupportsIndex] | SupportsBytes | Buffer`, found `list[bytes | Unknown]`
+ ddtrace/internal/coverage/instrumentation_py3_12.py:216:46: error[invalid-argument-type] Argument to bound method `from_bytes` is incorrect: Expected `Iterable[SupportsIndex] | SupportsBytes | Buffer`, found `list[bytes | Unknown]`
- Found 8396 diagnostics
+ Found 8398 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/integrate/test_ode.pyi:30:1: error[type-assertion-failure] Type `int | float` does not match asserted type `Unknown`
+ tests/integrate/test_ode.pyi:30:1: error[type-assertion-failure] Type `int | float` does not match asserted type `@Todo`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/prefect/blocks/core.py:86:33: error[invalid-assignment] Object of type `tuple[<class 'list'>, <class 'dict'>, <class 'tuple'>, *tuple[<special-form 'typing.Union'> | <class 'UnionType'>, ...]]` is not assignable to `tuple[type, ...]`
- Found 5370 diagnostics
+ Found 5371 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/ad.py:60:25: error[invalid-argument-type] Argument to function `annotate` is incorrect: Expected `tuple[AbstractValue] | None`, found `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]`
+ jax/_src/interpreters/ad.py:60:25: error[invalid-argument-type] Argument to function `annotate` is incorrect: Expected `tuple[AbstractValue] | None`, found `tuple[AbstractValue | Unknown, ...]`
- jax/_src/interpreters/partial_eval.py:800:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/lax/control_flow/conditionals.py:231:7: error[invalid-assignment] Not enough values to unpack: Expected 4
- jax/_src/pallas/fuser/block_spec.py:854:10: error[invalid-return-type] Return type does not match returned value: expected `Sequence[BlockSpec]`, found `list[NoBlockSpec | @Todo(StarredExpression)]`
+ jax/_src/pallas/fuser/block_spec.py:854:10: error[invalid-return-type] Return type does not match returned value: expected `Sequence[BlockSpec]`, found `list[NoBlockSpec | Unknown | BlockSpec]`
- jax/_src/pallas/hlo_interpreter.py:399:33: error[invalid-argument-type] Argument to function `pad` is incorrect: Expected `Sequence[tuple[int, int, int]]`, found `list[tuple[@Todo(StarredExpression), int] | Unknown]`
+ jax/_src/pallas/hlo_interpreter.py:399:33: error[invalid-argument-type] Argument to function `pad` is incorrect: Expected `Sequence[tuple[int, int, int]]`, found `list[tuple[*tuple[int | Unknown, ...], int] | Unknown]`
- jax/experimental/mosaic/gpu/utils.py:2042:10: error[invalid-return-type] Return type does not match returned value: expected `tuple[Unknown, Unknown, Unknown, Unknown]`, found `tuple[@Todo(StarredExpression), Unknown]`
- jax/experimental/pallas/ops/gpu/all_gather_mgpu.py:91:6: error[unsupported-operator] Operator `%` is not supported between objects of type `@Todo(StarredExpression) | int` and `int | None`
+ jax/experimental/pallas/ops/gpu/all_gather_mgpu.py:91:6: error[unsupported-operator] Operator `%` is not supported between objects of type `int` and `int | None`
- jax/experimental/pallas/ops/gpu/reduce_scatter_mgpu.py:106:6: error[unsupported-operator] Operator `%` is not supported between objects of type `Unknown | Literal[1]` and `int | None`
+ jax/experimental/pallas/ops/gpu/reduce_scatter_mgpu.py:106:6: error[unsupported-operator] Operator `%` is not supported between objects of type `int` and `int | None`
- jax/experimental/slab/slab.py:248:3: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 2855 diagnostics
+ Found 2851 diagnostics

altair (https://github.com/vega/altair)
- tests/vegalite/v6/schema/test_channels.py:56:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1061 diagnostics
+ Found 1060 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/__init__.py:108:11: error[invalid-assignment] Object of type `tuple[Literal["Model"], @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]` is not assignable to `tuple[@Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]`
- src/bokeh/models/annotations/__init__.py:44:11: error[invalid-assignment] Object of type `tuple[@Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]` is not assignable to `tuple[@Todo(StarredExpression), @Todo(StarredExpression), @Todo(StarredExpression)]`
+ src/bokeh/server/tornado.py:448:41: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, type[RequestHandler]] | tuple[str, type[RequestHandler], dict[str, Any]]`, found `tuple[str | Unknown, *tuple[type[RequestHandler] | dict[str, Any] | Unknown, ...], dict[Unknown | str, Unknown | dict[str, Unknown] | str | None | bool]]`
+ src/bokeh/server/tornado.py:451:37: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, type[RequestHandler]] | tuple[str, type[RequestHandler], dict[str, Any]]`, found `tuple[str | Unknown, *tuple[type[RequestHandler] | dict[str, Any] | Unknown, ...]]`

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/types/arrays.py:226:32: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@concat, ArrayValue, @Todo(StarredExpression)]`
+ ibis/expr/types/arrays.py:226:32: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@concat, ArrayValue, *tuple[ArrayValue, ...]]`
- ibis/expr/types/arrays.py:1014:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@zip, ArrayValue, @Todo(StarredExpression)]`
+ ibis/expr/types/arrays.py:1014:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Array[Unknown], Any], ...]`, found `tuple[Self@zip, ArrayValue, *tuple[ArrayValue, ...]]`
- ibis/expr/types/generic.py:384:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[Unknown, Any], ...]`, found `tuple[Self@coalesce, @Todo(StarredExpression)]`
+ ibis/expr/types/generic.py:384:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[ibis.expr.operations.core.Value[Unknown, Any], ...]`, found `tuple[Self@coalesce, *tuple[ibis.expr.types.generic.Value, ...]]`
- ibis/expr/types/strings.py:1580:33: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[String, Any], ...]`, found `tuple[Self@concat, str | StringValue, @Todo(StarredExpression)]`
+ ibis/expr/types/strings.py:1580:33: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Value[String, Any], ...]`, found `tuple[Self@concat, str | StringValue, *tuple[str | StringValue, ...]]`

sympy (https://github.com/sympy/sympy)
- sympy/parsing/latex/__init__.py:117:12: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:118:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:120:59: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:125:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:126:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:128:53: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:133:43: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:135:30: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:136:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:139:37: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:140:30: error[index-out-of-bounds] Index 6 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:140:47: error[index-out-of-bounds] Index 4 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- sympy/parsing/latex/__init__.py:141:65: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo(StarredExpression), @Todo(StarredExpression)]` with length 2
- Found 15597 diagnostics
+ Found 15584 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5169 diagnostics
+ Found 5167 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/arbitrum_one/modules/thegraph/balances.py:153:43: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ChecksumAddress, ChecksumAddress, ChecksumAddress, ChecksumAddress, int]`, found `tuple[@Todo(StarredExpression), Any]`
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

core (https://github.com/home-assistant/core)
- homeassistant/components/esphome/light.py:337:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown]`
- homeassistant/components/esphome/light.py:358:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown, Unknown]`
- homeassistant/components/esphome/light.py:363:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown, Unknown]`
- homeassistant/components/flux_led/util.py:98:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), int]`
- homeassistant/components/flux_led/util.py:99:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), int]`
- homeassistant/components/flux_led/util.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), int, int]`
- homeassistant/components/flux_led/util.py:115:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo(StarredExpression), int, int]`
- homeassistant/components/knx/light.py:375:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int] | None`, found `tuple[@Todo(StarredExpression), Unknown & ~None]`
- homeassistant/components/rfxtrx/__init__.py:212:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/rfxtrx/entity.py:24:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/shelly/light.py:229:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), int]`
- homeassistant/components/shelly/light.py:404:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo(StarredExpression), Unknown]`
+ homeassistant/components/zwave_js/discovery.py:1382:17: error[invalid-argument-type] Argument is incorrect: Expected `type[ZWaveBaseEntity]`, found `type`
- Found 14513 diagnostics
+ Found 14502 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/interpolate/tests/test_bsplines.py:1807:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1819:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1832:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1842:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1856:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1867:9: error[invalid-assignment] Not enough values to unpack: Expected 3
- scipy/interpolate/tests/test_bsplines.py:1879:9: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 8082 diagnostics
+ Found 8075 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-11 16:40_

---

_Comment by @astral-sh-bot[bot] on 2026-01-11 16:45_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 3 | 19 | 3 |
| `invalid-return-type` | 0 | 17 | 4 |
| `invalid-argument-type` | 5 | 3 | 9 |
| `index-out-of-bounds` | 0 | 13 | 0 |
| `unused-ignore-comment` | 0 | 4 | 0 |
| `unsupported-operator` | 0 | 0 | 3 |
| `no-matching-overload` | 2 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `type-assertion-failure` | 0 | 0 | 1 |
| **Total** | **11** | **56** | **20** |


**[Full report with detailed diff](https://95300fec.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://95300fec.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @AlexWaygood on 2026-01-12 18:06_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-12 18:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-12 18:06_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-12 18:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:8441 on 2026-01-13 19:19_

Can we add a comment here about why we need to ignore type context in this case?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:8841 on 2026-01-13 19:22_

It's not clear to me what this is doing or why it's needed; a comment would be helpful.

---

_@carljm approved on 2026-01-13 19:24_

Looks great, thank you!

---

_@AlexWaygood reviewed on 2026-01-13 21:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:8441 on 2026-01-13 21:03_

It's basically just a bit of a hack to avoid too much complexity for now... I'm adding a comment.

We can revisit this later but I'm trying to cut scope for now because I've been sitting on this patch for weeks and I'd really like to get it landed 🙃

---

_Merged by @AlexWaygood on 2026-01-13 21:09_

---

_Closed by @AlexWaygood on 2026-01-13 21:09_

---

_Branch deleted on 2026-01-13 21:09_

---
