```yaml
number: 22537
title: "[ty] Support 'dangling' `type(...)` constructors"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: charlie/dyn
head: charlie/dyn-expression
created_at: 2026-01-12T18:12:25Z
updated_at: 2026-01-12T20:08:49Z
url: https://github.com/astral-sh/ruff/pull/22537
synced_at: 2026-01-12T20:26:27Z
```

# [ty] Support 'dangling' `type(...)` constructors

---

_@charliermarsh_

## Summary

This PR adds support for 'dangling' `type(...)` constructors, e.g.:

```python
class Foo(type("Bar", ...)):
   ...
```

As opposed to:

```python
Bar = type("Bar", ...)
```

The former doesn't have a `Definition` since it doesn't get bound to a place, so we instead need to store the `NodeIndex`. Per @MichaReiser's suggestion, we can use a Salsa tracked struct for this.


---

_Comment by @astral-sh-bot[bot] on 2026-01-12 18:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-12 18:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
+ lib/spack/spack/llnl/util/lang.py:693:50: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__init__]`
- Found 4318 diagnostics
+ Found 4319 diagnostics

black (https://github.com/psf/black)
+ src/black/linegen.py:775:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:785:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:794:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:796:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- Found 54 diagnostics
+ Found 58 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/deserialization.py:137:34: error[invalid-assignment] Object of type `type` is not assignable to `type[SafeLoader]`
+ src/schemathesis/core/deserialization.py:137:54: warning[unsupported-dynamic-base] Unsupported class base: Has type `<class 'CSafeLoader'> | <class 'SafeLoader'>`
+ src/schemathesis/core/deserialization.py:174:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/schemathesis/specs/openapi/stateful/__init__.py:206:12: error[invalid-return-type] Return type does not match returned value: expected `type[APIStateMachine]`, found `type`

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/mappings.py:111:49: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- src/arti/producers/__init__.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `type[Producer]`, found `type`
- src/arti/types/__init__.py:339:13: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
+ src/arti/types/__init__.py:341:18: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@generate]`
- src/arti/types/bigquery.py:66:9: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
- src/arti/types/pyarrow.py:42:9: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
- Found 149 diagnostics
+ Found 147 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/types/array.py:350:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseListDumper]`, found `type`
- psycopg/psycopg/types/array.py:356:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseListDumper]`, found `type`
- psycopg/psycopg/types/composite.py:525:12: error[invalid-return-type] Return type does not match returned value: expected `type[_CompositeLoader[T@_make_loader]]`, found `type`
- psycopg/psycopg/types/composite.py:534:12: error[invalid-return-type] Return type does not match returned value: expected `type[_CompositeBinaryLoader[T@_make_binary_loader]]`, found `type`
- psycopg/psycopg/types/composite.py:543:12: error[invalid-return-type] Return type does not match returned value: expected `type[_SequenceDumper[T@_make_dumper]]`, found `type`
- psycopg/psycopg/types/composite.py:552:12: error[invalid-return-type] Return type does not match returned value: expected `type[_SequenceBinaryDumper[T@_make_binary_dumper]]`, found `type`
- psycopg/psycopg/types/enum.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumLoader[E@_make_loader]]`, found `type`
- psycopg/psycopg/types/enum.py:191:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumLoader[E@_make_binary_loader]]`, found `type`
- psycopg/psycopg/types/enum.py:199:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumDumper[E@_make_dumper]]`, found `type`
- psycopg/psycopg/types/enum.py:207:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumDumper[E@_make_binary_dumper]]`, found `type`
- psycopg/psycopg/types/multirange.py:407:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeLoader[Any]]`, found `type`
- psycopg/psycopg/types/multirange.py:412:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeBinaryLoader[Any]]`, found `type`
- psycopg/psycopg/types/range.py:586:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeLoader[Any]]`, found `type`
- psycopg/psycopg/types/range.py:591:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeBinaryLoader[Any]]`, found `type`
- Found 666 diagnostics
+ Found 652 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/dataframe/model.py:257:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
+ pandera/api/dataframe/model.py:257:42: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[BaseConfig]`
- pandera/api/dataframe/model.py:455:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[type[BaseConfig], dict[str, Any]]`, found `tuple[type, dict[str, Any]]`
+ pandera/api/dataframe/model.py:455:32: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[BaseConfig]`
- pandera/api/pyspark/model.py:162:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
- Found 1580 diagnostics
+ Found 1579 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/config.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseConfig]`, found `type`
- pydantic/v1/networks.py:572:12: error[invalid-return-type] Return type does not match returned value: expected `type[AnyUrl]`, found `type`
+ pydantic/v1/schema.py:1094:50: warning[unsupported-dynamic-base] Unsupported class base: Has type `(Any & type[SecretStr]) | (Any & type[SecretBytes])`
- pydantic/v1/types.py:239:12: error[invalid-return-type] Return type does not match returned value: expected `type[int]`, found `type`
- pydantic/v1/types.py:318:12: error[invalid-return-type] Return type does not match returned value: expected `type[int] | type[float]`, found `type`
- pydantic/v1/types.py:391:12: error[invalid-return-type] Return type does not match returned value: expected `type[bytes]`, found `type`
- pydantic/v1/types.py:471:12: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `type`
- pydantic/v1/types.py:749:12: error[invalid-return-type] Return type does not match returned value: expected `type[Decimal]`, found `type`
- pydantic/v1/types.py:833:16: error[invalid-return-type] Return type does not match returned value: expected `type[JsonWrapper]`, found `type`
- pydantic/v1/types.py:1205:12: error[invalid-return-type] Return type does not match returned value: expected `type[date]`, found `type`
- Found 3159 diagnostics
+ Found 3151 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_util.py:236:5: error[unresolved-attribute] Unresolved attribute `recursion` on type `type`.
+ src/trio/_tests/test_util.py:236:5: error[unresolved-attribute] Unresolved attribute `recursion` on type `<class 'SomeClass'>`.

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:101:28: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
+ mkdocs/plugins.py:75:28: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- Found 224 diagnostics
+ Found 226 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/compilers/C/msvc.py:583:13: error[unresolved-attribute] Unresolved attribute `value` on type `Bag`.
- Found 1265 diagnostics
+ Found 1266 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/tools/merge_types.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `type`, found `<decorator produced by dataclass-like function>`
- Found 358 diagnostics
+ Found 357 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/input/run_input.py:332:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5374 diagnostics
+ Found 5368 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/core/modeltools.py:3252:61: warning[unsupported-dynamic-base] Unsupported class base: Has type `<class 'InletSequences'> | <class 'ObserverSequences'> | <class 'ReceiverSequences'> | ... omitted 8 union elements`
- Found 665 diagnostics
+ Found 666 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/partial_eval.py:1710:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/interpreters/partial_eval.py:1726:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2805 diagnostics
+ Found 2803 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/expr/operations/udf.py:155:16: error[invalid-return-type] Return type does not match returned value: expected `type[S@_make_node]`, found `<class '<unknown>'>`
- ibis/expr/operations/udf.py:155:50: error[invalid-argument-type] Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[property]`
+ ibis/expr/operations/udf.py:155:51: error[invalid-base] Invalid class base with type `property`
- Found 4607 diagnostics
+ Found 4608 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] Support 'dangling' type(...) constructors" to "[ty] Support 'dangling' `type(...)` constructors" by @charliermarsh on 2026-01-12 19:21_

---

_Label `ty` added by @charliermarsh on 2026-01-12 19:21_

---

_Comment by @codspeed-hq[bot] on 2026-01-12 19:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **improve performance by 4.88%**




`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn-expression?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 10.6 s | 10.1 s | +4.88% |
---

<sub>Comparing <code>charlie/dyn-expression</code> (598d059) with <code>charlie/dyn</code> (5a177bb)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn-expression?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn-expression?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---
