```yaml
number: 20875
title: "[ty] Avoid unnecessarily widening generic specializations"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/generic-call-inference
created_at: 2025-10-15T00:23:12Z
updated_at: 2025-10-16T19:17:40Z
url: https://github.com/astral-sh/ruff/pull/20875
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Avoid unnecessarily widening generic specializations

---

_Pull request opened by @ibraheemdev on 2025-10-15 00:23_

## Summary

Ignore the type context when specializing a generic call if it leads to an unnecessarily wide return type. For example, [the example mentioned here](https://github.com/astral-sh/ruff/pull/20796#issuecomment-3403319536) works as expected after this change:
```py
def id[T](x: T) -> T:
    return x

def _(i: int):
    x: int | None = id(i)
    y: int | None = i
    reveal_type(x)  # revealed: int
    reveal_type(y)  # revealed: int
```

I also added extended our usage of `filter_disjoint_elements` to tuple and typed-dict inference, which resolves https://github.com/astral-sh/ty/issues/1266.

---

_Label `ty` added by @ibraheemdev on 2025-10-15 00:23_

---

_Review requested from @carljm by @ibraheemdev on 2025-10-15 00:23_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-15 00:23_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-15 00:23_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-15 00:23_

---

_Comment by @github-actions[bot] on 2025-10-15 00:25_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-16 19:14:50.885037218 +0000
+++ new-output.txt	2025-10-16 19:14:54.231077442 +0000
@@ -266,8 +266,8 @@
 dataclasses_transform_class.py:119:18: error[unknown-argument] Argument `id` does not match any known parameter of bound method `__init__`
 dataclasses_transform_class.py:119:24: error[unknown-argument] Argument `name` does not match any known parameter of bound method `__init__`
 dataclasses_transform_converter.py:25:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@model_field`
-dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> int`, found `def bad_converter1() -> int`
-dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> int`, found `def bad_converter2(*, x: int) -> int`
+dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter1() -> int`
+dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter2(*, x: int) -> int`
 dataclasses_transform_converter.py:107:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
 dataclasses_transform_converter.py:108:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
 dataclasses_transform_converter.py:109:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
@@ -277,7 +277,7 @@
 dataclasses_transform_converter.py:116:1: error[invalid-assignment] Object of type `Literal[b"f6"]` is not assignable to attribute `field3` of type `ConverterClass`
 dataclasses_transform_converter.py:119:1: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `field3` of type `ConverterClass`
 dataclasses_transform_converter.py:121:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 7
-dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> int`, found `def converter_simple(s: str) -> int`
+dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo(unsupported type[X] special form)`
 dataclasses_transform_field.py:64:16: error[unknown-argument] Argument `id` does not match any known parameter
 dataclasses_transform_field.py:75:1: error[missing-argument] No argument provided for required parameter `name`
@@ -301,7 +301,6 @@
 dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
 dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
 dataclasses_usage.py:83:13: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_usage.py:88:14: error[no-matching-overload] No overload of function `field` matches arguments
 dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_usage.py:130:1: error[missing-argument] No argument provided for required parameter `y` of bound method `__init__`
 dataclasses_usage.py:179:6: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
@@ -901,5 +900,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 903 diagnostics
+Found 902 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-15 00:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/local.py:511:24: error[invalid-return-type] Return type does not match returned value: expected `T@LocalProxy`, found `T@LocalProxy | ~None | Unknown`
+ src/werkzeug/local.py:511:24: error[invalid-return-type] Return type does not match returned value: expected `T@LocalProxy`, found `~None | T@LocalProxy | Unknown`
- src/werkzeug/routing/exceptions.py:107:20: error[no-matching-overload] No overload of function `max` matches arguments
- Found 384 diagnostics
+ Found 383 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:3416:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `InstanceConfigDict | (Unknown & ~None) | dict[str, Any]`
+ paasta_tools/utils.py:3416:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `(Unknown & ~None) | dict[str, Any] | InstanceConfigDict`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/python.py:1064:39: error[no-matching-overload] No overload of function `field` matches arguments
- Found 489 diagnostics
+ Found 488 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/request.py:25:33: error[no-matching-overload] No overload of function `field` matches arguments
- boostedblob/request.py:29:34: error[no-matching-overload] No overload of function `field` matches arguments
- boostedblob/request.py:76:33: error[no-matching-overload] No overload of function `field` matches arguments
- boostedblob/request.py:80:34: error[no-matching-overload] No overload of function `field` matches arguments
- boostedblob/request.py:85:51: error[no-matching-overload] No overload of function `field` matches arguments
- Found 76 diagnostics
+ Found 71 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[bytes | str]`
+ pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[str | bytes]`

