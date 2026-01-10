```yaml
number: 20591
title: "[ty] Use `Top` materializations for `TypeIs` special form"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/typeis-top
created_at: 2025-09-26T11:40:46Z
updated_at: 2025-09-26T17:24:44Z
url: https://github.com/astral-sh/ruff/pull/20591
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Use `Top` materializations for `TypeIs` special form

---

_Pull request opened by @AlexWaygood on 2025-09-26 11:40_

## Summary

Typeshed has many functions that return `TypeIs[SomeCovariantGeneric[Any]]`. This is somewhat unfortunate for ty, because for our purposes it's almost always better to intersect with `CovariantGeneric[object]` for the purposes of type narrowing, but it's [somewhat hard](https://github.com/python/typeshed/pull/14784) to change these in typeshed without creating false-positive diagnostics for users of other type checkers. This PR therefore introduces a workaround for the typeshed issue: under the hood, we convert the type argument passed to `TypeIs` to its top materialization before intersecting with that type.

## Test Plan

I added an mdtest that fails on `main` without this change. It's similar to the false-positive diagnostics relating to the use of [`asyncio.iscoroutine`](https://github.com/python/typeshed/blob/6af8f287bb3eae6c6d8839c69c39f07ee7057517/stdlib/asyncio/coroutines.pyi#L28) that we see going away on `homeassistant/core` in the mypy_primer report.

Some more ecosystem analysis is found in https://github.com/astral-sh/ruff/pull/20591#issuecomment-3338847554.

---

_Label `ty` added by @AlexWaygood on 2025-09-26 11:40_

---

_Comment by @github-actions[bot] on 2025-09-26 11:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-26 11:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/filesystem.py:330:21: warning[possibly-missing-attribute] Attribute `replace` on type `(str & ~((...) -> object)) | (((Match[Unknown], /) -> str) & ~((...) -> object))` may be missing
+ lib/spack/spack/llnl/util/filesystem.py:330:21: warning[possibly-missing-attribute] Attribute `replace` on type `(str & ~(() -> object)) | (((Match[Unknown], /) -> str) & ~(() -> object))` may be missing
- lib/spack/spack/vendor/jinja2/runtime.py:734:17: error[invalid-assignment] Object of type `(Unknown & ~((...) -> object)) | bool | (((str | None, /) -> bool) & ~((...) -> object))` is not assignable to `bool | None`
+ lib/spack/spack/vendor/jinja2/runtime.py:734:17: error[invalid-assignment] Object of type `(Unknown & ~(() -> object)) | bool | (((str | None, /) -> bool) & ~(() -> object))` is not assignable to `bool | None`

jinja (https://github.com/pallets/jinja)
- src/jinja2/runtime.py:690:17: error[invalid-assignment] Object of type `(Unknown & ~((...) -> object)) | bool | (((str | None, /) -> bool) & ~((...) -> object))` is not assignable to `bool | None`
+ src/jinja2/runtime.py:690:17: error[invalid-assignment] Object of type `(Unknown & ~(() -> object)) | bool | (((str | None, /) -> bool) & ~(() -> object))` is not assignable to `bool | None`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:1236:29: error[unresolved-attribute] Type `FixtureFunction@__call__ & ~type[Any] & ~FixtureFunctionDefinition` has no attribute `__name__`
+ src/_pytest/fixtures.py:1236:29: error[unresolved-attribute] Type `FixtureFunction@__call__ & ~type & ~FixtureFunctionDefinition` has no attribute `__name__`
+ src/_pytest/fixtures.py:1383:9: error[invalid-argument-type] Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | (Sequence[object] & (() -> object)) | (((Any, /) -> object) & (() -> object)) | Unknown`
- src/_pytest/fixtures.py:1383:70: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `(Sequence[object] & ~((...) -> object)) | (((Any, /) -> object) & ~((...) -> object))`
+ src/_pytest/fixtures.py:1383:70: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `(Sequence[object] & ~(() -> object)) | (((Any, /) -> object) & ~(() -> object))`
- src/_pytest/fixtures.py:1658:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~((...) -> object) & ~str) | (((str, Config, /) -> Unknown) & ~((...) -> object) & ~str) | (Unknown & ~str)`
+ src/_pytest/fixtures.py:1658:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~(() -> object) & ~str) | (((str, Config, /) -> Unknown) & ~(() -> object) & ~str) | (Unknown & ~str)`
+ src/_pytest/python.py:1389:13: error[invalid-argument-type] Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | (Iterable[object] & (() -> object)) | (((Any, /) -> object) & (() -> object))`
- Found 485 diagnostics
+ Found 487 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/metadata/metadecor.py:523:34: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ beartype/_check/metadata/metadecor.py:523:34: error[unresolved-attribute] Type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT@callable_cached & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT@callable_cached & (() -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT@method_cached_arg_by_id & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT@method_cached_arg_by_id & (() -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Type `CallableT@property_cached & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Type `CallableT@property_cached & (() -> object)` has no attribute `__name__`
- beartype/_util/func/utilfuncscope.py:261:29: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/func/utilfuncscope.py:261:29: error[unresolved-attribute] Type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`
- beartype/_util/func/utilfuncscope.py:320:16: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__qualname__`
+ beartype/_util/func/utilfuncscope.py:320:16: error[unresolved-attribute] Type `((...) -> Unknown) & (() -> object)` has no attribute `__qualname__`
- beartype/_util/func/utilfuncscope.py:334:16: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__qualname__`
+ beartype/_util/func/utilfuncscope.py:334:16: error[unresolved-attribute] Type `((...) -> Unknown) & (() -> object)` has no attribute `__qualname__`
- beartype/_util/func/utilfunctest.py:907:16: error[unresolved-attribute] Type `((...) -> Unknown) & ~((...) -> Unknown)` has no attribute `__qualname__`
+ beartype/_util/func/utilfunctest.py:907:16: error[unresolved-attribute] Type `((...) -> Unknown) & ~(() -> object)` has no attribute `__qualname__`
- beartype/_util/func/utilfunctest.py:1100:16: warning[possibly-missing-attribute] Attribute `__name__` on type `(Any & ~MethodType) | ((...) -> Unknown)` may be missing
+ beartype/_util/func/utilfunctest.py:1100:16: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
- beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/utils.py:508:12: error[unsupported-operator] Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object)) | (@Todo & ~None)` with `Literal[0]`
+ src/werkzeug/utils.py:508:12: error[unsupported-operator] Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `(int & ~(() -> object)) | (((str | None, /) -> int | None) & ~(() -> object)) | (@Todo & ~None)` with `Literal[0]`
- src/werkzeug/utils.py:512:9: error[invalid-assignment] Object of type `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object)) | (@Todo & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ src/werkzeug/utils.py:512:9: error[invalid-assignment] Object of type `(int & ~(() -> object)) | (((str | None, /) -> int | None) & ~(() -> object)) | (@Todo & ~None)` is not assignable to attribute `max_age` of type `int | None`
- src/werkzeug/wsgi.py:246:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `((() -> None) & ~((...) -> object)) | (Iterable[() -> None] & ~((...) -> object))`
- Found 367 diagnostics
+ Found 366 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/routing.py:67:5: error[invalid-assignment] Object of type `((Request, /) -> Awaitable[Response] | Response) | partial[Unknown]` is not assignable to `(Request, /) -> Awaitable[Response]`
- Found 146 diagnostics
+ Found 147 diagnostics

ignite (https://github.com/pytorch/ignite)
- ignite/handlers/base_logger.py:52:29: error[not-iterable] Object of type `(list[str] & ~((...) -> object)) | (((str, Unknown, /) -> bool) & ~((...) -> object))` may not be iterable
+ ignite/handlers/base_logger.py:52:29: error[not-iterable] Object of type `(list[str] & ~(() -> object)) | (((str, Unknown, /) -> bool) & ~(() -> object))` may not be iterable

poetry (https://github.com/python-poetry/poetry)
- src/poetry/utils/cache.py:149:16: error[invalid-return-type] Return type does not match returned value: expected `T@FileCache`, found `(Unknown & ~None) | @Todo | (T@FileCache & ~((...) -> object)) | ((() -> T@FileCache) & ~((...) -> object))`
- Found 986 diagnostics
+ Found 985 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/escape.py:336:30: warning[possibly-missing-attribute] Attribute `strip` on type `(str & ~AlwaysFalsy & ~((...) -> object)) | (((str, /) -> str) & ~AlwaysFalsy & ~((...) -> object))` may be missing
+ tornado/escape.py:336:30: warning[possibly-missing-attribute] Attribute `strip` on type `(str & ~AlwaysFalsy & ~(() -> object)) | (((str, /) -> str) & ~AlwaysFalsy & ~(() -> object))` may be missing

vision (https://github.com/pytorch/vision)
- test/datasets_utils.py:835:52: error[invalid-argument-type] Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `@Todo | (Sequence[int] & ~((...) -> object)) | (int & ~((...) -> object)) | (((int, /) -> Sequence[int] | int) & ~((...) -> object))`
+ test/datasets_utils.py:835:52: error[invalid-argument-type] Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `@Todo | (Sequence[int] & ~(() -> object)) | (int & ~(() -> object)) | (((int, /) -> Sequence[int] | int) & ~(() -> object))`
+ torchvision/transforms/v2/_utils.py:149:16: error[invalid-return-type] Return type does not match returned value: expected `(Any, /) -> Any`, found `(str & (() -> object)) | (((Any, /) -> Any) & (() -> object))`
- Found 1473 diagnostics
+ Found 1474 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg_pool/psycopg_pool/_acompat.py:185:12: error[invalid-return-type] Return type does not match returned value: expected `T@ensure_async`, found `object`
- Found 698 diagnostics
+ Found 699 diagnostics

operator (https://github.com/canonical/operator)
+ ops/charm.py:1639:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 99 diagnostics
+ Found 100 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/messagequeue.py:373:36: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression
+ cki_lib/messagequeue.py:373:36: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not allowed in a type expression

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/environment/__init__.py:393:13: error[invalid-assignment] Object of type `(str & ((...) -> object)) | (((Node, /) -> bool) & ((...) -> object))` is not assignable to `Literal[False] | ((Node, /) -> bool)`
+ sphinx/environment/__init__.py:393:13: error[invalid-assignment] Object of type `(str & (() -> object)) | (((Node, /) -> bool) & (() -> object))` is not assignable to `Literal[False] | ((Node, /) -> bool)`
- sphinx/environment/__init__.py:397:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Literal[False] | ((Node, /) -> bool)].__getitem__(key: str, /) -> Literal[False] | ((Node, /) -> bool)` cannot be called with key of type `(str & ~((...) -> object)) | (((Node, /) -> bool) & ~((...) -> object))` on object of type `dict[str, Literal[False] | ((Node, /) -> bool)]`
+ sphinx/environment/__init__.py:397:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Literal[False] | ((Node, /) -> bool)].__getitem__(key: str, /) -> Literal[False] | ((Node, /) -> bool)` cannot be called with key of type `(str & ~(() -> object)) | (((Node, /) -> bool) & ~(() -> object))` on object of type `dict[str, Literal[False] | ((Node, /) -> bool)]`

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_ki.py:685:43: error[invalid-argument-type] Argument to function `_consume_async_generator` is incorrect: Expected `AsyncGenerator[None, None]`, found `AsyncGeneratorType[object, Never]`
+ src/trio/_core/_tests/test_ki.py:690:25: error[invalid-argument-type] Argument to bound method `send` is incorrect: Expected `Never`, found `None`
+ src/trio/_core/_tests/test_ki.py:690:25: error[invalid-argument-type] Argument to bound method `send` is incorrect: Expected `Never`, found `None`
- Found 665 diagnostics
+ Found 668 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/hooks.py:297:16: error[unresolved-attribute] Type `(str & ((...) -> object)) | (((...) -> Unknown) & ((...) -> object))` has no attribute `__name__`
+ src/schemathesis/hooks.py:297:16: error[unresolved-attribute] Type `(str & (() -> object)) | (((...) -> Unknown) & (() -> object))` has no attribute `__name__`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/strategy/informative_decorator.py:147:21: warning[possibly-missing-attribute] Attribute `format` on type `(str & ~AlwaysFalsy & ~((...) -> object)) | (((Any, /) -> str) & ~AlwaysFalsy & ~((...) -> object))` may be missing
+ freqtrade/strategy/informative_decorator.py:147:21: warning[possibly-missing-attribute] Attribute `format` on type `(str & ~AlwaysFalsy & ~(() -> object)) | (((Any, /) -> str) & ~AlwaysFalsy & ~(() -> object))` may be missing

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/concurrency/v1/sync.py:63:58: error[invalid-argument-type] Argument to function `emit_concurrency_acquisition_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `(Unknown & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Unknown] & ~Coroutine[Any, Any, Any])`
- src/prefect/concurrency/v1/sync.py:72:41: error[invalid-argument-type] Argument to function `emit_concurrency_release_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `(Unknown & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Unknown] & ~Coroutine[Any, Any, Any])`
- src/prefect/flows.py:288:34: warning[possibly-missing-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object) & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object))` may be missing
+ src/prefect/flows.py:288:34: warning[possibly-missing-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & (() -> object) & ~classmethod & ~staticmethod) | (@Todo & (() -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & (() -> object) & ~staticmethod) | (((...) -> R@__init__) & (() -> object))` may be missing
- src/prefect/flows.py:405:68: warning[possibly-missing-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object) & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object))` may be missing
+ src/prefect/flows.py:405:68: warning[possibly-missing-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & (() -> object) & ~classmethod & ~staticmethod) | (@Todo & (() -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & (() -> object) & ~staticmethod) | (((...) -> R@__init__) & (() -> object))` may be missing
+ src/prefect/runner/submit.py:174:35: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `FlowRun`, found `object`
- Found 3082 diagnostics
+ Found 3081 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:2252:47: error[invalid-argument-type] Argument to function `get_target_path` is incorrect: Expected `HasTarget`, found `(() -> object) & type`
- Found 567 diagnostics
+ Found 568 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/core/dataarray.py:5269:35: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ xarray/core/dataset.py:8080:35: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- Found 1619 diagnostics
+ Found 1621 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/batching.py:1008:37: error[invalid-argument-type] Argument to function `safe_zip` is incorrect: Expected `Iterable[Unknown]`, found `@Todo | ((() -> Sequence[int | None]) & ~((...) -> object))`
- jax/_src/interpreters/batching.py:1010:39: error[invalid-argument-type] Argument to function `safe_zip` is incorrect: Expected `Iterable[Unknown]`, found `@Todo | ((() -> Sequence[int | None]) & ~((...) -> object))`
- jax/_src/xla_bridge.py:532:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 2267 diagnostics
+ Found 2264 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/web.py:307:9: error[invalid-assignment] Object of type `object` is not assignable to `Application | Awaitable[Application]`
- Found 160 diagnostics
+ Found 161 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typeguard/_decorators.py:209:29: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, (@Todo & str) | (@Todo & None)].__setitem__(key: str, value: (@Todo & str) | (@Todo & None), /) -> None` cannot be called with a key of type `Literal["fset", "fget", "fdel"]` and a value of type `FunctionType` on object of type `dict[str, (@Todo & str) | (@Todo & None)]`
+ setuptools/_vendor/typeguard/_decorators.py:209:29: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, (Unknown & str) | (Unknown & None)].__setitem__(key: str, value: (Unknown & str) | (Unknown & None), /) -> None` cannot be called with a key of type `Literal["fset", "fget", "fdel"]` and a value of type `FunctionType` on object of type `dict[str, (Unknown & str) | (Unknown & None)]`
- setuptools/_vendor/typeguard/_importhook.py:211:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MetaPathFinder`, found `MetaPathFinderProtocol & type[Any]`
+ setuptools/_vendor/typeguard/_importhook.py:211:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MetaPathFinder`, found `MetaPathFinderProtocol & type`
- setuptools/_vendor/typing_extensions.py:2855:17: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `__deprecated__` on type `_T@__call__ & ((...) -> object) & ~type`
+ setuptools/_vendor/typing_extensions.py:2855:17: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `__deprecated__` on type `_T@__call__ & (() -> object) & ~type`
- setuptools/config/_apply_pyprojecttoml.py:124:31: error[invalid-argument-type] Argument to function `_set_config` is incorrect: Expected `str`, found `(Unknown & ~((...) -> object)) | (partial[Unknown] & ~((...) -> object))`
+ setuptools/config/_apply_pyprojecttoml.py:124:31: error[invalid-argument-type] Argument to function `_set_config` is incorrect: Expected `str`, found `(Unknown & ~(() -> object)) | (partial[Unknown] & ~(() -> object))`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/property/bases.py:184:20: error[invalid-return-type] Return type does not match returned value: expected `T@Property`, found `((() -> T@Property) & ~((...) -> object)) | (T@Property & ~((...) -> object))`
- src/bokeh/core/property/bases.py:187:24: error[invalid-return-type] Return type does not match returned value: expected `T@Property`, found `((() -> T@Property) & ((...) -> object)) | (T@Property & ((...) -> object))`
+ src/bokeh/core/property/bases.py:187:24: error[invalid-return-type] Return type does not match returned value: expected `T@Property`, found `(() -> T@Property) | (T@Property & (() -> object))`
- Found 467 diagnostics
+ Found 466 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/groupby/numba_.py:61:57: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ pandas/core/groupby/numba_.py:61:57: error[unresolved-attribute] Type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`
- pandas/io/parsers/base_parser.py:991:12: error[invalid-return-type] Return type does not match returned value: expected `SequenceT@evaluate_callable_usecols | set[int]`, found `(((Hashable, /) -> object) & ~((...) -> object)) | (SequenceT@evaluate_callable_usecols & ~((...) -> object))`
+ pandas/io/parsers/base_parser.py:991:12: error[invalid-return-type] Return type does not match returned value: expected `SequenceT@evaluate_callable_usecols | set[int]`, found `(((Hashable, /) -> object) & ~(() -> object)) | (SequenceT@evaluate_callable_usecols & ~(() -> object))`
- pandas/util/_decorators.py:199:41: warning[possibly-missing-attribute] Attribute `get` on type `(Mapping[Any, Any] & ~((...) -> object)) | (((Any, /) -> Any) & <Protocol with members 'get'> & ~((...) -> object)) | (((Any, /) -> Any) & ((...) -> object) & ~((...) -> object))` may be missing
- Found 3320 diagnostics
+ Found 3319 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/tests/conftest.py:9:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression
+ ibis/backends/tests/conftest.py:9:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not allowed in a type expression
- ibis/expr/types/relations.py:4433:13: warning[possibly-missing-attribute] Attribute `setdefault` on type `(((str, /) -> Value) & Top[Mapping[Unknown, object]]) | Mapping[str, (str, /) -> Value] | Unknown | dict[Unknown, ((str, /) -> Value) & ((...) -> object) & ~Top[Mapping[Unknown, object]]]` may be missing
+ ibis/expr/types/relations.py:4433:13: warning[possibly-missing-attribute] Attribute `setdefault` on type `(((str, /) -> Value) & Top[Mapping[Unknown, object]]) | Mapping[str, (str, /) -> Value] | Unknown | dict[Unknown, ((str, /) -> Value) & (() -> object) & ~Top[Mapping[Unknown, object]]]` may be missing

zulip (https://github.com/zulip/zulip)
- zerver/lib/rest.py:110:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[(...) -> HttpResponse, set[str]] | HttpResponse`, found `tuple[(...) -> object, Top[set[Unknown]]]`
+ zerver/lib/rest.py:110:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[(...) -> HttpResponse, set[str]] | HttpResponse`, found `tuple[() -> object, Top[set[Unknown]]]`
- zerver/lib/rest.py:112:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[(...) -> HttpResponse, set[str]] | HttpResponse`, found `tuple[((...) -> object) & ~tuple[object, ...], set[Unknown]]`
+ zerver/lib/rest.py:112:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[(...) -> HttpResponse, set[str]] | HttpResponse`, found `tuple[(() -> object) & ~tuple[object, ...], set[Unknown]]`
- zerver/tests/test_openapi.py:588:38: error[not-iterable] Object of type `((...) -> Unknown) & ~((...) -> object)` is not iterable
+ zerver/tests/test_openapi.py:588:38: error[not-iterable] Object of type `((...) -> Unknown) & ~(() -> object)` is not iterable
- zerver/tests/test_openapi.py:600:58: warning[possibly-missing-attribute] Attribute `__name__` on type `(Unknown & ((...) -> object) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | (((...) -> Unknown) & ((...) -> object) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | (Unknown & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | ((...) -> Unknown)` may be missing
+ zerver/tests/test_openapi.py:600:58: warning[possibly-missing-attribute] Attribute `__name__` on type `(Unknown & (() -> object) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | (((...) -> Unknown) & (() -> object) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | (Unknown & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | ((...) -> Unknown)` may be missing

sympy (https://github.com/sympy/sympy)
- sympy/utilities/tests/test_pickling.py:72:17: warning[possibly-missing-attribute] Attribute `loads` on type `(Unknown & ModuleType & ~((...) -> object)) | (((x: _T@copy) -> _T@copy) & ModuleType & ~((...) -> object)) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ModuleType & ~((...) -> object))` may be missing
+ sympy/utilities/tests/test_pickling.py:72:17: warning[possibly-missing-attribute] Attribute `loads` on type `(Unknown & ModuleType & ~(() -> object)) | (((x: _T@copy) -> _T@copy) & ModuleType & ~(() -> object)) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ModuleType & ~(() -> object))` may be missing
- sympy/utilities/tests/test_pickling.py:72:32: warning[possibly-missing-attribute] Attribute `dumps` on type `(Unknown & ModuleType & ~((...) -> object)) | (((x: _T@copy) -> _T@copy) & ModuleType & ~((...) -> object)) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ModuleType & ~((...) -> object))` may be missing
+ sympy/utilities/tests/test_pickling.py:72:32: warning[possibly-missing-attribute] Attribute `dumps` on type `(Unknown & ModuleType & ~(() -> object)) | (((x: _T@copy) -> _T@copy) & ModuleType & ~(() -> object)) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ModuleType & ~(() -> object))` may be missing
- sympy/utilities/tests/test_pickling.py:74:46: error[invalid-argument-type] Argument to function `dumps` is incorrect: Expected `int | None`, found `(Unknown & ~((...) -> object) & ~ModuleType) | (int & ~((...) -> object)) | (((x: _T@copy) -> _T@copy) & ~((...) -> object) & ~ModuleType) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ~((...) -> object) & ~ModuleType)`
+ sympy/utilities/tests/test_pickling.py:74:46: error[invalid-argument-type] Argument to function `dumps` is incorrect: Expected `int | None`, found `(Unknown & ~(() -> object) & ~ModuleType) | (int & ~(() -> object)) | (((x: _T@copy) -> _T@copy) & ~(() -> object) & ~ModuleType) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ~(() -> object) & ~ModuleType)`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:20: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression
+ ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:20: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not allowed in a type expression

core (https://github.com/home-assistant/core)
- homeassistant/components/tomorrowio/sensor.py:349:18: error[unsupported-operator] Operator `*` is unsupported between objects of type `float` and `(((int | float, /) -> int | float) & ~((...) -> object)) | (int & ~((...) -> object)) | (float & ~((...) -> object))`
+ homeassistant/components/tomorrowio/sensor.py:349:18: error[unsupported-operator] Operator `*` is unsupported between objects of type `float` and `(((int | float, /) -> int | float) & ~(() -> object)) | (int & ~(() -> object)) | (float & ~(() -> object))`
- homeassistant/core.py:596:65: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(...) -> Unknown`, found `(((...) -> Any) & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Any] & ~Coroutine[Any, Any, Any])`
- homeassistant/core.py:661:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(...) -> Unknown`, found `(((...) -> Coroutine[Any, Any, _R@async_add_job] | _R@async_add_job) & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, _R@async_add_job] & ~Coroutine[Any, Any, Any])`
- homeassistant/core.py:988:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(...) -> Unknown`, found `(((...) -> Coroutine[Any, Any, _R@async_run_job] | _R@async_run_job) & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, _R@async_run_job] & ~Coroutine[Any, Any, Any])`
- homeassistant/data_entry_flow.py:1042:25: error[invalid-argument-type] Argument to bound method `async_show_progress` is incorrect: Expected `Mapping[str, str] | None`, found `None | @Todo | (dict[str, str] & ~((...) -> object)) | (((Any, /) -> dict[str, str]) & ~((...) -> object))`
+ homeassistant/data_entry_flow.py:1042:25: error[invalid-argument-type] Argument to bound method `async_show_progress` is incorrect: Expected `Mapping[str, str] | None`, found `None | @Todo | (dict[str, str] & ~(() -> object)) | (((Any, /) -> dict[str, str]) & ~(() -> object))`
- homeassistant/helpers/schema_config_entry_flow.py:258:13: error[invalid-assignment] Object of type `(((dict[str, Any], /) -> Coroutine[Any, Any, str | None]) & ~((...) -> object)) | (str & ~((...) -> object)) | None` is not assignable to `str | None`
+ homeassistant/helpers/schema_config_entry_flow.py:258:13: error[invalid-assignment] Object of type `(((dict[str, Any], /) -> Coroutine[Any, Any, str | None]) & ~(() -> object)) | (str & ~(() -> object)) | None` is not assignable to `str | None`
- homeassistant/helpers/service.py:807:9: error[invalid-argument-type] Argument to function `_get_permissible_entity_candidates` is incorrect: Expected `dict[str, Entity]`, found `@Todo | (dict[str, Entity] & ~((...) -> object)) | ((() -> dict[str, Entity]) & ~((...) -> object))`
- Found 13731 diagnostics
+ Found 13727 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-26 13:15_

---

_Comment by @github-actions[bot] on 2025-09-26 13:20_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 7 | 9 | 7 |
| `unresolved-attribute` | 1 | 0 | 12 |
| `possibly-missing-attribute` | 0 | 2 | 9 |
| `invalid-assignment` | 2 | 0 | 7 |
| `invalid-return-type` | 2 | 2 | 5 |
| `invalid-type-form` | 0 | 0 | 3 |
| `not-iterable` | 0 | 0 | 2 |
| `too-many-positional-arguments` | 2 | 0 | 0 |
| `unsupported-operator` | 0 | 0 | 2 |
| `no-matching-overload` | 0 | 1 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **15** | **14** | **47** |

**[Full report with detailed diff](https://alex-typeis-top.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-typeis-top.ecosystem-663.pages.dev/timing))


---

_Comment by @AlexWaygood on 2025-09-26 13:55_

# Ecosystem analysis

Overall this PR adds about as many diagnostics as it removes, but I think it makes our narrowing behaviour for `TypeIs` more consistent with our narrowing behaviour elsewhere, and therefore easier to reason about.

There are several hits that look like true positives: we're now inferring less dynamic types which is allowing us to highlight places where the code arguably is unsound (`aiohttp`, `beartype`, `prefect`, `psycopg`, `trio`). I worry that some of these will seem pedantic, overly strict, and possibly confusing to users. But it makes our `TypeIs` narrowing behaviour more consistent with our `isinstance()` narrowing behaviour, which will be especially beneficial when https://github.com/astral-sh/ruff/pull/20368 lands.

The new diagnostic on `hydra-zen` is caused by missing support for `TypeGuard`.

We have some new false positives in `pytest`, `vision` and `xarray` that are caused by this TODO here, which causes us to incorrectly think that `Top[Callable[..., Any]]` -> `Callable[[], object]` (it should really be `Callable[Top[...], object]`, I suppose?

https://github.com/astral-sh/ruff/blob/57e1ff8294d630b0f265d875caad80e32c8ee6b6/crates/ty_python_semantic/src/types/signatures.rs#L1286-L1291

We have a new false positive on `starlette` which is caused by missing support for `functools.partial`.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-26 16:05_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-26 16:05_

---

_Marked ready for review by @AlexWaygood on 2025-09-26 16:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-26 16:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-26 16:27_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-26 16:27_

---

_Comment by @carljm on 2025-09-26 16:55_

> it should really be `Callable[Top[...], object]`, I suppose?

No, it should be `Callable[Bottom[...], object]` due to contravariance of arguments.

---

_Comment by @AlexWaygood on 2025-09-26 16:56_

> > it should really be `Callable[Top[...], object]`, I suppose?
> 
> No, it should be `Callable[Bottom[...], object]` due to contravariance of arguments.

Right, yes, sorry

---

_Renamed from "Use `Top` materializations for `TypeIs` special form" to "[ty] Use `Top` materializations for `TypeIs` special form" by @AlexWaygood on 2025-09-26 17:10_

---

_@carljm reviewed on 2025-09-26 17:12_

I don't love implicitly transforming a type explicitly written by the user :/ but I do feel like this practically improves the results, given the way `TypeIs` is currently used in typeshed. Narrowing with a gradual type is just not really very useful, if it's handled precisely (as we do).

I feel like the user intent expressed by `-> TypeIs[Foo[Any]]` is "the type is some kind of `Foo`, I don't know which kind" (this is accurately modeled by taking the top materialization), but also then "treat the contained type of `Foo` as dynamic if I end up using it" -- and this latter part is what we don't do.

I wonder what it would look like if, instead of doing this, we simplified `Foo[Any] & Foo[Bar]` to `Foo[Bar & Any]`? I think that's a correct simplification? And I think that would give the same benefits as this PR, but with fewer false positives -- it would better respect the user's intent in writing `TypeIs[Foo[Any]]`. WDYT?

Edit: I guess then we'd have false negatives compared to other type checkers, who are happy to simplify `Foo[Any] & Foo[Bar]` to just `Foo[Bar]`. Which is not correct, but is maybe practically what users expect.

---

_@carljm approved on 2025-09-26 17:16_

Going ahead and approving this -- I don't _love_ it, but it seems like a net improvement for now, and it's easy to reverse if we come up with a better idea.

---

_Comment by @AlexWaygood on 2025-09-26 17:16_

> I wonder what it would look like if, instead of doing this, we simplified `Foo[Any] & Foo[Bar]` to `Foo[Bar & Any]`? I think that's a correct simplification?

That's only a correct simplification if `Foo` is covariant, I think? I don't think it's true, for example, that `list[Any] & list[int]` can be simplified to `list[int & Any]`

---

_@AlexWaygood reviewed on 2025-09-26 17:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:1297 on 2025-09-26 17:19_

```suggestion
                    // N.B. Using the top materialization here is a pragmatic decision
                    // that makes us produce more intuitive results given how
                    // `TypeIs` is used in the real world (in particular, in typeshed).
                    // However, there's some debate about whether this is really
                    // fully correct. See <https://github.com/astral-sh/ruff/pull/20591>
                    // for more discussion.
                    self.infer_type_expression(arguments_slice)
```

---

_Merged by @AlexWaygood on 2025-09-26 17:24_

---

_Closed by @AlexWaygood on 2025-09-26 17:24_

---

_Branch deleted on 2025-09-26 17:24_

---
