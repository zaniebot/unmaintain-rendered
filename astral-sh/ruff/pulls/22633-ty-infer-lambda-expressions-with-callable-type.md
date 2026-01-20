```yaml
number: 22633
title: "[ty] Infer lambda expressions with `Callable` type context"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: ibraheem/comprehension-tcx
head: ibraheem/lambda-tcx
created_at: 2026-01-16T22:48:02Z
updated_at: 2026-01-19T23:56:03Z
url: https://github.com/astral-sh/ruff/pull/22633
synced_at: 2026-01-20T00:49:15Z
```

# [ty] Infer lambda expressions with `Callable` type context

---

_@ibraheemdev_

Infer lambda expressions eagerly as part of their parent scope, and with type context. This allows us to infer more precise types for lambda expressions, as well as perform check assignability against `Callable` annotations.

Note that this does not change the inferred type of a lambda parameter with the body of the lambda, even if it is annotated. That part is a little more tricky, so will be addressed in a followup PR.

This PR is stacked on https://github.com/astral-sh/ruff/pull/22564.

---

_Label `ty` added by @ibraheemdev on 2026-01-16 22:48_

---

_Review requested from @carljm by @ibraheemdev on 2026-01-16 22:48_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-16 22:48_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-16 22:48_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-16 22:48_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 22:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 22:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_cmp.py:364:23: error[invalid-argument-type] Argument to function `cmp_using` is incorrect: Expected `((Any, Any, /) -> bool) | None`, found `(a: Any, b: Any) -> _NotImplementedType | (bool & Unknown)`
- tests/test_validators.py:228:32: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str, str, int, /) -> Match[str] | None) | None`, found `() -> Unknown`
+ tests/test_validators.py:228:32: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str, str, int, /) -> Match[str] | None) | None`, found `() -> None`
- tests/test_validators.py:448:52: error[invalid-argument-type] Argument to function `include` is incorrect: Expected `type | str | Attribute[Any]`, found `(val) -> Unknown`
+ tests/test_validators.py:448:52: error[invalid-argument-type] Argument to function `include` is incorrect: Expected `type | str | Attribute[Any]`, found `(val) -> Literal[True]`
- tests/test_validators.py:449:52: error[invalid-argument-type] Argument to function `exclude` is incorrect: Expected `type | str | Attribute[Any]`, found `(val) -> Unknown`
+ tests/test_validators.py:449:52: error[invalid-argument-type] Argument to function `exclude` is incorrect: Expected `type | str | Attribute[Any]`, found `(val) -> Literal[True]`
- Found 628 diagnostics
+ Found 629 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_core/_tempfile.py:108:9: error[invalid-assignment] Object of type `AsyncFile[str]` is not assignable to attribute `_async_file` of type `AsyncFile[AnyStr@TemporaryFile]`
- Found 92 diagnostics
+ Found 93 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/spec.py:4006:60: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `defaultdict[Unknown, Unknown] | None`
+ lib/spack/spack/spec.py:4006:60: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `defaultdict[Unknown, int] | None`
- lib/spack/spack/test/llnl/util/lock.py:1102:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1108:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1112:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1121:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1125:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/util/file_cache.py:159:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/file_cache.py:175:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 4337 diagnostics
+ Found 4330 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_futures.py:22:5: error[unresolved-attribute] Object of type `Future[Unknown]` has no attribute `on_computed`
+ asynq/tests/test_futures.py:22:5: error[unresolved-attribute] Object of type `Future[int]` has no attribute `on_computed`
- asynq/tests/test_futures.py:25:5: error[unresolved-attribute] Object of type `Future[Unknown]` has no attribute `on_computed`
+ asynq/tests/test_futures.py:25:5: error[unresolved-attribute] Object of type `Future[int]` has no attribute `on_computed`

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/_reloader.py:446:35: error[invalid-argument-type] Argument to function `signal` is incorrect: Expected `((int, FrameType | None, /) -> Any) | int | None`, found `(*args: int) -> Never`
- tests/live_apps/run.py:35:5: error[invalid-assignment] Object of type `(_) -> Unknown` is not assignable to attribute `address_string` of type `def address_string(self) -> str`
+ tests/live_apps/run.py:35:5: error[invalid-assignment] Object of type `(_) -> Any` is not assignable to attribute `address_string` of type `def address_string(self) -> str`
- Found 407 diagnostics
+ Found 408 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ tests/validation/test_validation.py:49:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((GraphQLSchema, GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType, FieldNode, /) -> GraphQLField | None) | None`, found `(*_args: GraphQLSchema) -> None`
- Found 641 diagnostics
+ Found 642 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/frameworks/constraints.py:25:37: error[invalid-assignment] Object of type `dict[str, ((str, str, str, dict[str, dict[str, Any]], /) -> bool) | ((cv: str, ov: str, *_: str) -> Unknown) | ((cv: str, ov: str, *_: str) -> bool)]` is not assignable to `dict[str, (str, str, str, dict[str, dict[str, Any]], /) -> bool]`
- Found 1114 diagnostics
+ Found 1115 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ src/aiortc/rtcdtlstransport.py:199:64: error[invalid-argument-type] Argument to bound method `set_verify` is incorrect: Expected `((Connection, X509, int, int, int, /) -> bool) | None`, found `(*args: Connection) -> Literal[True]`
- Found 194 diagnostics
+ Found 195 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ testing/_py/test_local.py:918:35: error[invalid-argument-type] Argument to bound method `sysfind` is incorrect: Expected `((local, /) -> bool) | None`, found `(x: local) -> None`
- Found 414 diagnostics
+ Found 415 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/engine/test_engine.py:60:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Engine, Any, /) -> Any`, found `() -> Unknown`
+ tests/ignite/engine/test_engine.py:60:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Engine, Any, /) -> Any`, found `() -> None`
- tests/ignite/engine/test_engine.py:63:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Engine, Any, /) -> Any`, found `(batch) -> Unknown`
+ tests/ignite/engine/test_engine.py:63:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Engine, Any, /) -> Any`, found `(batch: Engine) -> None`
- tests/ignite/engine/test_engine.py:66:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Engine, Any, /) -> Any`, found `(engine, batch, extra_arg) -> Unknown`
+ tests/ignite/engine/test_engine.py:66:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Engine, Any, /) -> Any`, found `(engine: Engine, batch: Any, extra_arg) -> None`
+ tests/ignite/handlers/test_checkpoint.py:102:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Engine, /) -> int | float) | None`, found `(e: Engine) -> dict[Unknown | str, Unknown | int]`
- Found 2034 diagnostics
+ Found 2035 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
+ httpx_caching/_utils.py:47:19: error[invalid-await] `Unknown | None` is not awaitable
- Found 27 diagnostics
+ Found 28 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye_contrib/plot_metrics.py:24:1: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 415 diagnostics
+ Found 416 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> str)`
- tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `str | ((str, /) -> str)`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `str | ((str, /) -> str)`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> str)`
- tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> str)`
- tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `list[str]`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `list[str]`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> str)`

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/specs/openapi/adapter/parameters.py:998:48: error[invalid-argument-type] Argument to bound method `filter` is incorrect: Expected `(GeneratedValue, /) -> Any`, found `Unknown | ((parameters: dict[str, object]) -> bool) | ((headers: dict[str, object]) -> bool) | ((query: dict[str, object]) -> bool)`
+ src/schemathesis/specs/openapi/adapter/parameters.py:1010:45: error[invalid-argument-type] Argument to bound method `map` is incorrect: Expected `(GeneratedValue, /) -> Unknown`, found `def quote_all(parameters: dict[str, Any]) -> dict[str, Any]`
+ src/schemathesis/specs/openapi/adapter/parameters.py:1015:45: error[invalid-argument-type] Argument to bound method `map` is incorrect: Expected `(GeneratedValue, /) -> Unknown`, found `def jsonify_python_specific_types(value: dict[str, Any]) -> dict[str, Any]`
- Found 282 diagnostics
+ Found 285 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/contrib/emscripten/test_emscripten.py:387:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `is_cross_origin_isolated` of type `def is_cross_origin_isolated() -> bool`
+ test/contrib/emscripten/test_emscripten.py:387:9: error[invalid-assignment] Object of type `() -> Literal[False]` is not assignable to attribute `is_cross_origin_isolated` of type `def is_cross_origin_isolated() -> bool`