poetry (https://github.com/python-poetry/poetry)
+ tests/console/commands/test_show.py:42:12: warning[redundant-cast] Value is already of type `F@output_format_parametrize`
- Found 990 diagnostics
+ Found 991 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some[_T1](value: _T1@Some) -> Option[_T1@Some], (Any, /) -> Option[Any], /) -> def Some[_T1](value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
+ expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any], (Any, /) -> Result[Any, Any], /) -> def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
+ tests/test_array.py:307:38: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
+ tests/test_block.py:274:38: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
+ tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0], Any]`
+ tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error"]]`
- Found 207 diagnostics
+ Found 213 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[no-matching-overload] No overload of bound method `pipe` matches arguments
- tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[SchemaOut] | DataFrame[Schema]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema] | DataFrame[SchemaOut]`
- Found 1598 diagnostics
+ Found 1597 diagnostics

mypy (https://github.com/python/mypy)
- mypy/type_visitor.py:267:13: warning[redundant-cast] Value is already of type `Any`
- mypy/type_visitor.py:282:13: warning[redundant-cast] Value is already of type `Any`
- Found 1841 diagnostics
+ Found 1839 diagnostics

pylox (https://github.com/sco1/pylox)
- pylox/builtins/py_builtins.py:175:32: error[invalid-argument-type] Argument to function `mean` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
- pylox/builtins/py_builtins.py:192:34: error[invalid-argument-type] Argument to function `median` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
- pylox/builtins/py_builtins.py:216:32: error[invalid-argument-type] Argument to function `mode` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
- pylox/builtins/py_builtins.py:326:33: error[invalid-argument-type] Argument to function `stdev` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
- Found 26 diagnostics
+ Found 22 diagnostics

meson (https://github.com/mesonbuild/meson)
+ unittests/cargotests.py:337:68: error[invalid-key] Invalid key access on TypedDict `FromWorkspace`: Unknown key "optional"
+ unittests/cargotests.py:345:62: error[invalid-key] Invalid key access on TypedDict `FromWorkspace`: Unknown key "features"
- Found 885 diagnostics
+ Found 887 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/domains/python/__init__.py:672:42: error[index-out-of-bounds] Index 0 is out of bounds for string `Literal[""]` with length 0
- Found 493 diagnostics
+ Found 494 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_highlevel_open_unix_stream.py:63:28: error[no-matching-overload] No overload of function `fspath` matches arguments
- Found 613 diagnostics
+ Found 612 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/api.py:1701:26: warning[redundant-cast] Value is already of type `str`
- xarray/backends/scipy_.py:326:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | ReadBuffer[Unknown] | AbstractDataStore | ... omitted 3 union elements`
+ xarray/backends/scipy_.py:326:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | PathLike[Any] | ReadBuffer[Unknown] | ... omitted 3 union elements`
- Found 1615 diagnostics
+ Found 1614 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/tasks.py:1673:13: error[invalid-argument-type] Argument to function `emit_task_run_state_change_event` is incorrect: Expected `State[Any]`, found `State[Any] | None`
- Found 3148 diagnostics
+ Found 3147 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/calibration.py:1093:17: error[invalid-key] Invalid key access on TypedDict `_MinimizeScalarOptionsBracketed`: Unknown key "xatol" - did you mean "xtol"?
- Found 1993 diagnostics
+ Found 1994 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/object_type.py:90:9: error[invalid-argument-type] Argument to function `type` is incorrect: Expected `T@_impl_type`, found `T@_impl_type | None`
- strawberry/types/base.py:272:63: error[no-matching-overload] No overload of function `field` matches arguments
- Found 382 diagnostics
+ Found 380 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/wrapper/_implementations.py:945:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo] | ListConfig | DictConfig`, found `@Todo | DataClass_ | type[@Todo] | ... omitted 6 union elements`
+ src/hydra_zen/wrapper/_implementations.py:945:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo] | ListConfig | DictConfig`, found `@Todo | (((...) -> Any) & Top[dict[Unknown, Unknown]]) | DataClass_ | ... omitted 6 union elements`
- tests/annotations/declarations.py:1241:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1246:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1249:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 557 diagnostics
+ Found 554 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- src/scikit_build_core/settings/skbuild_docs_sphinx.py:27:36: error[no-matching-overload] No overload of function `field` matches arguments
- src/scikit_build_core/settings/skbuild_docs_sphinx.py:43:30: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 49 diagnostics

altair (https://github.com/vega/altair)
+ altair/utils/core.py:356:10: warning[redundant-cast] Value is already of type `_PandasDataFrameT@sanitize_pandas_dataframe`
- Found 1339 diagnostics
+ Found 1340 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/hydpytools.py:2631:61: error[unresolved-attribute] Type `object` has no attribute `name`
- Found 609 diagnostics
+ Found 608 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/arithmetic/timedeltaindex/test_mul.py:20:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `TimedeltaIndex | Index[Any]`
+ tests/indexes/arithmetic/timedeltaindex/test_mul.py:20:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `Index[Any] | TimedeltaIndex`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:440:29: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(T@tree_reduce & ~Unspecified, Unknown, /) -> T@tree_reduce & ~Unspecified`, found `(T@tree_reduce, Any, /) -> T@tree_reduce`
- Found 2435 diagnostics
+ Found 2436 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/polys/densetools.py:292:36: error[invalid-argument-type] Argument to function `dup_TC` is incorrect: Expected `Domain[Es@convert | Expr | int | ... omitted 3 union elements]`, found `Domain[Er@dup_eval]`
- Found 11058 diagnostics
+ Found 11057 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/assets/asset.py:172:16: error[invalid-return-type] Return type does not match returned value: expected `AssetWithSymbol`, found `AssetWithNameAndType`
- rotkehlchen/assets/asset.py:184:16: error[invalid-return-type] Return type does not match returned value: expected `EvmToken`, found `CryptoAsset`
- rotkehlchen/assets/asset.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `SolanaToken`, found `CryptoAsset`
- rotkehlchen/assets/asset.py:196:16: error[invalid-return-type] Return type does not match returned value: expected `AssetWithOracles`, found `AssetWithNameAndType`
- Found 1717 diagnostics
+ Found 1713 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/helpers/service_info/ssdp.py:39:39: error[no-matching-overload] No overload of function `field` matches arguments
- Found 13761 diagnostics
+ Found 13760 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~31MB
+     struct metadata = ~30MB
-     struct fields = ~36MB
+     struct fields = ~35MB
-     memo metadata = ~103MB
+     memo metadata = ~98MB