Expression (https://github.com/cognitedata/Expression)
+ tests/test_result.py:315:46: error[invalid-argument-type] Argument to bound method `filter_with` is incorrect: Expected `(Any, /) -> Literal["original error"]`, found `(value: Any) -> str`
+ tests/test_result.py:517:26: error[invalid-argument-type] Argument to bound method `or_else_with` is incorrect: Expected `(Any, /) -> Result[Literal["good"], Any]`, found `(error: Any) -> Result[str, Any]`
+ tests/test_result.py:523:26: error[invalid-argument-type] Argument to bound method `or_else_with` is incorrect: Expected `(Any, /) -> Result[Literal["good"], Any]`, found `(error: Any) -> Result[str, Any]`
+ tests/test_result.py:535:26: error[invalid-argument-type] Argument to bound method `or_else_with` is incorrect: Expected `(Literal["original error"], /) -> Result[Any, Literal["original error"]]`, found `(error: Literal["original error"]) -> Result[Any, str]`
- Found 205 diagnostics
+ Found 209 diagnostics

vision (https://github.com/pytorch/vision)
+ references/depth/stereo/transforms.py:124:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None, Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None], tuple[Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None, Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown | None, ...], tuple[Unknown | None, ...]]`
- Found 1403 diagnostics
+ Found 1404 diagnostics

operator (https://github.com/canonical/operator)
- ops/_private/harness.py:2075:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 132 diagnostics
+ Found 131 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/visualization/matplotlib/_contour.py:168:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `filter[Unknown | str | int | float | None]`
- tests/study_tests/test_optimize.py:73:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/study_tests/test_optimize.py:78:86: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/visualization_tests/test_utils.py:177:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 580 diagnostics
+ Found 578 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/livereload_tests.py:21:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `close` of type `def close(self) -> None`
+ mkdocs/tests/livereload_tests.py:21:9: error[invalid-assignment] Object of type `() -> None` is not assignable to attribute `close` of type `def close(self) -> None`

discord.py (https://github.com/Rapptz/discord.py)
- discord/enums.py:97:5: error[invalid-assignment] Object of type `(self) -> Unknown` is not assignable to attribute `__repr__` of type `def __repr__(self) -> str`
+ discord/enums.py:97:5: error[invalid-assignment] Object of type `(self) -> str` is not assignable to attribute `__repr__` of type `def __repr__(self) -> str`
- discord/enums.py:98:5: error[invalid-assignment] Object of type `(self) -> Unknown` is not assignable to attribute `__str__` of type `def __str__(self) -> str`
+ discord/enums.py:98:5: error[invalid-assignment] Object of type `(self) -> str` is not assignable to attribute `__str__` of type `def __str__(self) -> str`
- discord/enums.py:100:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__le__` of type `def __le__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:100:9: error[invalid-assignment] Object of type `(self, other) -> Literal[False] | Unknown` is not assignable to attribute `__le__` of type `def __le__(self, value: tuple[Unknown, ...], /) -> bool`
- discord/enums.py:101:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__ge__` of type `def __ge__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:101:9: error[invalid-assignment] Object of type `(self, other) -> Literal[False] | Unknown` is not assignable to attribute `__ge__` of type `def __ge__(self, value: tuple[Unknown, ...], /) -> bool`
- discord/enums.py:102:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__lt__` of type `def __lt__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:102:9: error[invalid-assignment] Object of type `(self, other) -> Literal[False] | Unknown` is not assignable to attribute `__lt__` of type `def __lt__(self, value: tuple[Unknown, ...], /) -> bool`
- discord/enums.py:103:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__gt__` of type `def __gt__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:103:9: error[invalid-assignment] Object of type `(self, other) -> Literal[False] | Unknown` is not assignable to attribute `__gt__` of type `def __gt__(self, value: tuple[Unknown, ...], /) -> bool`
- discord/file.py:106:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `close` of type `def close(self) -> None`
+ discord/file.py:106:9: error[invalid-assignment] Object of type `() -> None` is not assignable to attribute `close` of type `def close(self) -> None`

pyppeteer (https://github.com/pyppeteer/pyppeteer)
+ pyppeteer/browser.py:66:13: error[invalid-argument-type] Argument to bound method `setClosedCallback` is incorrect: Expected `() -> None`, found `() -> bool`
- Found 87 diagnostics
+ Found 88 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/sources/test_altcloud.py:157:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `user_data_rhevm` of type `def user_data_rhevm(self) -> Unknown`
+ tests/unittests/sources/test_altcloud.py:157:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `user_data_rhevm` of type `def user_data_rhevm(self) -> Unknown`
- tests/unittests/sources/test_altcloud.py:169:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `user_data_vsphere` of type `def user_data_vsphere(self) -> Unknown`
+ tests/unittests/sources/test_altcloud.py:169:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `user_data_vsphere` of type `def user_data_vsphere(self) -> Unknown`
- tests/unittests/sources/test_altcloud.py:181:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `user_data_rhevm` of type `def user_data_rhevm(self) -> Unknown`
+ tests/unittests/sources/test_altcloud.py:181:9: error[invalid-assignment] Object of type `() -> Literal[False]` is not assignable to attribute `user_data_rhevm` of type `def user_data_rhevm(self) -> Unknown`
- tests/unittests/sources/test_altcloud.py:190:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `user_data_vsphere` of type `def user_data_vsphere(self) -> Unknown`
+ tests/unittests/sources/test_altcloud.py:190:9: error[invalid-assignment] Object of type `() -> Literal[False]` is not assignable to attribute `user_data_vsphere` of type `def user_data_vsphere(self) -> Unknown`
- tests/unittests/sources/test_altcloud.py:225:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `user_data_rhevm` of type `def user_data_rhevm(self) -> Unknown`
+ tests/unittests/sources/test_altcloud.py:225:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `user_data_rhevm` of type `def user_data_rhevm(self) -> Unknown`
- tests/unittests/sources/test_altcloud.py:235:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `user_data_vsphere` of type `def user_data_vsphere(self) -> Unknown`
+ tests/unittests/sources/test_altcloud.py:235:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `user_data_vsphere` of type `def user_data_vsphere(self) -> Unknown`
- tests/unittests/sources/test_exoscale.py:72:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
+ tests/unittests/sources/test_exoscale.py:72:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
- tests/unittests/sources/test_exoscale.py:114:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
+ tests/unittests/sources/test_exoscale.py:114:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
- tests/unittests/sources/test_exoscale.py:150:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
+ tests/unittests/sources/test_exoscale.py:150:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
- tests/unittests/sources/test_exoscale.py:216:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
+ tests/unittests/sources/test_exoscale.py:216:9: error[invalid-assignment] Object of type `() -> Literal[False]` is not assignable to attribute `ds_detect` of type `def ds_detect() -> Unknown`
+ tests/unittests/test_url_helper.py:518:31: warning[division-by-zero] Cannot divide object of type `Literal[1]` by zero
+ tests/unittests/test_url_helper.py:527:31: warning[division-by-zero] Cannot divide object of type `Literal[1]` by zero
+ tests/unittests/test_url_helper.py:585:32: warning[division-by-zero] Cannot divide object of type `Literal[1]` by zero
+ tests/unittests/test_url_helper.py:593:32: warning[division-by-zero] Cannot divide object of type `Literal[1]` by zero
- Found 1168 diagnostics
+ Found 1172 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:88:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_core/_tests/test_run.py:1048:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_util.py:139:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 487 diagnostics
+ Found 484 diagnostics

meson (https://github.com/mesonbuild/meson)
- docs/refman/main.py:42:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `colorize_console` of type `bound method _Logger.colorize_console() -> bool`
+ docs/refman/main.py:42:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `colorize_console` of type `bound method _Logger.colorize_console() -> bool`
- mesonbuild/dependencies/cuda.py:136:76: error[invalid-argument-type] Argument to function `version_compare_many` is incorrect: Expected `str`, found `Unknown | str | None`
- unittests/internaltests.py:245:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `get_default_include_dirs` of type `def get_default_include_dirs(self) -> list[str]`
+ unittests/internaltests.py:245:9: error[invalid-assignment] Object of type `() -> list[Unknown | str]` is not assignable to attribute `get_default_include_dirs` of type `def get_default_include_dirs(self) -> list[str]`
- unittests/internaltests.py:275:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `get_default_include_dirs` of type `def get_default_include_dirs(self) -> list[str]`
+ unittests/internaltests.py:275:9: error[invalid-assignment] Object of type `() -> list[Unknown | str]` is not assignable to attribute `get_default_include_dirs` of type `def get_default_include_dirs(self) -> list[str]`
+ unittests/machinefiletests.py:154:26: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Compiler | None`

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/registry.py:552:9: error[invalid-assignment] Object of type `(_) -> Unknown` is not assignable to attribute `changed` on type `Unknown | AdapterRegistry`
+ src/zope/interface/registry.py:552:9: error[invalid-assignment] Object of type `(_) -> None` is not assignable to attribute `changed` on type `Unknown | AdapterRegistry`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/book_providers.py:829:9: error[invalid-argument-type] Argument to function `multisort_best` is incorrect: Expected `list[tuple[Literal["min", "max"], (Unknown | tuple[Edition, AbstractBookProvider[Unknown] | None], /) -> int | float]]`, found `list[Unknown | tuple[str, (rec) -> Unknown]]`
+ openlibrary/book_providers.py:829:9: error[invalid-argument-type] Argument to function `multisort_best` is incorrect: Expected `list[tuple[Literal["min", "max"], (Unknown | tuple[Edition, AbstractBookProvider[Unknown] | None], /) -> int | float]]`, found `list[Unknown | tuple[str, (rec) -> int | float]]`
- openlibrary/conftest.py:110:5: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `init_plugin` of type `def init_plugin() -> Unknown`
+ openlibrary/conftest.py:110:5: error[invalid-assignment] Object of type `() -> None` is not assignable to attribute `init_plugin` of type `def init_plugin() -> Unknown`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-github/prefect_github/utils.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `list[Unknown]`, found `defaultdict[Unknown, Unknown]`
+ src/integrations/prefect-github/prefect_github/utils.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `list[Unknown]`, found `defaultdict[Unknown, list[Unknown]]`

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/main.py:165:9: error[invalid-assignment] Object of type `(*args) -> Unknown` is not assignable to attribute `default_user_agent` of type `def default_user_agent(name: str = "python-requests") -> str`
+ cwltool/main.py:165:9: error[invalid-assignment] Object of type `(*args) -> str` is not assignable to attribute `default_user_agent` of type `def default_user_agent(name: str = "python-requests") -> str`