```
</details>


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-15 00:28_

---

_@ibraheemdev reviewed on 2025-10-15 00:29_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:425 on 2025-10-15 00:29_

The churn here could be avoided if we performed a *third* specialization, inferring the call expression annotation first before the argument types, but I'm not sure it's worth it.

---

_Comment by @github-actions[bot] on 2025-10-15 00:33_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 7 | 7 | 0 |
| `no-matching-overload` | 0 | 14 | 0 |
| `invalid-return-type` | 0 | 4 | 7 |
| `redundant-cast` | 2 | 3 | 0 |
| `invalid-key` | 3 | 0 | 0 |
| `unused-ignore-comment` | 0 | 3 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| `index-out-of-bounds` | 1 | 0 | 0 |
| **Total** | **13** | **33** | **7** |

**[Full report with detailed diff](https://ibraheem-generic-call-infere.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-generic-call-infere.ecosystem-663.pages.dev/timing))


---

_Comment by @ibraheemdev on 2025-10-15 00:59_

The ecosystem results look good. There are some regressions around `functools.reduce` because we aren't able to correctly infer the type variables in a `Callable`, so we choose the overly narrow type without widening based on the type context. However, those should be fixed by the new trait solver, we shouldn't need the type context for that to work.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2563 on 2025-10-15 12:34_

You can use `Type::is_type_var` here. Or alternatively, do we want to update this to skip the type context if it contains a typevar anywhere inside of it? In that case you'd use `Type::has_typevar`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2552 on 2025-10-15 12:36_

Should this use `TypeContext::default`? The comment says "without the type context"

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1200 on 2025-10-15 12:40_

nit: I would call this `filter_union`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1199 on 2025-10-15 12:40_

```suggestion
    /// If this type is a union, filters the union elements based on the provided predicate.
    /// Otherwise returns the type unchanged.