manticore (https://github.com/trailofbits/manticore)
- manticore/native/plugins.py:13:9: error[invalid-assignment] Object of type `(...) -> Unknown` is not assignable to attribute `_publish` on type `Unknown | None`
+ manticore/native/plugins.py:13:9: error[invalid-assignment] Object of type `(...) -> None` is not assignable to attribute `_publish` on type `Unknown | None`

setuptools (https://github.com/pypa/setuptools)
- setuptools/build_meta.py:107:5: error[invalid-assignment] Object of type `(attrs) -> Unknown` is not assignable to attribute `_install_setup_requires` of type `def _install_setup_requires(attrs) -> Unknown`
+ setuptools/build_meta.py:107:5: error[invalid-assignment] Object of type `(attrs) -> None` is not assignable to attribute `_install_setup_requires` of type `def _install_setup_requires(attrs) -> Unknown`

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/disasm/disassembly.py:89:53: error[invalid-assignment] Object of type `defaultdict[int, int | None]` is not assignable to `defaultdict[int, int]`
+ pwndbg/aglib/disasm/disassembly.py:94:60: error[invalid-assignment] Object of type `defaultdict[int, int | None]` is not assignable to `defaultdict[int, int]`
+ pwndbg/aglib/disasm/disassembly.py:97:79: error[invalid-assignment] Object of type `defaultdict[int, PwndbgInstruction | None]` is not assignable to `defaultdict[int, PwndbgInstruction]`
+ pwndbg/aglib/heap/ptmalloc.py:1420:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ pwndbg/aglib/heap/ptmalloc.py:1421:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ pwndbg/aglib/heap/ptmalloc.py:1422:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ pwndbg/aglib/heap/ptmalloc.py:1423:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ pwndbg/dbg_mod/gdb/__init__.py:181:40: error[invalid-argument-type] Argument is incorrect: Expected `() -> T@selection`, found `() -> Frame`
+ pwndbg/dbg_mod/gdb/__init__.py:200:36: error[invalid-argument-type] Argument is incorrect: Expected `() -> T@selection`, found `() -> Frame`
+ pwndbg/dbg_mod/gdb/__init__.py:390:36: error[invalid-argument-type] Argument is incorrect: Expected `() -> T@selection`, found `() -> InferiorThread`
- pwndbg/gdblib/tui/context.py:221:24: error[invalid-assignment] Object of type `(...) -> Unknown` is not assignable to `def _ansi_substr(self, line: str, start_char: int, end_char: int) -> str`
+ pwndbg/gdblib/tui/context.py:221:24: error[invalid-assignment] Object of type `(...) -> Unknown | str` is not assignable to `def _ansi_substr(self, line: str, start_char: int, end_char: int) -> str`
- Found 1999 diagnostics
+ Found 2009 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/wrapper/_implementations.py:1735:52: error[invalid-assignment] Object of type `((x: str | None) -> object) | (((str | None, /) -> str | None) & ~Top[Mapping[Unknown, object]])` is not assignable to `(str | None, /) -> str | None`
- Found 521 diagnostics
+ Found 522 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/tests/test_datatree_mapping.py:92:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_datatree_mapping.py:104:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1753 diagnostics
+ Found 1751 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/debugging/_debugger.py:269:13: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Unknown | DerivedVariable[int | float | Unknown]`
+ ddtrace/debugging/_debugger.py:269:13: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Unknown | DerivedVariable[int | float]`
- ddtrace/debugging/_redaction.py:123:12: warning[possibly-missing-attribute] Attribute `search` may be missing on object of type `(Unknown & ~None) | DerivedVariable[Unknown]`
+ ddtrace/debugging/_redaction.py:123:12: warning[possibly-missing-attribute] Attribute `search` may be missing on object of type `(Unknown & ~None) | DerivedVariable[Pattern[str] | None]`
- ddtrace/debugging/_signal/utils.py:227:38: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((Any, /) -> bool) | ((_) -> Unknown)`
- ddtrace/debugging/_signal/utils.py:257:38: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((Any, /) -> bool) | ((_) -> Unknown)`
- ddtrace/debugging/_signal/utils.py:313:41: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((Any, /) -> bool) | ((_) -> Unknown)`
- ddtrace/debugging/_signal/utils.py:332:34: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((Any, /) -> bool) | ((_) -> Unknown)`
- ddtrace/debugging/_signal/utils.py:358:37: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((Any, /) -> bool) | ((_) -> Unknown)`
+ ddtrace/debugging/_signal/utils.py:227:38: error[unresolved-attribute] Object of type `(Any, /) -> bool` has no attribute `__name__`
+ ddtrace/debugging/_signal/utils.py:257:38: error[unresolved-attribute] Object of type `(Any, /) -> bool` has no attribute `__name__`
+ ddtrace/debugging/_signal/utils.py:313:41: error[unresolved-attribute] Object of type `(Any, /) -> bool` has no attribute `__name__`
+ ddtrace/debugging/_signal/utils.py:332:34: error[unresolved-attribute] Object of type `(Any, /) -> bool` has no attribute `__name__`
+ ddtrace/debugging/_signal/utils.py:358:37: error[unresolved-attribute] Object of type `(Any, /) -> bool` has no attribute `__name__`
- ddtrace/debugging/_uploader.py:80:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | DerivedVariable[str | Unknown]`
+ ddtrace/debugging/_uploader.py:80:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | DerivedVariable[str | (Unknown & ~AlwaysFalsy)]`
- ddtrace/debugging/_uploader.py:86:66: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | DerivedVariable[str | Unknown]`
+ ddtrace/debugging/_uploader.py:86:66: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | DerivedVariable[str | (Unknown & ~AlwaysFalsy)]`
- ddtrace/internal/symbol_db/symbols.py:527:34: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ ddtrace/internal/symbol_db/symbols.py:527:34: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- ddtrace/internal/symbol_db/symbols.py:545:36: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ ddtrace/internal/symbol_db/symbols.py:545:36: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
+ ddtrace/vendor/xmltodict.py:114:5: error[invalid-assignment] Object of type `(*x: str) -> Literal[1]` is not assignable to attribute `ExternalEntityRefHandler` of type `((str, str | None, str | None, str | None, /) -> int) | None`
+ tests/appsec/appsec/test_asm_request_context.py:99:9: error[invalid-argument-type] Argument is incorrect: Expected `(() -> bool) | None`, found `() -> Literal[42]`
- tests/ci_visibility/test_atr.py:74:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `efd_is_faulty_session` of type `def efd_is_faulty_session(self) -> Unknown`
+ tests/ci_visibility/test_atr.py:74:9: error[invalid-assignment] Object of type `() -> Literal[False]` is not assignable to attribute `efd_is_faulty_session` of type `def efd_is_faulty_session(self) -> Unknown`
- tests/ci_visibility/test_atr.py:169:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `efd_is_faulty_session` of type `def efd_is_faulty_session(self) -> Unknown`
+ tests/ci_visibility/test_atr.py:169:9: error[invalid-assignment] Object of type `() -> Literal[False]` is not assignable to attribute `efd_is_faulty_session` of type `def efd_is_faulty_session(self) -> Unknown`
- tests/ci_visibility/test_atr.py:175:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `get_session` of type `def get_session(self) -> TestVisibilitySession | None`
+ tests/ci_visibility/test_atr.py:175:9: error[invalid-assignment] Object of type `() -> TestVisibilitySession` is not assignable to attribute `get_session` of type `def get_session(self) -> TestVisibilitySession | None`
- tests/debugging/mocking.py:192:9: error[invalid-assignment] Object of type `(_) -> Unknown` is not assignable to attribute `register` of type `def register[**_P, _T](func: (**_P@register) -> _T, /, *args: _P.args, **kwargs: _P.kwargs) -> (**_P@register) -> _T`
+ tests/debugging/mocking.py:192:9: error[invalid-assignment] Object of type `(_) -> None` is not assignable to attribute `register` of type `def register[**_P, _T](func: (**_P@register) -> _T, /, *args: _P.args, **kwargs: _P.kwargs) -> (**_P@register) -> _T`
- tests/internal/symbol_db/test_config.py:8:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ tests/internal/symbol_db/test_config.py:8:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- tests/internal/symbol_db/test_config.py:9:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ tests/internal/symbol_db/test_config.py:9:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- tests/internal/symbol_db/test_config.py:10:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ tests/internal/symbol_db/test_config.py:10:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- tests/internal/symbol_db/test_config.py:12:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ tests/internal/symbol_db/test_config.py:12:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- tests/internal/symbol_db/test_config.py:13:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ tests/internal/symbol_db/test_config.py:13:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- tests/internal/symbol_db/test_config.py:14:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Unknown]`
+ tests/internal/symbol_db/test_config.py:14:12: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | DerivedVariable[Pattern[Unknown] | Pattern[str]]`
- tests/testing/internal/test_telemetry.py:396:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `is_benchmark` of type `def is_benchmark(self) -> bool`
+ tests/testing/internal/test_telemetry.py:396:9: error[invalid-assignment] Object of type `() -> Literal[True]` is not assignable to attribute `is_benchmark` of type `def is_benchmark(self) -> bool`
- Found 8414 diagnostics
+ Found 8416 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/SelfTest/Protocol/test_KDF.py:142:26: error[invalid-argument-type] Argument to function `PBKDF2` is incorrect: Expected `((int, /) -> bytes) | None`, found `(p, s) -> Unknown`
+ lib/Crypto/SelfTest/Protocol/test_KDF.py:142:26: error[invalid-argument-type] Argument to function `PBKDF2` is incorrect: Expected `((int, /) -> bytes) | None`, found `(p: int, s) -> bytes`

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/client_reqrep.py:211:66: error[invalid-assignment] Object of type `(*_: ClientResponse) -> Literal["utf-8"]` is not assignable to `(ClientResponse, bytes, /) -> str`
- Found 181 diagnostics
+ Found 182 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/annotations/geometry.py:298:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> Unknown` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/annotations/geometry.py:298:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> BoxInteractionHandles` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/annotations/geometry.py:298:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BoxInteractionHandles | UndefinedType | IntrinsicType`, found `() -> Unknown`
+ src/bokeh/models/annotations/geometry.py:298:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BoxInteractionHandles | UndefinedType | IntrinsicType`, found `() -> BoxInteractionHandles`
+ src/bokeh/models/annotations/legends.py:551:9: error[invalid-argument-type] Argument to bound method `accepts` is incorrect: Expected `(Any, /) -> LegendItem`, found `(items: Any) -> list[Unknown | LegendItem]`
+ src/bokeh/models/layouts.py:587:18: error[invalid-argument-type] Argument to bound method `accepts` is incorrect: Expected `(Any, /) -> TabPanel`, found `(items: Any) -> list[Unknown | TabPanel]`
- src/bokeh/models/renderers/graph_renderer.py:84:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> Unknown` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/renderers/graph_renderer.py:84:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> GlyphRenderer[Scatter]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/renderers/graph_renderer.py:84:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GlyphRenderer[Unknown] | UndefinedType | IntrinsicType`, found `() -> Unknown`
+ src/bokeh/models/renderers/graph_renderer.py:84:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GlyphRenderer[Unknown] | UndefinedType | IntrinsicType`, found `() -> GlyphRenderer[Scatter]`
- src/bokeh/models/renderers/graph_renderer.py:89:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> Unknown` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/renderers/graph_renderer.py:89:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> GlyphRenderer[MultiLine]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/renderers/graph_renderer.py:89:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GlyphRenderer[Unknown] | UndefinedType | IntrinsicType`, found `() -> Unknown`
+ src/bokeh/models/renderers/graph_renderer.py:89:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GlyphRenderer[Unknown] | UndefinedType | IntrinsicType`, found `() -> GlyphRenderer[MultiLine]`
- src/bokeh/models/tools.py:573:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> Unknown` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/tools.py:573:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> BoxAnnotation` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/tools.py:573:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BoxAnnotation | UndefinedType | IntrinsicType`, found `() -> Unknown`
+ src/bokeh/models/tools.py:573:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BoxAnnotation | UndefinedType | IntrinsicType`, found `() -> BoxAnnotation`
- Found 871 diagnostics
+ Found 873 diagnostics

cryptography (https://github.com/pyca/cryptography)
- tests/hazmat/primitives/test_dsa.py:940:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_dsa.py:955:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_dsa.py:970:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_ec.py:1020:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_ec.py:1036:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_ec.py:1052:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_ec.py:1363:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_ec.py:1411:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/hazmat/primitives/test_ec.py:1646:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 36 diagnostics
+ Found 27 diagnostics

jax (https://github.com/google/jax)
- jax/_src/ad_checkpoint.py:805:16: error[unresolved-attribute] Unresolved attribute `in_cts_zero` on type `() -> Unknown`
+ jax/_src/ad_checkpoint.py:805:16: error[unresolved-attribute] Unresolved attribute `in_cts_zero` on type `() -> None`
- jax/_src/ad_checkpoint.py:814:28: error[unresolved-attribute] Object of type `() -> Unknown` has no attribute `in_cts_zero`
+ jax/_src/ad_checkpoint.py:814:28: error[unresolved-attribute] Object of type `() -> None` has no attribute `in_cts_zero`
- jax/_src/ad_checkpoint.py:953:32: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, name) -> Unknown`
+ jax/_src/ad_checkpoint.py:953:32: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, name) -> list[Unknown]`
- jax/_src/error_check.py:278:5: error[invalid-argument-type] Argument to function `register_pytree_node_serialization` is incorrect: Expected `_SerializeAuxData`, found `(x) -> Unknown`
+ jax/_src/error_check.py:278:5: error[invalid-argument-type] Argument to function `register_pytree_node_serialization` is incorrect: Expected `_SerializeAuxData`, found `(x) -> bytes`
- jax/_src/error_check.py:281:5: error[invalid-argument-type] Argument to function `register_pytree_node_serialization` is incorrect: Expected `_DeserializeAuxData`, found `(x) -> Unknown`
+ jax/_src/error_check.py:281:5: error[invalid-argument-type] Argument to function `register_pytree_node_serialization` is incorrect: Expected `_DeserializeAuxData`, found `(x) -> Any`
- jax/_src/export/_export.py:535:5: error[invalid-argument-type] Argument to function `register_pytree_node_serialization` is incorrect: Expected `_DeserializeAuxData`, found `(b) -> Unknown`
+ jax/_src/export/_export.py:535:5: error[invalid-argument-type] Argument to function `register_pytree_node_serialization` is incorrect: Expected `_DeserializeAuxData`, found `(b) -> Any`
- jax/_src/hijax.py:454:7: error[unresolved-attribute] Unresolved attribute `out_nzs` on type `() -> Unknown`
+ jax/_src/hijax.py:454:7: error[unresolved-attribute] Unresolved attribute `out_nzs` on type `() -> None`
- jax/_src/interpreters/mlir.py:2829:44: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x) -> Unknown`
+ jax/_src/interpreters/mlir.py:2829:44: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x) -> list[Unknown]`
- jax/_src/lax/lax.py:5115:37: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> Unknown`
+ jax/_src/lax/lax.py:5115:37: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> list[Unknown]`
- jax/_src/lax/lax.py:5154:39: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> Unknown`
+ jax/_src/lax/lax.py:5154:39: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> list[Unknown]`
- jax/_src/lax/lax.py:8459:32: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x) -> Unknown`
+ jax/_src/lax/lax.py:8459:32: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x) -> list[Unknown]`
- jax/_src/lax/lax.py:8478:36: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, _) -> Unknown`
+ jax/_src/lax/lax.py:8478:36: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, _) -> list[Unknown]`
- jax/_src/lax/lax.py:8945:31: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, y) -> Unknown`
+ jax/_src/lax/lax.py:8945:31: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, y) -> list[Unknown]`
- jax/_src/lax/parallel.py:2396:38: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> Unknown`
+ jax/_src/lax/parallel.py:2396:38: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> list[Unknown]`
- jax/_src/lax/parallel.py:2676:36: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> Unknown`
+ jax/_src/lax/parallel.py:2676:36: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> list[Unknown]`
- jax/_src/lax/parallel.py:2722:47: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> Unknown`
+ jax/_src/lax/parallel.py:2722:47: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> list[Unknown]`
- jax/_src/lax/parallel.py:2766:50: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> Unknown`
+ jax/_src/lax/parallel.py:2766:50: error[invalid-argument-type] Argument to function `register_lowering` is incorrect: Expected `LoweringRule`, found `(ctx, x, *, axes) -> list[Unknown]`
- jax/_src/pallas/mosaic_gpu/lowering.py:3335:17: error[

... (truncated 254 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2026-01-16 22:58_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 23:03_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 28 | 17 | 102 |
| `invalid-assignment` | 13 | 0 | 46 |
| `possibly-missing-attribute` | 7 | 5 | 10 |
| `unused-ignore-comment` | 0 | 22 | 0 |
| `unresolved-attribute` | 5 | 0 | 7 |
| `invalid-return-type` | 2 | 0 | 5 |
| `type-assertion-failure` | 0 | 5 | 1 |
| `unsupported-operator` | 5 | 0 | 0 |
| `not-subscriptable` | 4 | 0 | 0 |
| `invalid-await` | 3 | 0 | 0 |
| `no-matching-overload` | 3 | 0 | 0 |
| **Total** | **70** | **49** | **171** |


**[Full report with detailed diff](https://b3ddcfb7.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://b3ddcfb7.ty-ecosystem-ext.pages.dev/timing))



---

_@ibraheemdev reviewed on 2026-01-16 23:25_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/cycle.md`:141 on 2026-01-16 23:25_

The revealed type here is a little unfortunate. Ideally it would just be `() -> Unknown | (() -> Divergent)`, but I'm not sure that is an issue related to this PR.

---

_Comment by @AlexWaygood on 2026-01-16 23:26_

> This PR is stacked on #22564.

The weird typing- conformance comment should be fixed if you rebase that PR on main, and then this PR on that PR (sorry for the teething problems there with the new workflow...)

---

_Comment by @MichaReiser on 2026-01-19 08:06_

There seems to be a new fuzzer panic 

---

_Comment by @codspeed-hq[bot] on 2026-01-19 23:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **improve performance by 30.32%**




`⚡ 4` improved benchmarks  
`✅ 19` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Flambda-tcx?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 4.7 s | 4.5 s | +5.58% |
| ⚡ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Flambda-tcx?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 21.9 s | 21 s | +4.4% |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Flambda-tcx?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 10.4 s | 8 s | +30.32% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Flambda-tcx?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 1.2 s | 1.2 s | +5.13% |
---

<sub>Comparing <code>ibraheem/lambda-tcx</code> (7d33c41) with <code>ibraheem/comprehension-tcx</code> (fa2b058)[^unexpected-base]</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Flambda-tcx?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Flambda-tcx?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).
[^unexpected-base]: No successful run was found on <code>ibraheem/comprehension-tcx</code> (109d2a5) during the generation of this report, so cafeac0 was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.


---

_Converted to draft by @ibraheemdev on 2026-01-19 23:56_

---