```

(probably needs to be reflowed to satisfy pre-commit)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:11203 on 2025-10-15 12:43_

Can we make the callback take in `Type<'db>` instead of `&Type<'db>`? I find that much more ergonomic, and I think it just requires a `.copied()` before the `filter`. (Ditto above in `Type::filter`)

---

_@dcreager reviewed on 2025-10-15 12:44_

---

_@AlexWaygood reviewed on 2025-10-15 12:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11203 on 2025-10-15 12:53_

Ibraheem's current signature matches the signature for `UnionType::map()`. I don't have a strong objection to changing _that_ signature, but I think it's nice to keep the signature of `map()` consistent with the signature of `filter()`. (And IIRC, I made the callback passed to `UnionType::map()` take `&Type<'db>` because it seemed generally to make most callsites more ergonomic than if it took `Type<'db>`. Due to the fact that `some_union.elements(db).iter()` calls return `Iterator<Item = &Type<'db>>`s rather than `Iterator<Item = Type<'db>>`s)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:11203 on 2025-10-15 12:57_

:+1: I agree with keeping them consistent, but feel strongly enough about avoiding `&Type` when we can that I might tackle that as a separate follow on. You know, in my copious spare time.

---

_@dcreager reviewed on 2025-10-15 12:57_

---

_@ibraheemdev reviewed on 2025-10-15 21:05_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/call/bind.rs`:2563 on 2025-10-15 21:05_

I'll use `is_type_var` for now. We still want to use the type context as much as possible, we just want to avoid ever adding a type-mapping to a typevar, e.g. `dict[TypedDict(..), T]` is still useful. I'll address this in a follow up PR.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:2501 on 2025-10-15 21:06_

```suggestion
    #[expect(clippy::too_many_arguments)]
```

---

_@AlexWaygood reviewed on 2025-10-15 21:06_

---

_@ibraheemdev reviewed on 2025-10-15 21:06_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/call/bind.rs`:2552 on 2025-10-15 21:06_

I updated the comment, this is specifically talking about inferring the return type using the type context. We still need the type context to avoid eager literal promotion (regardless of whether or not we choose the inference with or without the type context).

---

_@ibraheemdev reviewed on 2025-10-15 21:07_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:11203 on 2025-10-15 21:07_

There are some `Type` methods that take `&self`, so we should probably address them all at once. Only changing `filter` would mean `.filter(Type::is_typed_dict)` no longer works, for example.

---

_@AlexWaygood reviewed on 2025-10-15 21:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11203 on 2025-10-15 21:09_

I think the last time we discussed it, we decided that it was pretty borderline but that it was _probably_ slightly more efficient for methods like that to take `&self` rather than `self`, despite the fact that `Type` is `Copy`, given how big `Type` is

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-15 23:38_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-15 23:39_

---

_Comment by @ibraheemdev on 2025-10-16 00:31_

I updated the heuristic to ignore the type context if the specialization is already assignable to the type context. This seems more consistent than the previous subtyping check, and also means we don't have to perform a second specialization if the first one is valid.

This is somewhat related to https://github.com/astral-sh/ty/issues/136, but I think that is a decision we have to make separately and more related to preferring the declared type itself, not the inference that uses the declared type. For example, in https://github.com/astral-sh/ruff/pull/20796, we want re-assignments to be narrowed while still accounting for the type context. This seems to remove all the false positives that showed up in the ecosystem report as https://github.com/astral-sh/ruff/pull/20796, and removes a number of existing false positives as well.

The regression with `functools.reduce` remains as mentioned before, and there seems to also be a regression with static method constructors, but I believe in that case we aren't able to perform literal promotion due to an unrelated issue.

---

_@ibraheemdev reviewed on 2025-10-16 00:54_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/call/bind.rs`:2563 on 2025-10-16 00:54_

Actually, I'm not even sure we need this check anymore, given we'll prefer the first inference over the one unioned with a typevar. I suppose it might make for better diagnostics for invalid assignments?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:425 on 2025-10-16 18:32_

There is a TODO below indicating where we'd make the change if we wanted to fix this, so I'm 👍 to this. Though maybe add a TODO comment here, too, calling out that we realize that we reordered the union, and that we know how we'd fix it?

---

_@dcreager approved on 2025-10-16 18:34_

---

_Merged by @ibraheemdev on 2025-10-16 19:17_

---

_Closed by @ibraheemdev on 2025-10-16 19:17_

---

_Branch deleted on 2025-10-16 19:17_

---
