```yaml
number: 22291
title: "[ty] Add support for dynamic `type()` classes"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/dyn
created_at: 2025-12-29T21:45:15Z
updated_at: 2026-01-11T15:02:25Z
url: https://github.com/astral-sh/ruff/pull/22291
synced_at: 2026-01-12T02:12:03Z
```

# [ty] Add support for dynamic `type()` classes

---

_Pull request opened by @charliermarsh on 2025-12-29 21:45_

## Summary

This PR adds support for dynamic classes created via `type()`. The core of the change is that `ClassLiteral` is now an enum:

```rust
pub enum ClassLiteral<'db> {
    /// A class defined via a `class` statement.
    Stmt(StmtClassLiteral<'db>),
    /// A class created via the functional form `type(name, bases, dict)`.
    Functional(FunctionalClassLiteral<'db>),
}
```

And, in turn, various methods on `ClassLiteral` like `body_scope` now return `Option` or similar (and callers must adjust to that change in signature).

Over time, we can expand the enum to include functional namedtuples, etc. (I already have this working in a separate branch, and I believe it slots in well.)

(I'd love help with the names -- I think `StmtClassLiteral` is kind of lame. Maybe `DeclarativeClassLiteral`?)

Closes https://github.com/astral-sh/ty/issues/740.


---

_Label `ty` added by @charliermarsh on 2025-12-29 21:45_

---

_Comment by @charliermarsh on 2025-12-29 21:45_

(Not ready for review.)

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 21:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 21:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
+ bidict/_base.py:147:47: warning[unsupported-base] Unsupported class base: Has type `type[BT@_make_inv_cls]`
- Found 15 diagnostics
+ Found 16 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/builder.py:134:17: warning[unsupported-base] Unsupported class base: Has type `type[Self@__init__]`
+ lib/spack/spack/llnl/util/lang.py:693:50: warning[unsupported-base] Unsupported class base: Has type `type[Self@__init__]`
- lib/spack/spack/test/packages.py:417:9: error[unresolved-attribute] Object of type `type` has no attribute `name`
- Found 4318 diagnostics
+ Found 4319 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/test.py:815:32: error[invalid-assignment] Object of type `type` is not assignable to `type[Response] | None`
+ src/werkzeug/test.py:817:32: warning[unsupported-base] Unsupported class base: Has type `type[Response] & ~type[TestResponse]`

black (https://github.com/psf/black)
+ src/black/linegen.py:775:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:785:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:794:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:796:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- Found 54 diagnostics
+ Found 58 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/error.py:78:13: error[invalid-assignment] Invalid subscript assignment with key of type `int` and value of type `type` on object of type `dict[int, type[Error]]`
- src/_pytest/_py/error.py:79:20: error[invalid-return-type] Return type does not match returned value: expected `type[Error]`, found `type`
- Found 425 diagnostics
+ Found 423 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ tests/test_runserver_logs.py:17:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aiohttp_std'>.Logger`
+ tests/test_runserver_logs.py:39:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aiohttp_debugtoolbar'>.Logger`
+ tests/test_runserver_logs.py:61:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aux_logger'>.Logger`
+ tests/test_runserver_logs.py:84:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_aux_logger_livereload'>.Logger`
+ tests/test_runserver_logs.py:99:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `logging.Logger`, found `tests.test_runserver_logs.<locals of function 'test_extra'>.Logger`
- Found 30 diagnostics
+ Found 35 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/deserialization.py:137:34: error[invalid-assignment] Object of type `type` is not assignable to `type[SafeLoader]`
+ src/schemathesis/core/deserialization.py:137:54: warning[unsupported-base] Unsupported class base: Has type `<class 'CSafeLoader'> | <class 'SafeLoader'>`
+ src/schemathesis/core/deserialization.py:174:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/schemathesis/specs/openapi/stateful/__init__.py:206:12: error[invalid-return-type] Return type does not match returned value: expected `type[APIStateMachine]`, found `type`

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/dataframe/model.py:257:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
+ pandera/api/dataframe/model.py:257:42: warning[unsupported-base] Unsupported class base: Has type `type[BaseConfig]`
+ pandera/api/dataframe/model.py:306:55: warning[unsupported-base] Unsupported class base: Has type `type[Self@__class_getitem__] & <Protocol with members '__parameters__'>`
- pandera/api/dataframe/model.py:308:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `type`
+ pandera/api/dataframe/model.py:308:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `<class '<unknown>'>`
- pandera/api/dataframe/model.py:455:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[type[BaseConfig], dict[str, Any]]`, found `tuple[type, dict[str, Any]]`
+ pandera/api/dataframe/model.py:455:32: warning[unsupported-base] Unsupported class base: Has type `type[BaseConfig]`
- pandera/api/pyspark/model.py:162:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
+ pandera/api/pyspark/model.py:211:55: warning[unsupported-base] Unsupported class base: Has type `type[Self@__class_getitem__] & <Protocol with members '__parameters__'>`
- pandera/api/pyspark/model.py:213:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `type`
+ pandera/api/pyspark/model.py:213:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@__class_getitem__]`, found `<class '<unknown>'>`
- Found 1578 diagnostics
+ Found 1579 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

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
- pydantic/v1/config.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseConfig]`, found `type`
- pydantic/v1/networks.py:572:12: error[invalid-return-type] Return type does not match returned value: expected `type[AnyUrl]`, found `type`
+ pydantic/v1/schema.py:1094:50: warning[unsupported-base] Unsupported class base: Has type `(Any & type[SecretStr]) | (Any & type[SecretBytes])`
- pydantic/v1/types.py:239:12: error[invalid-return-type] Return type does not match returned value: expected `type[int]`, found `type`
- pydantic/v1/types.py:318:12: error[invalid-return-type] Return type does not match returned value: expected `type[int] | type[float]`, found `type`
- pydantic/v1/types.py:391:12: error[invalid-return-type] Return type does not match returned value: expected `type[bytes]`, found `type`
- pydantic/v1/types.py:471:12: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `type`
- pydantic/v1/types.py:749:12: error[invalid-return-type] Return type does not match returned value: expected `type[Decimal]`, found `type`
- pydantic/v1/types.py:833:16: error[invalid-return-type] Return type does not match returned value: expected `type[JsonWrapper]`, found `type`
- pydantic/v1/types.py:1205:12: error[invalid-return-type] Return type does not match returned value: expected `type[date]`, found `type`
- Found 3159 diagnostics
+ Found 3151 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/mappings.py:111:49: warning[unsupported-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- src/arti/producers/__init__.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `type[Producer]`, found `type`
- src/arti/types/__init__.py:339:13: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
+ src/arti/types/__init__.py:341:18: warning[unsupported-base] Unsupported class base: Has type `type[Self@generate]`
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
+ psycopg/psycopg/types/json.py:140:26: warning[unsupported-base] Unsupported class base: Has type `type[Loader]`
- psycopg/psycopg/types/multirange.py:407:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeLoader[Any]]`, found `type`
- psycopg/psycopg/types/multirange.py:412:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeBinaryLoader[Any]]`, found `type`
- psycopg/psycopg/types/range.py:586:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeLoader[Any]]`, found `type`
- psycopg/psycopg/types/range.py:591:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeBinaryLoader[Any]]`, found `type`
- Found 665 diagnostics
+ Found 652 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:101:28: warning[unsupported-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
+ mkdocs/plugins.py:75:28: warning[unsupported-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- Found 224 diagnostics
+ Found 226 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_util.py:236:5: error[unresolved-attribute] Unresolved attribute `recursion` on type `type`.
+ src/trio/_tests/test_util.py:236:5: error[unresolved-attribute] Unresolved attribute `recursion` on type `<class 'SomeClass'>`.

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/compilers/C/msvc.py:583:13: error[unresolved-attribute] Unresolved attribute `value` on type `Bag`.
+ setuptools/tests/test_editable_install.py:1206:53: error[unresolved-attribute] Object of type `SimulatedErr` has no attribute `__notes__`
- setuptools/tests/test_editable_install.py:1204:24: error[invalid-argument-type] Argument to function `raises` is incorrect: Argument type `object` does not satisfy upper bound `BaseException` of type variable `E`
- setuptools/tests/test_editable_install.py:1204:24: error[invalid-argument-type] Argument to function `raises` is incorrect: Expected `type[BaseException] | tuple[type[BaseException], ...]`, found `type`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/input/run_input.py:332:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5368 diagnostics
+ Found 5367 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/tools/merge_types.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `type`, found `<decorator produced by dataclass-like function>`
+ strawberry/types/base.py:341:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/partial_eval.py:1710:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/interpreters/partial_eval.py:1726:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2804 diagnostics
+ Found 2802 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/examples/__init__.py:122:9: error[unresolved-attribute] Object of type `type` has no attribute `fetch`
+ ibis/expr/operations/udf.py:155:16: error[invalid-return-type] Return type does not match returned value: expected `type[S@_make_node]`, found `<class '<unknown>'>`
- ibis/expr/operations/udf.py:155:50: error[invalid-argument-type] Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[property]`
+ ibis/expr/operations/udf.py:155:51: error[invalid-base] Invalid class base with type `property`

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/core/modeltools.py:3252:61: warning[unsupported-base] Unsupported class base: Has type `<class 'InletSequences'> | <class 'ObserverSequences'> | <class 'ReceiverSequences'> | ... omitted 8 union elements`
+ hydpy/core/testtools.py:1376:47: warning[unsupported-base] Unsupported class base: Has type `type[T@make_abc_testable]`
+ hydpy/core/testtools.py:1377:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- hydpy/core/testtools.py:1378:12: error[invalid-return-type] Return type does not match returned value: expected `type[T@make_abc_testable]`, found `type`
+ hydpy/core/testtools.py:1378:12: error[invalid-return-type] Return type does not match returned value: expected `type[T@make_abc_testable]`, found `<class '<unknown>'>`
- Found 663 diagnostics
+ Found 666 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-29 22:56_

I think the scrapy, black, and aiohttp-devtools changes are true positives.

---

_Comment by @codspeed-hq[bot] on 2025-12-29 22:59_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **not alter performance**




`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



---

<sub>Comparing <code>charlie/dyn</code> (7729353) with <code>main</code> (2c68057)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @carljm by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-29 23:35_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-29 23:35_

---

_Converted to draft by @charliermarsh on 2025-12-29 23:36_

---

_@charliermarsh reviewed on 2025-12-29 23:49_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/functional_class.rs`:54 on 2025-12-29 23:49_

(Need to remove this.)

---

_Marked ready for review by @charliermarsh on 2025-12-30 03:24_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2025-12-30 03:24_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 03:30_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 2 | 28 | 9 |
| `unsupported-base` | 17 | 0 | 0 |
| `invalid-argument-type` | 5 | 7 | 0 |
| `invalid-assignment` | 4 | 5 | 0 |
| `unresolved-attribute` | 2 | 2 | 3 |
| `unused-ignore-comment` | 4 | 3 | 0 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `invalid-base` | 1 | 0 | 0 |
| **Total** | **35** | **50** | **13** |


**[Full report with detailed diff](https://9be7a62a.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://9be7a62a.ty-ecosystem-ext.pages.dev/timing))



---

_Converted to draft by @charliermarsh on 2025-12-30 16:28_

---

_Marked ready for review by @charliermarsh on 2025-12-30 17:12_

---

_Comment by @AlexWaygood on 2025-12-30 17:24_

I haven't looked too hard at this yet, but is adding a new `Type` variant definitely the best way of doing this? We've talked in the past about making `ClassLiteral` (the wrapped data inside the `Type::ClassLiteral` variant) an enum. In almost all respects, types created using three-argument `type()` calls should be treated the same way as classes created using `class` statements (the only difference is that its definition in the AST will be an `ExprCall` rather than a `StmtClassDef`), and that design would better reflect that, I think.

We should also try to make our design here extensible for other classes-created-by-function-calls: functional namedtuples, functional typeddicts, and functional enums.

---

_Comment by @AlexWaygood on 2025-12-30 17:28_

For a previous attempt to make `ClassLiteral` an enum, you can see https://github.com/astral-sh/ruff/pull/19998. We eventually realised that that wasn't the correct way to go to implement `NewType`, but it still might be for three-argument `type()`, functional typeddicts, functional namedtuples and functional enums.

---

_Comment by @charliermarsh on 2025-12-30 17:34_

I can give that a try. I initially tried to add another variant to `ClassType`, but there was so much coupling to the class AST node that it ended up not being worth it in my assessment.

---

_Comment by @charliermarsh on 2025-12-30 17:35_

(The context around functional namedtuples, functional typeddicts, and functional enums is also very useful, thank you.)

---

_Converted to draft by @charliermarsh on 2025-12-30 19:12_

---

_Marked ready for review by @charliermarsh on 2025-12-31 20:57_

---

_Converted to draft by @charliermarsh on 2025-12-31 21:16_

---

_Marked ready for review by @charliermarsh on 2026-01-01 00:15_

---

_Comment by @carljm on 2026-01-06 04:15_

This needs a rebase.

---

_Comment by @charliermarsh on 2026-01-06 14:48_

Should be rebased now. It was a little gnarly, sorry for any fragments...

---

_@charliermarsh reviewed on 2026-01-06 14:48_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:2114 on 2026-01-06 14:48_

I'm not a huge fan of `Stmt` vs. `Functional`. I would almost prefer `Declarative` vs. `Functional`.

---

_Comment by @carljm on 2026-01-08 01:42_

Ecosystem report looks like mostly good news, a few quick notes:

1. It looks like this PR now allows inheritance from `type[...]` (SubclassOf) types. Is that intentional? I think it might be OK -- we've intentionally not done it so far, but I'm not sure it can actually cause unsoundness. Mypy and Pyrefly seem to allow it, Pyright does not.
2. The new diagnostics in `src/prefect/flow_engine.py` look a bit concerning. I don't think those are non-determinism, and it looks like we are now failing to specialize away a type variable from a method call. I think we need to understand what's happening here.

---

_Comment by @charliermarsh on 2026-01-08 01:47_

Which new diagnostics are you referring to? I only see these removed diagnostics: https://dee97dfa.ty-ecosystem-ext.pages.dev/diff

<img width="876" height="594" alt="Screenshot 2026-01-07 at 8 47 00 PM" src="https://github.com/user-attachments/assets/276f08f3-3569-4603-badf-1563ec32513a" />


---

_Comment by @charliermarsh on 2026-01-08 01:47_

I assume I'm looking in the wrong place.

---

_Comment by @carljm on 2026-01-08 01:57_

Oh interesting! I was still looking at the pre rebase ecosystem report. And it was exactly those same diagnostics now showing as removed that were showing as new in that report. So maybe they are non deterministic. In any case, no need to worry about them. 

---

_Comment by @MichaReiser on 2026-01-08 07:36_

> . In any case, no need to worry about them.

Shouldn't we worry about any new non-deterministic diagnostics to avoid making ty even more non-deterministic? It's obviously possible that the they're non-deterministic because of an existing issue but it might be worth to scan through the code to see if there's anything that might introduce new non-determinism (are we iterating over a `FxHashMap`, any non-deterministic cycle handling...)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:982 on 2026-01-08 07:37_

Nit: You could write this as:


```suggestion
            .and_then(|class| crate::types::enums::enum_metadata(db, instance.class_literal(db)?))
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 07:43_

I find `as_stmt` somewhat confusing because I immediately associate it with `ast::Stmt`. 

I think I would go with:

* `ClassDefinition` which is the terminology used in the [Python documentation](https://docs.python.org/3/reference/compound_stmts.html#class)
* `Static` and `Dynamic`, `dynamic` is the terminology used in the [`type` documentation](https://docs.python.org/3/library/functions.html#type) *This is essentially a dynamic form of the [class](https://docs.python.org/3/reference/compound_stmts.html#class) statement.*

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3262 on 2026-01-08 07:45_

```suggestion
                    .and_then(|(class_literal, _)| {
                    	let metadata = enum_metadata(db, class_literal)?;
                    	let index = *metadata.members.get_index(0)?;
                    	Some(Place::bound(index))
                  	})
                  	.unwrap_or(Place::Undefined)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3371 on 2026-01-08 07:45_

```suggestion
                        .and_then(|class| Some(class.stmt_class_literal(db)?.0)),
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8996 on 2026-01-08 07:46_

```suggestion
                    Some(Type::ClassLiteral(
                    	instance
                        .class(db)
                        .stmt_class_literal(db)?
                        .0
                        .into()
                    ))
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/arguments.rs`:409 on 2026-01-08 07:47_

```suggestion
            if let Some((class_literal, _)) = class.stmt_class_literal(db) &&
            	let Some(enum_members) = enum_member_literals(db, class_literal, None) {
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:91 on 2026-01-08 07:49_

That's a lot of monomorphized code. Does the code need to be generic over `I`? Are there parts that we can move into non-generic functions?

I also think this should come after the `ClassLiteral` definition as it seems more an implementation detail

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:682 on 2026-01-08 07:50_

```suggestion
        self.as_stmt()?.known(db)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4738 on 2026-01-08 07:53_

Could we use `deref` here? (so that salsa returns `&[ClassBase]`) or are there cases where we need to know that it's a boxed slice?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4784 on 2026-01-08 07:55_

```suggestion
        let (mut candidate_base, rest) = bases.split_first().unwrap();

        // Reconcile with other bases' metaclasses.
        for base in rest {
```

---

_@MichaReiser reviewed on 2026-01-08 07:57_

---

_@charliermarsh reviewed on 2026-01-08 14:43_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 14:43_

Yeah I like this too. @carljm -- does that seem reasonable? (Just want to avoid multiple rounds of renaming if possible.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:981 on 2026-01-08 14:51_

we'll probably have to use a `match` here in the future when we add support for functional enums, so is it worth switching to use a `match` here now? It might lead to less code churn in the future

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 14:52_

> I find `as_stmt` somewhat confusing because I immediately associate it with `ast::Stmt`.

I think that's sort of the point? One kind of class is created using a `class` statement, and the other is created using a call expression.

But I also like your proposed terminology!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5489 on 2026-01-08 14:56_

what's the motivation for making this a separate branch in the `match`? It still looks like the branch arm does the same thing as the following one to me

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6543 on 2026-01-08 14:58_

we should be able to provide a `TypeDefinition` for dynamic `type()` classes too -- you might have to add a new variant to the `TypeDefinition` enum, but that shouldn't be too hard

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12725 on 2026-01-08 15:00_

in the long term we won't be able to count on all `enum_class`es having `StmtClassDef` nodes in the AST, due to the functional syntax for creating enum classes. (But no change requested; this seems fine for now.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12953 on 2026-01-08 15:01_

why is this taking a `StmtClassLiteral` rather than a `ClassLiteral`? All kinds of class-literals have MROs, right?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/bound_super.rs`:1 on 2026-01-08 15:10_

most of the changes to this file *look* unnecessary -- but I think that's just because you need to fix some merge conflicts, and they'll probably go away from the diff after a rebase?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1396 on 2026-01-08 15:15_

```suggestion
                                    .map(|s| Name::new(s.value(db)));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:755 on 2026-01-08 15:23_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:763 on 2026-01-08 15:24_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:578 on 2026-01-08 15:32_

is this correct? Couldn't you create a protocol class using the `type()` function?

```py
from typing import Protocol

X = type("X", (Protocol,), {})
```

though I don't know why you would, since you wouldn't be able to define any protocol members. Maybe we should just explicitly ban that, and emit a diagnostic for that? (Fine to defer that to a followup.)

There are lots of assumptions everywhere that you can't define a generic class using the `type()` function, so we should probably try to emit a diagnostic for

```py
from typing import Generic, TypeVar

T = TypeVar("T")

X = type("X", (Generic[T],), {})
```

Interestingly, all type checkers reject "dynamic generic classes" like the one above, but mypy _does_ support defining generic functional namedtuples/typeddicts:

```py
from typing import Generic, NamedTuple, TypeVar, TypedDict

T = TypeVar("T")

X = NamedTuple("X", [("a", T)])
Y = TypedDict("Y", {"some-key": T})

def _(obj: X[int], obj2: Y[int]):
    reveal_type(obj)  # revealed: tuple[builtins.int, fallback=__main__.X[builtins.int]]
    reveal_type(obj2)  # revealed: TypedDict('__main__.Y', {'some-key': builtins.int})
```

the second case here is important, actually, because it's the only way to define a generic `TypedDict` with invalid-identifier keys.

https://mypy-play.net/?mypy=latest&python=3.12&gist=5f45711ace3174d8a87b2c4d14920f57

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:825 on 2026-01-08 15:32_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:837 on 2026-01-08 15:33_

Ideally, functional classes would store a reference to the `ExprCall` `Definition` that was used to define them, and return that `Definition` here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:855 on 2026-01-08 15:34_

```suggestion
            Self::Functional(_) => {
                Type::instance(db, ClassType::NonGeneric(self))
            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:875 on 2026-01-08 15:34_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:896 on 2026-01-08 15:35_

```suggestion
            Self::Functional(_) => ClassType::NonGeneric(self),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4692 on 2026-01-08 15:40_

Hmm, but pyright's behaviour seems incorrect here? Ideally we would treat these as distinct types, the same as we do for different class-literals that have the same name. Storing the `Definition` on the `FunctionalClassLiteral` struct, as I suggested above, might help with that.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4893 on 2026-01-08 15:42_

I would add `#[salsa::tracked]` to the `impl` block above and then put this method in that `impl` block. Just because `#[salsa::tracked]` is added to an `impl` block doesn't mean that all methods in that `impl` block need to be themselves decorated with `#[salsa::tracked]`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4766 on 2026-01-08 15:43_

```suggestion
        // If no bases, metaclass is `type`.
        // To dynamically create a class with no bases that has a custom metaclass,
        // you have to invoke that metaclass rather than `type()`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:321 on 2026-01-08 15:46_

At some point we need to add support for the fact that all `Protocol` classes actually have `_ProtocolMeta` as their metaclass (https://github.com/astral-sh/ty/issues/1204); you could possibly add a TODO here about that

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:373 on 2026-01-08 15:48_

```suggestion
                let Some((class_literal, specialization)) = class.stmt_class_literal(db) else {
                    // Functional classes can't have cyclic MRO since their bases must
                    // already exist at creation time. Unlike statement classes, we do not
                    // permit functional classes to have forward references in their
                    // bases list.
                    return false;
                };
```

Do you have any tests for attempts to create cyclic MROs using `type()` in stub files? If not, you could add this as a test (make sure you make the mdtest a `.pyi` file!):

```py
X = type("X", (X,), {})
```

(you don't panic on it, which is good 😄)

---

_@AlexWaygood reviewed on 2026-01-08 15:51_

Wow, thanks for taking this on!!

---

_Comment by @jelle-openai on 2026-01-08 18:06_

> but I'm not sure it can actually cause unsoundness

```python
def func(x: int) -> str:
    class Base1:
        pass
    
    class Base2:
        def meth(self, x: int) -> str:
            return str(x)
    
    class Sneaky(Base1):
        def meth(self, x: int) -> int:
            return x
            
    t: type[Base1] = Sneaky
    
    class Child(t, Base2):
        pass
    
    c = Child()
    return c.meth(x)
```

Pyrefly seems to allow this and it is unsound. Mypy rejects the inheritance from `t`, not sure what you were doing that made it accept it.

---

_Comment by @carljm on 2026-01-08 20:01_

Thanks @jelle-openai ! I didn't spend very long thinking about it, and knew I was probably missing something. I was thinking that Liskov checking should make it safe, but of course that doesn't apply when subclasses introduce new methods/attributes.

I agree this means we shouldn't allow this, so whatever is changing in this PR to allow it, let's not make that change (even if it shows up in the ecosystem as a thing people try to do.)

> Mypy rejects the inheritance from `t`, not sure what you were doing that made it accept it.

Don't have the playground I used anymore, but probably it was the usual -- forgetting that mypy doesn't check all code by default.




---

_Comment by @carljm on 2026-01-08 20:02_

(This PR needs conflict resolution again, with the bound-super PR that landed.)

---

_Comment by @charliermarsh on 2026-01-08 20:02_

Sounds good -- I will rebase and address some of the initial feedback tonight.

---

_Comment by @carljm on 2026-01-08 20:04_

> Shouldn't we worry about any new non-deterministic diagnostics to avoid making ty even more non-deterministic?

Yes, I guess I was thinking "the non-determinism in prefect isn't new", but if these particular diagnostics are really new (not totally sure, but I don't think I remember this block of added/removed diagnostics) we should try to avoid adding them.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-08 20:05_

I also like @MichaReiser 's proposed terminology!

---

_@carljm reviewed on 2026-01-08 20:05_

---

_Comment by @AlexWaygood on 2026-01-08 20:06_

> > Shouldn't we worry about any new non-deterministic diagnostics to avoid making ty even more non-deterministic?
> 
> Yes, I guess I was thinking "the non-determinism in prefect isn't new", but if these particular diagnostics are really new (not totally sure, but I don't think I remember this block of added/removed diagnostics) we should try to avoid adding them.

I've seen the diagnostics in `src/prefect/flow_engine.py` come and go on quite a few other PRs; there's nothing new about those flakes in particular

---

_Comment by @AlexWaygood on 2026-01-08 20:08_

I agree with Carl and Jelle that we definitely shouldn't allow this (we don't on `main`, and that's deliberate):

```py
class Foo: ...

def f(x: type[Foo]):
    class Y(x): ...
```

---

_@AlexWaygood reviewed on 2026-01-08 20:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:263 on 2026-01-08 20:13_

huh? Not sure I understand this comment. For a class defined as `X = type("X", (A, B), {})`, surely the explicit bases of `X` are `[A, B]`?

---

_Comment by @charliermarsh on 2026-01-08 22:21_

Reducing the diff quite a bit here (hopefully) by using `ClassLiteral` in more places (i.e., pushing the match on `ClassLiteral` lower).

---

_@charliermarsh reviewed on 2026-01-08 22:40_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/mro.rs`:263 on 2026-01-08 22:40_

Sorry, this comment is clearly wrong. (But we only ever call this particular method with a statement-based class.) I will clean up.

---

_Converted to draft by @charliermarsh on 2026-01-08 23:00_

---

_Comment by @charliermarsh on 2026-01-08 23:00_

Converting to draft while I work through the feedback.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/ty_walltime.rs`:166 on 2026-01-09 01:17_

You can stick to round numbers here and give us a buffer, so we don't have to bump this on every small change.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:118 on 2026-01-09 01:22_

I kinda think this behavior isn't great, and I'd rather have mypy/pyrefly's behavior (where all attributes are allowed, with type `Unknown`), or better, just attributes named in the namespace dict are allowed, with type `Unknown`. (Or we could even infer types from the assigned elements in the namespace dict?)

Not saying we should do it in this PR, though. And we probably don't even need a TODO for it here, we can wait for someone to file an issue.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:162 on 2026-01-09 01:29_

This test demonstrates (and the comment in it explains) the opposite of what the prose above it claims.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:169 on 2026-01-09 01:30_

And this test isn't really about disjointness at all, just subtyping

---

_@charliermarsh reviewed on 2026-01-09 01:38_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:612 on 2026-01-09 01:38_

I _think_ we want to omit dynamic classes here.

---

_@carljm reviewed on 2026-01-09 01:39_

Holding off on more code review since this is currently in draft and getting revisions, but I did take a look at the tests quick.

---

_@charliermarsh reviewed on 2026-01-09 01:56_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:1 on 2026-01-09 01:56_

I'm not very confident in what's in here.

---

_Marked ready for review by @charliermarsh on 2026-01-09 02:27_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-09 02:27_

---

_Comment by @charliermarsh on 2026-01-09 02:29_

Okay, remarking as ready for review:

- I renamed to `StaticClassLiteral` and `DynamicClassLiteral`.
- I removed a variety of special-casings (like for display, etc.) that were unnecessary and wrong. (Likely Claude fragments from iterating on this a few times.)
- I implemented `Definition` and span tracking for dynamic class literals. This enabled me to remove a lot of special-casing that was no longer necessary as it further unified the `ClassLiteral` interface.

The pieces in which I have lowest confidence:

- The actual call parsing and inference in `builder.rs`, and the fall-through handling.
- The MRO handling for dynamic classes.


---

_@charliermarsh reviewed on 2026-01-09 15:57_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:118 on 2026-01-09 15:57_

Doing this in https://github.com/astral-sh/ruff/pull/22480 (in part because it actually helps the code generalize a bit more).

---

_@AlexWaygood reviewed on 2026-01-09 17:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1275 on 2026-01-09 17:10_

Okay this was a great call @MichaReiser -- I find the code so much more readable now we've switched to this terminology!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:1 on 2026-01-09 17:23_

There are so many new tests here that it might be worth moving all `type()`-related assertions in this file to a new, standalone, `type.md` mdtest suite

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12720 on 2026-01-09 19:16_

looks like a spurious change

```suggestion
        self.enum_class(db).to_non_generic_instance(db)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3258 on 2026-01-09 19:19_

can be this if you rebase on `main`

```suggestion
                    .unwrap_or_default()
```

though idk if that's actually any clearer at the end of the day 😆

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1138 on 2026-01-09 19:21_

Why must this specifically be a static class-literal? In theory we could support this, right? (We don't have to do it in this PR, but you could add a test and/or a TODO comment)

```py
from dataclasses import dataclass

T = type("T", (), {})
T = dataclass(T)
```

If we made this change, `CodeGeneratorKind::from_class` would need to be updated so that it accepted a `ClassLiteral` rather than a `StaticClassLiteral`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:580 on 2026-01-09 19:32_

```suggestion
    /// Returns whether this class is a `TypedDict`.
    ///
    // TODO: We should emit a diagnostic if a functional class (created via `type()`) attempts
    // to inherit from `TypedDict`. To create a functional TypedDict, you should invoke
    // `TypedDict` as a function, not `type`. See also: `generic_context`, `is_protocol`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:556 on 2026-01-09 19:38_

The code here is correct but the comments are wrong:
- Functional types _can_ be tuple subclasses: your branch gets this right!

  ```py
  from ty_extensions import reveal_mro

  X = type("X", (tuple[int, str],), {})
  reveal_mro(X)  # Revealed MRO: (<class 'X'>, <class 'tuple[int, str]'>, <class 'Sequence[int | str]'>, <class 'Reversible[int | str]'>, <class 'Collection[int | str]'>, <class 'Iterable[int | str]'>, <class 'Container[int | str]'>, typing.Protocol, typing.Generic, <class 'object'>)
  ```

- But this method isn't actually asking "Is this class a tuple subclass?", it's asking "Is this the tuple class exactly?"

```suggestion
    /// Returns whether this class is `builtins.tuple` exactly
    pub(crate) fn is_tuple(self, db: &'db dyn Db) -> bool {
        match self {
            Self::Static(class) => class.is_tuple(db),
            Self::Dynamic(_) => false,
        }
    }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:653 on 2026-01-09 19:40_

this will need to be revised in https://github.com/astral-sh/ruff/pull/22480, when we start adding support for dynamic classes to define attributes via the namespace parameter

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:628 on 2026-01-09 19:44_

this API now feels like a bit of a footgun, because I think you usually want to go via higher-level APIs for attribute access, etc. (which will work for both static classes and dynamic classes) rather than asking for the body scope. Considering that it is only used in one place, I'd be inclined to get rid of it:

```diff
diff --git a/crates/ty_python_semantic/src/place.rs b/crates/ty_python_semantic/src/place.rs
index 1eb7e0b511..6b44f328af 100644
--- a/crates/ty_python_semantic/src/place.rs
+++ b/crates/ty_python_semantic/src/place.rs
@@ -1606,7 +1606,9 @@ mod implicit_globals {
     use crate::place::{Definedness, PlaceAndQualifiers};
     use crate::semantic_index::symbol::Symbol;
     use crate::semantic_index::{place_table, use_def_map};
-    use crate::types::{KnownClass, MemberLookupPolicy, Parameter, Parameters, Signature, Type};
+    use crate::types::{
+        ClassLiteral, KnownClass, MemberLookupPolicy, Parameter, Parameters, Signature, Type,
+    };
     use ruff_python_ast::PythonVersion;
 
     use super::{DefinedPlace, Place, place_from_declarations};
@@ -1756,10 +1758,11 @@ mod implicit_globals {
             return smallvec::SmallVec::default();
         };
 
-        let Some(module_type_scope) = module_type.body_scope(db) else {
+        let ClassLiteral::Static(module_type) = module_type else {
             return smallvec::SmallVec::default();
         };
-        let module_type_symbol_table = place_table(db, module_type_scope);
+
+        let module_type_symbol_table = place_table(db, module_type.body_scope(db));
 
         module_type_symbol_table
             .symbols()
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index d67e927f39..5cf85f120c 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -621,11 +621,6 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns the body scope of this class, if it's a statement class.
-    pub(crate) fn body_scope(self, db: &'db dyn Db) -> Option<ScopeId<'db>> {
-        self.as_static().map(|class| class.body_scope(db))
-    }
-
     /// Returns the definition of this class, if available.
     pub(crate) fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
         match self {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:721 on 2026-01-09 19:47_

we may want to revise this in https://github.com/astral-sh/ruff/pull/22480, because a dynamic class could provide `__slots__` in the namespace dictionary, which would make it a disjoint base:

```pycon
>>> X = type("X", (), {"__slots__": ("a",)})
>>> class Foo(int, X): ...
... 
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    class Foo(int, X): ...
TypeError: multiple bases have instance lay-out conflict
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:348 on 2026-01-09 20:01_

when we hit this situation for static class literals, we still infer the class as a class-literal, but (after emitting a diagnostic to tell the user off about it) we insert `Unknown` into the MRO. This has the nice property that we will allow arbitrary attributes to be accessed on instances of the class: the user gets one (and exactly one) diagnostic about the bad class definition, rather than getting many other diagnostics about unresolved attributes on instances of the class:

```py
from ty_extensions import reveal_mro

def f(x: type[int]):
    class Foo(x): ...  # error: [unsupported-base]

    reveal_mro(Foo)  # revealed: (<class 'Foo'>, Unknown, <class 'object'>)
```

Should we just do the same for dynamic class-literals?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:1 on 2026-01-09 20:02_

Can you also add a test that attributes defined on `builtins.type` are accessible on dynamic classes? It looks like you get this behaviour correct on your branch, which is great:

```py
T = type("T", (), {})

# inherited from `builtins.type`:
reveal_type(T.__dictoffset__)  # revealed: int
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4519 on 2026-01-09 20:09_

`type[]` types are different from class-literal types: `type[Foo]` means "the class object `Foo` or any subclass of `Foo`"; `<class 'Foo'>` means "the class object `Foo`, and only that exact class object (no subclasses)"

```suggestion
/// The type of `Foo` would be `<class 'Foo'>` where `Foo` is a `DynamicClassLiteral` with:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4678 on 2026-01-09 20:10_

nit

```suggestion
/// This matches Pyright's behavior. For example:
///
/// ```python
/// Foo = type("Foo", (), {"attr": 42})
/// Foo().attr  # Error: no attribute 'attr'
/// ```
///
/// Supporting namespace dict attributes would require parsing dict literals and tracking
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4688 on 2026-01-09 20:10_

```suggestion
/// same name and bases:
///
/// ```python
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4712 on 2026-01-09 20:14_

We may need to put some warnings in the doc-comments for some of these fields that they shouldn't be accessed when we're inferring types for a different module to the one the class was defined in? I think doing so would lead to overly aggressive cache invalidation. @MichaReiser will be able to say whether I'm spouting nonsense here or not

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4596 on 2026-01-09 20:18_

```suggestion
#[salsa::tracked]
impl<'db> DynamicClassLiteral<'db> {
    /// Returns a [`Span`] with the range of the `type()` call expression.
    ///
    /// See [`Self::header_range`] for more details.
    pub(super) fn header_span(self, db: &'db dyn Db) -> Span {
        Span::from(self.file(db)).with_range(self.header_range(db))
    }

    /// Returns the range of the `type()` call expression that created this class.
    pub(super) fn header_range(self, db: &'db dyn Db) -> TextRange {
        let module = parsed_module(db, self.file(db)).load(db);
        let node = module.get_by_index(self.node_index(db));
        node.range()
    }

```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4624 on 2026-01-09 20:21_

this isn't quite correct: we have this behaviour on your branch:

```py
class Foo: ...
Bar = type("Bar", (), {})

reveal_type(Foo.__class__)  # <class 'type'>
reveal_type(Bar.__class__)  # type
```

so whereas for the static class, we return "the class `type` itself", for the functional class we return "instance of `type`". We should be consistent here and return "the class `type` itself" in both cases:

```suggestion
            return Ok(KnownClass::Type.to_class_literal(db));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4881 on 2026-01-09 20:23_

nit: I'd find this more readable

```suggestion
        let result = MroLookup::new(db, self.iter_mro(db)).class_member(
            name,
            policy,
            None,  // No inherited generic context.
            false,  // Functional classes are never `object`.
        );
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4961 on 2026-01-09 20:33_

you can do this without the `unreachable!()` if you add another intermediate type:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index d67e927f39..d773c7daef 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -488,7 +488,7 @@ impl<'db> ClassLiteral<'db> {
                 // Functional classes don't have inherited generic context and are never `object`.
                 let result = MroLookup::new(db, mro_iter).class_member(name, policy, None, false);
                 match result {
-                    ClassMemberResult::Done { .. } => result.finalize(db),
+                    ClassMemberResult::Done(result) => result.finalize(db),
                     ClassMemberResult::TypedDict => KnownClass::TypedDictFallback
                         .to_class_literal(db)
                         .find_name_in_mro_with_policy(db, name, policy)
@@ -2634,7 +2634,7 @@ impl<'db> StaticClassLiteral<'db> {
         );
 
         match result {
-            ClassMemberResult::Done { .. } => result.finalize(db),
+            ClassMemberResult::Done(result) => result.finalize(db),
 
             ClassMemberResult::TypedDict => KnownClass::TypedDictFallback
                 .to_class_literal(db)
@@ -4713,7 +4713,7 @@ impl<'db> DynamicClassLiteral<'db> {
         );
 
         match result {
-            ClassMemberResult::Done { .. } => result.finalize(db),
+            ClassMemberResult::Done(result) => result.finalize(db),
             ClassMemberResult::TypedDict => {
                 // Simplified `TypedDict` handling without type mapping.
                 KnownClass::TypedDictFallback
@@ -4848,10 +4848,10 @@ impl<'db, I: Iterator<Item = ClassBase<'db>>> MroLookup<'db, I> {
             }
         }
 
-        ClassMemberResult::Done {
+        ClassMemberResult::Done(CompletedMemberLookup {
             lookup_result,
             dynamic_type,
-        }
+        })
     }
 
     /// Look up an instance member by iterating through the MRO.
@@ -4942,50 +4942,46 @@ impl<'db, I: Iterator<Item = ClassBase<'db>>> MroLookup<'db, I> {
 /// Result of class member lookup from MRO iteration.
 pub(super) enum ClassMemberResult<'db> {
     /// Found the member or exhausted the MRO.
-    Done {
-        lookup_result: LookupResult<'db>,
-        dynamic_type: Option<Type<'db>>,
-    },
+    Done(CompletedMemberLookup<'db>),
     /// Encountered a `TypedDict` base.
     TypedDict,
 }
 
-impl<'db> ClassMemberResult<'db> {
+pub(super) struct CompletedMemberLookup<'db> {
+    lookup_result: LookupResult<'db>,
+    dynamic_type: Option<Type<'db>>,
+}
+
+impl<'db> CompletedMemberLookup<'db> {
     /// Finalize the lookup result by handling dynamic type intersection.
     pub(super) fn finalize(self, db: &'db dyn Db) -> PlaceAndQualifiers<'db> {
-        match self {
-            ClassMemberResult::TypedDict => {
-                // Caller should handle TypedDict case before calling finalize
-                unreachable!("finalize called on TypedDict result")
-            }
-            ClassMemberResult::Done {
-                lookup_result,
-                dynamic_type,
-            } => match (PlaceAndQualifiers::from(lookup_result), dynamic_type) {
-                (symbol_and_qualifiers, None) => symbol_and_qualifiers,
+        match (
+            PlaceAndQualifiers::from(self.lookup_result),
+            self.dynamic_type,
+        ) {
+            (symbol_and_qualifiers, None) => symbol_and_qualifiers,
 
-                (
-                    PlaceAndQualifiers {
-                        place: Place::Defined(DefinedPlace { ty, .. }),
-                        qualifiers,
-                    },
-                    Some(dynamic),
-                ) => Place::bound(
-                    IntersectionBuilder::new(db)
-                        .add_positive(ty)
-                        .add_positive(dynamic)
-                        .build(),
-                )
-                .with_qualifiers(qualifiers),
+            (
+                PlaceAndQualifiers {
+                    place: Place::Defined(DefinedPlace { ty, .. }),
+                    qualifiers,
+                },
+                Some(dynamic),
+            ) => Place::bound(
+                IntersectionBuilder::new(db)
+                    .add_positive(ty)
+                    .add_positive(dynamic)
+                    .build(),
+            )
+            .with_qualifiers(qualifiers),
 
-                (
-                    PlaceAndQualifiers {
-                        place: Place::Undefined,
-                        qualifiers,
-                    },
-                    Some(dynamic),
-                ) => Place::bound(dynamic).with_qualifiers(qualifiers),
-            },
+            (
+                PlaceAndQualifiers {
+                    place: Place::Undefined,
+                    qualifiers,
+                },
+                Some(dynamic),
+            ) => Place::bound(dynamic).with_qualifiers(qualifiers),
         }
     }
 }
```

---

_@AlexWaygood reviewed on 2026-01-09 20:33_

Sorry, just a partial review but I gotta go!

---

_@charliermarsh reviewed on 2026-01-09 21:12_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:628 on 2026-01-09 21:12_

Smart!

---

_@charliermarsh reviewed on 2026-01-09 21:18_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:4881 on 2026-01-09 21:18_

Rust un-formats that away sadly :(

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/ty_walltime.rs`:174 on 2026-01-10 02:07_

I suspect we don't need this? No significant number of new diagnostics on pandas in the current iteration of the PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/type.md`:70 on 2026-01-10 02:11_

nit: should we match the code and call these "dynamic classes" throughout the tests? It seems clearer to me than "functional classes".

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/type.md`:469 on 2026-01-10 02:22_

There's an issue here that shows up in pywin32 in the ecosystem:

```py
from not_found import Bar, Baz  # import from unknown module to get values of type Unknown

class Child(Bar, Baz):  # no diagnostic here, on main or this PR
    pass

X = type("X", (Bar, Baz), {})  # duplicate-bases diagnostic here on this PR
```

We don't emit a duplicate-bases diagnostic on `Child`, but in this PR we do on `X`. There shouldn't be a diagnostic on either one -- as far as we know (and should assume), `Bar` and `Baz` are separate types and it's perfectly valid to inherit from both; we just don't know what types they are.

The way this is handled in the static-class MRO logic is that we don't emit duplicate-base errors until after we've tried and failed to linearize an MRO -- and multiple `Unknown` or `Any` bases doesn't fail linearization. Might be ideal to see if we can use the same approach for dynamic classes?

If not, the other approach would be to manually avoid emitting this diagnostic if the "dupes" are `Type::Dynamic`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/type.md`:518 on 2026-01-10 02:23_

This means we aren't treating the bases of a dynamic class as forward references (deferred evaluation) in a stub file?

I think that's fine -- nobody should be using `type(...)` in a stub file anyway.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1123 on 2026-01-10 02:25_

Should there be a TODO here that this should also work for dynamic class literal types?

---

_@carljm reviewed on 2026-01-10 02:26_

I also didn't make it through a complete review before needing to head out, but I left what I've got so far.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:518 on 2026-01-10 10:49_

Yeah, I specifically asked Charlie to add this test to demonstrate that we wouldn't panic on cyclic definitions in stub files 😄

---

_@AlexWaygood reviewed on 2026-01-10 10:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:404 on 2026-01-10 12:23_

but note that that `weird_other_arg` would actually be _mandatory_ if you created a dynamic class that had `Base` as a superclass, and `Base.__init_subclass__` required a `weird_other_arg` argument to be passed to it:

```pycon
>>> class Base:
...     def __init_subclass__(cls, weird_other_arg: int): ...
...     
>>> X = type("X", (Base,), {}, weird_other_arg=42)
>>> Y = type("X", (Base,), {})
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    Y = type("X", (Base,), {})
TypeError: Base.__init_subclass__() missing 1 required positional argument: 'weird_other_arg'
```

We don't handle this properly yet for static classes (https://github.com/astral-sh/ty/issues/1541), though we have an open PR to fix that (https://github.com/astral-sh/ruff/pull/22185). Maybe we should just land that PR and then rebase this on top of that...


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-10 12:27_

What happens if you create a type with a dynamic name?

```py
def f(x: str):
    X = type(x, (), {})
    reveal_type(X)]
```

I think our ideal behaviour there would maybe be that we'd still infer it as a class-literal type, but in our display we'd show something like `<class '<unknown>'> @ main.py:42`...?

---

What happens if you create a type with dynamic bases?

```py
from ty_extensions import reveal_mro

def f(bases: tuple[type, ...]):
    X = type("X", bases, {})
    reveal_type(X)
    reveal_mro(X)
```

I think our ideal behaviour there would maybe be that we'd still infer it as a class-literal type, but the revealed MRO would be `(<class 'X'>, Unknown, <class 'object'>)`, which would lead us to treat instances of the class highly dynamically (they would have all possible attributes available on them)

---

What happens for things like this?

```py
def f(*args, **kwargs):
    A = type(*args, **kwargs)
    reveal_type(A)

    B = type("B", *args, **kwargs)
    reveal_type(B)

    C = type("C", (), *args, **kwargs)
    reveal_type(C)

    D = type("D", (), {}, **kwargs)
    reveal_type(D)
```

For `A` and `B`, `type[Any]` might lead to the fewest false-positive errors. For `C` and `D`, it feels like we could infer class-literal types...?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:382 on 2026-01-10 12:36_

Why are these commented out? They all pass for me locally 😆

```suggestion
# Inherited from `builtins.type`:
reveal_type(T.__dictoffset__)  # revealed: int
reveal_type(T.__name__)  # revealed: str
reveal_type(T.__bases__)  # revealed: tuple[type, ...]
reveal_type(T.__mro__)  # revealed: tuple[type, ...]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:6132 on 2026-01-10 12:43_

```suggestion
        let class_literal = self.to_class_literal(db).as_class_literal()?.as_static()?;
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:7106 on 2026-01-10 12:57_

```suggestion
                    // Check for metaclass conflicts
                    if let Err(FunctionalMetaclassConflict {
                        metaclass1,
                        base1,
                        metaclass2,
                        base2,
                    }) = dynamic_class.try_metaclass(db)
                        && let Some(builder) =
                            context.report_lint(&CONFLICTING_METACLASS, call_expression)
                    {
                        builder.into_diagnostic(format_args!(
                            "The metaclass of a derived class (`{class}`) \
                            must be a subclass of the metaclasses of all its bases, \
                            but `{metaclass1}` (metaclass of base class `{base1}`) \
                            and `{metaclass2}` (metaclass of base class `{base2}`) \
                            have no subclass relationship",
                            class = dynamic_class.name(db),
                            metaclass1 = metaclass1.name(db),
                            base1 = base1.display(db),
                            metaclass2 = metaclass2.name(db),
                            base2 = base2.display(db),
                        ));
                    }
```

But given that this is the exact same message that we emit for static classes with metaclass conflicts in `infer/builder.rs`, maybe we could factor out a standalone `report_*` function in `types/diagnostic.rs`, to avoid the repetition?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5168 on 2026-01-10 12:59_

```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
pub(super) enum InstanceMemberResult<'db> {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:6974 on 2026-01-10 13:00_

```suggestion
                                        "Duplicate base class{maybe_s} {dupes} in class `{class}`",
                                        maybe_s = if duplicates.len() == 1 { "" } else { "es" },
                                        dupes = duplicates.iter().map(|base| base.display(db)).join(", "),
                                        class = dynamic_class.name(db),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-10 13:12_

I'd also love some x-failing tests for the various diagnostics that we plan to add in followups:
- `Protocol` in the bases list
- `TypedDict` in the bases list
- `NamedTuple` in the bases list
- `Generic[T]` in the bases list
- `Enum` (or any `Enum` subclass) in the bases list

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:167 on 2026-01-10 13:15_

oof, splitting that type annotation over three lines is ugly 😆 Let's just import `SmallVec` here?

```diff
diff --git a/crates/ty_python_semantic/src/types/enums.rs b/crates/ty_python_semantic/src/types/enums.rs
index 2bb0e4e4d1..f5067cc675 100644
--- a/crates/ty_python_semantic/src/types/enums.rs
+++ b/crates/ty_python_semantic/src/types/enums.rs
@@ -1,5 +1,6 @@
 use ruff_python_ast::name::Name;
 use rustc_hash::FxHashMap;
+use smallvec::SmallVec;
 
 use crate::{
     Db, FxIndexMap,
@@ -162,23 +163,22 @@ pub(crate) fn enum_metadata<'db>(
                                     {
                                         Type::string_literal(db, &name.to_lowercase())
                                     } else {
-                                        let custom_mixins: smallvec::SmallVec<
-                                            [Option<KnownClass>; 1],
-                                        > = class
-                                            .iter_mro(db, None)
-                                            .skip(1)
-                                            .filter_map(ClassBase::into_class)
-                                            .filter(|class| {
-                                                !Type::from(*class).is_subtype_of(
-                                                    db,
-                                                    KnownClass::Enum.to_subclass_of(db),
-                                                )
-                                            })
-                                            .map(|class| class.known(db))
-                                            .filter(|class| {
-                                                !matches!(class, Some(KnownClass::Object))
-                                            })
-                                            .collect();
+                                        let custom_mixins: SmallVec<[Option<KnownClass>; 1]> =
+                                            class
+                                                .iter_mro(db, None)
+                                                .skip(1)
+                                                .filter_map(ClassBase::into_class)
+                                                .filter(|class| {
+                                                    !Type::from(*class).is_subtype_of(
+                                                        db,
+                                                        KnownClass::Enum.to_subclass_of(db),
+                                                    )
+                                                })
+                                                .map(|class| class.known(db))
+                                                .filter(|class| {
+                                                    !matches!(class, Some(KnownClass::Object))
+                                                })
+                                                .collect();
 
                                         // `IntEnum`s have the same `auto()` behaviour to enums inheriting from `(int, Enum)`,
                                         // and `IntEnum`s also have `int` in their MROs, so both cases are handled here.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:39 on 2026-01-10 13:21_

```suggestion
            // Dynamic classes cannot be `TypedDict`s or protocols and they cannot be
            // the class `builtins.tuple`.
```

But I think it would be more future-proof to do an explicit `match` here over the `ClassLiteral` variant rather than calling `.static_class_literal(db)`. In the future we intend to add another `ClassLiteral` variant corresponding to functional `TypeDict`s; for that variant, we will want to go down the same codepath as the `class_literal.is_typed_dict(db).then(|| Type::typed_dict(class))` bit of this method a few lines below.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/list_members.rs`:1 on 2026-01-10 13:22_

Can we add some tests to https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md that demonstrate which members our autocomplete machinery considers to be available on instances of dynamic classes?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:51 on 2026-01-10 13:25_

We will want to change this in https://github.com/astral-sh/ruff/pull/22480 so that it accepts dynamic classes too: if we allow dynamic classes to define attributes in their namespace dictionary, we should also check whether those attributes are valid overrides of attributes in their superclasses.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:612 on 2026-01-10 13:29_

It looks like most of these checks either don't apply to dynamic classes, or we have them independently implemented elsewhere for dynamic classes. So I think this is fine. But we should rename the method to `check_static_class_definitions, and mention in the docstring that we only iterate over class definitions created using `class` statements.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-10 13:32_

What happens if you provide an explicit annotation when creating a dynamic class? I don't actually care _too_ much about our behaviour here right now, because we intend to change how this stuff works more broadly pretty soon in https://github.com/astral-sh/ty/issues/136. But let's add a test for it:

```py
T: type = type("T", (), {})
reveal_type(T)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5419 on 2026-01-10 13:34_

```suggestion
                        // Only handle 3-arg type() calls specially to capture the definition.
                        // 1-arg type() calls are handled by normal call binding.
                        Some(KnownClass::Type)
                            if call_expr.arguments.args.len() == 3
                                && call_expr.arguments.keywords.is_empty() =>
                        {
                            // Try to extract the functional class with definition.
                            // Fall back to regular call binding if extraction fails
                            // (e.g., for cyclic references where names aren't resolved yet).
                            self.infer_functional_type_expression(call_expr, definition)
                                .unwrap_or_else(|| {
                                    self.infer_call_expression_impl(call_expr, callable_type, tcx)
                                })
                        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6057 on 2026-01-10 13:36_

we should check whether we can infer the name as a string-literal _type_ rather than checking whether it's a string-literal _node_, so that we can support things like

```py
name = "some very very long name"
T = type(name, (), {})
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6075 on 2026-01-10 13:44_

I think here you want to do something like this:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index b3cacb5663..96c3243ad0 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -6077,10 +6077,13 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             KnownClass::Object.try_to_class_literal(db).unwrap().into();
 
         let base_classes: Option<Box<[ClassBase<'db>]>> = bases_type
-            .exact_tuple_instance_spec(db)
-            .and_then(|tuple_spec| {
-                tuple_spec
-                    .fixed_elements()
+            .tuple_instance_spec(db)
+            .as_deref()
+            .and_then(|spec|spec.as_fixed_length())
+            .and_then(|tuple| {
+                tuple
+                    .elements_slice()
+                    .iter()
                     .enumerate()
                     .map(|(idx, base)| {
                         // First try the standard conversion.
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index dfc484cabd..2bd955432b 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -1313,6 +1313,13 @@ pub enum Tuple<T> {
 }
 
 impl<T> Tuple<T> {
+    pub(crate) fn as_fixed_length(&self) -> Option<&FixedLengthTuple<T>> {
+        match self {
+            Tuple::Fixed(tuple) => Some(tuple),
+            Tuple::Variable(_) => None,
+        }
+    }
+
     pub(crate) const fn homogeneous(element: T) -> Self {
         Self::Variable(VariableLengthTuple::homogeneous(element))
     }
```

There's two changes here:
- Use `tuple_instance_spec()` rather than `exact_tuple_instance_spec()`. Tuple subclasses should be fine here. We could actually add a test for this:
- Make sure that the tuple is a fixed-length tuple. `Tuple::fixed_elements()` doesn't do this for you: if it's a tuple like `tuple[int, *tuple[str, ...]]`, it will give you the `int` element (the "fixed element"), then stop.

---

_@charliermarsh reviewed on 2026-01-10 13:46_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:70 on 2026-01-10 13:46_

Thank you, sorry. These are missed renames.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6153 on 2026-01-10 13:47_

It looks like there's quite a lot of duplication between this and the method above. Can we not have them both call into a `infer_functional_type_call_impl` method or something?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:94 on 2026-01-10 13:53_

I've been meaning for a while now to move this special casing to `class.rs`, where it would make more sense. Since this code is moving anyway in this PR, you could make that change here (or in a standalone PR and then rebase this on top of it):

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 32029a5617..ded5d5d9bc 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -2280,20 +2280,24 @@ impl<'db> StaticClassLiteral<'db> {
         let class_definition =
             semantic_index(db, self.file(db)).expect_single_definition(class_stmt);
 
-        if self.is_known(db, KnownClass::VersionInfo) {
-            let tuple_type = TupleType::new(db, &TupleSpec::version_info_spec(db))
-                .expect("sys.version_info tuple spec should always be a valid tuple");
-
-            Box::new([
-                definition_expression_type(db, class_definition, &class_stmt.bases()[0]),
-                Type::from(tuple_type.to_class_type(db)),
-            ])
-        } else {
-            class_stmt
+        match self.known(db) {
+            Some(KnownClass::VersionInfo) => {
+                let tuple_type = TupleType::new(db, &TupleSpec::version_info_spec(db))
+                    .expect("sys.version_info tuple spec should always be a valid tuple");
+
+                // Special-case `NotImplementedType`: typeshed says that it inherits from `Any`,
+                // but this causes more problems than it fixes.
+                Box::new([
+                    definition_expression_type(db, class_definition, &class_stmt.bases()[0]),
+                    Type::from(tuple_type.to_class_type(db)),
+                ])
+            }
+            Some(KnownClass::NotImplementedType) => Box::new([]),
+            _ => class_stmt
                 .bases()
                 .iter()
                 .map(|base_node| definition_expression_type(db, class_definition, base_node))
-                .collect()
+                .collect(),
         }
     }
 
diff --git a/crates/ty_python_semantic/src/types/mro.rs b/crates/ty_python_semantic/src/types/mro.rs
index eb6e05e58c..9f60c590ac 100644
--- a/crates/ty_python_semantic/src/types/mro.rs
+++ b/crates/ty_python_semantic/src/types/mro.rs
@@ -8,7 +8,7 @@ use crate::Db;
 use crate::types::class_base::ClassBase;
 use crate::types::generics::Specialization;
 use crate::types::{
-    ClassLiteral, ClassType, DynamicClassLiteral, KnownClass, KnownInstanceType, SpecialFormType,
+    ClassLiteral, ClassType, DynamicClassLiteral, KnownInstanceType, SpecialFormType,
     StaticClassLiteral, Type,
 };
 
@@ -86,13 +86,6 @@ impl<'db> Mro<'db> {
         }
 
         let class = class_literal.apply_optional_specialization(db, specialization);
-
-        // Special-case `NotImplementedType`: typeshed says that it inherits from `Any`,
-        // but this causes more problems than it fixes.
-        if class_literal.is_known(db, KnownClass::NotImplementedType) {
-            return Ok(Self::from([ClassBase::Class(class), ClassBase::object(db)]));
-        }
-
         let original_bases = class_literal.explicit_bases(db);
 
         match original_bases {
```

---

_@AlexWaygood reviewed on 2026-01-10 13:53_

This looks very good, and I think it's close! My main comment on this pass is I think we could do with a bunch more tests

---

_@charliermarsh reviewed on 2026-01-10 15:55_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:404 on 2026-01-10 15:55_

(Adding a TODO for now though happy to rebase on that if you want to merge it!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:523 on 2026-01-10 16:48_

```suggestion
                // Dynamic classes don't have inherited generic context and are never `object`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:544 on 2026-01-10 16:48_

```suggestion
    /// For static classes, this applies default type arguments.
    /// For dynamic classes, this returns a non-generic class type.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:563 on 2026-01-10 16:49_

```suggestion
    // TODO: We should emit a diagnostic if a dynamic class (created via `type()`) attempts
    // to inherit from `Generic[T]`, since dynamic classes can't be generic. See also: `is_protocol`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:571 on 2026-01-10 16:49_

```suggestion
    // TODO: We should emit a diagnostic if a dynamic class (created via `type()`) attempts
    // to inherit from `Protocol`, since dynamic classes can't be protocols. See also: `generic_context`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:578 on 2026-01-10 16:49_

```suggestion
    // TODO: We should emit a diagnostic if a dynamic class (created via `type()`) attempts
    // to inherit from `TypedDict`. To create a functional TypedDict, you should invoke
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:620 on 2026-01-10 16:49_

```suggestion
    /// For static classes, this is the class name and any arguments passed to the `class` statement.
    /// For dynamic classes, this is the entire `type()` call expression.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:769 on 2026-01-10 16:50_

```suggestion
    /// Dynamic classes cannot be `TypedDicts`, so this delegates to the Stmt variant.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:804 on 2026-01-10 16:50_

```suggestion
    fn from(dynamic: DynamicClassLiteral<'db>) -> Self {
        ClassLiteral::Dynamic(dynamic)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:946 on 2026-01-10 16:50_

```suggestion
    /// For static classes, returns `TypeDefinition::StaticClass`.
    /// For dynamic classes, returns `TypeDefinition::DynamicClass` if a definition is available.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1653 on 2026-01-10 16:50_

```suggestion
        // Dynamic classes don't have a generic context.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1877 on 2026-01-10 16:51_

```suggestion
    fn from(dynamic: DynamicClassLiteral<'db>) -> Type<'db> {
        Type::ClassLiteral(dynamic.into())
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2590 on 2026-01-10 16:51_

```suggestion
            // For dynamic classes, we can't get a StaticClassLiteral, so use self for tracking.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2645 on 2026-01-10 16:51_

```suggestion
            // For dynamic classes, we can't get a StaticClassLiteral, so use self for tracking.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2672 on 2026-01-10 16:51_

```suggestion
        // Dynamic classes don't have dataclass transformer params.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3685 on 2026-01-10 16:51_

```suggestion
                    // Dynamic classes don't have fields (no class body).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4622 on 2026-01-10 16:52_

outdated

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4695 on 2026-01-10 16:52_

```suggestion
    /// Get the metaclass of this dynamic class.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4706 on 2026-01-10 16:52_

```suggestion
    /// Try to get the metaclass of this dynamic class.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4776 on 2026-01-10 16:53_

```suggestion
    /// Iterate over the MRO of this dynamic class using C3 linearization.
    ///
    /// The MRO includes the dynamic class itself as the first element, followed
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4802 on 2026-01-10 16:53_

```suggestion
    /// - No inherited generic context (dynamic classes aren't generic).
    /// - `is_self_object = false` (dynamic classes are never `object`).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4832 on 2026-01-10 16:54_

```suggestion
            false, // Dynamic classes are never `object`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4847 on 2026-01-10 16:54_

```suggestion
    /// Try to compute the MRO for this dynamic class.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4857 on 2026-01-10 16:54_

```suggestion
/// Error for metaclass conflicts in dynamic classes.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4861 on 2026-01-10 16:55_

Should be renamed to `DynamicMetaclassConflict`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:379 on 2026-01-10 16:55_

```suggestion
                    // Dynamic classes can't have cyclic MRO since their bases must
                    // already exist at creation time. Unlike statement classes, we do not
                    // permit dynamic classes to have forward references in their
                    // bases list.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:317 on 2026-01-10 16:56_

```suggestion
    /// Attempt to resolve the MRO of a dynamic class (created via `type(name, bases, dict)`).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:322 on 2026-01-10 16:56_

parameter should be renamed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:389 on 2026-01-10 16:56_

```suggestion
    /// Compute a fallback MRO for a dynamic class when `of_dynamic_class` fails.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:394 on 2026-01-10 16:57_

parameter should be renamed

---

_Converted to draft by @charliermarsh on 2026-01-10 17:00_

---

_Comment by @charliermarsh on 2026-01-10 17:00_

(Back to draft while I address feedback.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:501 on 2026-01-10 17:00_

```suggestion
            ClassLiteral::Dynamic(dynamic) => {
                ClassBase::Class(ClassType::NonGeneric(dynamic.into()))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:688 on 2026-01-10 17:02_

```suggestion
/// Error kinds for dynamic class MRO computation.
///
/// These mirror the relevant variants from `MroErrorKind` for static classes.
```

---

_Comment by @AlexWaygood on 2026-01-10 17:21_

I think you're using `SubsequentMroElements::Owned()` more than you need to -- you could do this:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 2ac60dce6c..00ca535dc3 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -4848,19 +4848,10 @@ impl<'db> DynamicClassLiteral<'db> {
     ///
     /// Returns `Ok(Mro)` if successful, or `Err(FunctionalMroError)` if there's
     /// an error (duplicate bases or C3 linearization failure).
+    #[salsa::tracked(returns(ref), heap_size=ruff_memory_usage::heap_size)]
     pub(crate) fn try_mro(self, db: &'db dyn Db) -> Result<Mro<'db>, FunctionalMroError<'db>> {
         Mro::of_dynamic_class(db, self)
     }
-
-    /// Compute and cache the MRO for this functional class.
-    ///
-    /// Uses C3 linearization when possible, falling back to sequential iteration
-    /// with deduplication when there's an error (duplicate bases or C3 merge failure).
-    #[salsa::tracked(heap_size = ruff_memory_usage::heap_size)]
-    pub(crate) fn mro(self, db: &'db dyn Db) -> Mro<'db> {
-        self.try_mro(db)
-            .unwrap_or_else(|_| Mro::functional_fallback(db, self))
-    }
 }
 
 /// Error for metaclass conflicts in functional classes.
diff --git a/crates/ty_python_semantic/src/types/mro.rs b/crates/ty_python_semantic/src/types/mro.rs
index 9677b27603..9bb923b40a 100644
--- a/crates/ty_python_semantic/src/types/mro.rs
+++ b/crates/ty_python_semantic/src/types/mro.rs
@@ -435,6 +435,15 @@ impl<'db> FromIterator<ClassBase<'db>> for Mro<'db> {
     }
 }
 
+impl<'db> IntoIterator for Mro<'db> {
+    type Item = ClassBase<'db>;
+    type IntoIter = std::vec::IntoIter<ClassBase<'db>>;
+
+    fn into_iter(self) -> Self::IntoIter {
+        self.0.into_iter()
+    }
+}
+
 /// Iterator that yields elements of a class's MRO.
 ///
 /// We avoid materialising the *full* MRO unless it is actually necessary:
@@ -537,9 +546,14 @@ impl<'db> MroIterator<'db> {
                     SubsequentMroElements::Borrowed(full_mro_iter)
                 }
                 ClassLiteral::Dynamic(functional) => {
-                    let mro = functional.mro(self.db);
-                    let elements: Vec<_> = mro.iter().skip(1).copied().collect();
-                    SubsequentMroElements::Owned(elements.into_iter())
+                    let mut full_mro_iter = match functional.try_mro(self.db) {
+                        Ok(mro) => SubsequentMroElements::Borrowed(mro.iter()),
+                        Err(_) => SubsequentMroElements::Owned(
+                            Mro::functional_fallback(self.db, functional).into_iter(),
+                        ),
+                    };
+                    full_mro_iter.next();
+                    full_mro_iter
                 }
             })
     }
@@ -684,7 +698,7 @@ fn c3_merge(mut sequences: Vec<VecDeque<ClassBase>>) -> Option<Mro> {
 /// Error kinds for functional class MRO computation.
 ///
 /// These mirror the relevant variants from `MroErrorKind` for regular classes.
-#[derive(Debug, Clone)]
+#[derive(Debug, Clone, PartialEq, Eq, get_size2::GetSize, salsa::Update)]
 pub(crate) enum FunctionalMroError<'db> {
     /// The class has duplicate bases in its bases tuple.
     DuplicateBases(Box<[ClassBase<'db>]>),
```

But I'm a bit confused about why `Mro::functional_fallback` is as complex as it is, and why the fallback isn't stored on the error returned `DynamicClassLiteral::try_mro`. If we computed and stored the fallback MRO in a way more similar to what we do for static classes, we might not need the `SubsequentMroElements` enum at all.

---

_@charliermarsh reviewed on 2026-01-10 17:47_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6153 on 2026-01-10 17:47_

Added some shared helpers...

---

_Comment by @charliermarsh on 2026-01-10 17:47_

Yeah you're right. I think the functional MRO went through several iterations and I must've solved whatever required the enum along the way. Thanks!

---

_@charliermarsh reviewed on 2026-01-10 17:48_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:5424 on 2026-01-10 17:48_

This seems wrong, looking into it.

---

_@charliermarsh reviewed on 2026-01-10 17:49_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6228 on 2026-01-10 17:49_

(I suspect this is _not_ what you had in mind @AlexWaygood.)

---

_@charliermarsh reviewed on 2026-01-10 18:30_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:5424 on 2026-01-10 18:30_

Still not sure I have the ideal approach here.

---

_Marked ready for review by @charliermarsh on 2026-01-10 18:48_

---

_@charliermarsh reviewed on 2026-01-10 18:53_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:677 on 2026-01-10 18:53_

I think we said we wanted to ban this as a known simplification @AlexWaygood -- is that right?

---

_@charliermarsh reviewed on 2026-01-10 18:59_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:677 on 2026-01-10 18:59_

Ah no -- it was inheriting from `Protocol` itself.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:677 on 2026-01-10 19:00_

yeah, inheriting from protocol classes is fine, it just doesn't seem necessary to support creating a _new_ `Protocol` class using `type()` (which you'd do by including `Protocol` itself in the bases list)

---

_@AlexWaygood reviewed on 2026-01-10 19:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:211 on 2026-01-11 11:08_

```suggestion
Dynamic classes can be used as the pivot class in `super()` calls:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:259 on 2026-01-11 11:08_

```suggestion
# Child instances are subtypes of `Parent` instances.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:262 on 2026-01-11 11:09_

```suggestion
takes_parent(child)  # No error - `ChildCls` is a subtype of `Parent`
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:400 on 2026-01-11 11:11_

```suggestion
reveal_type(type("Foo", ()))  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:404 on 2026-01-11 11:17_

```suggestion
# TODO: the keyword arguments for `Foo`/`Bar`/`Baz` here are invalid
# (you cannot pass `metaclass=` to `type()`, and none of them have
# base classes with `__init_subclass__` methods),
# but `type[Unknown]` would be better than `Unknown` here
#
# error: [no-matching-overload] "No overload of class `type` matches arguments"
reveal_type(type("Foo", (), {}, weird_other_arg=42))  # revealed: Unknown
# error: [no-matching-overload] "No overload of class `type` matches arguments"
reveal_type(type("Bar", (int,), {}, weird_other_arg=42))  # revealed: Unknown
# error: [no-matching-overload] "No overload of class `type` matches arguments"
reveal_type(type("Baz", (), {}, metaclass=type))  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:434 on 2026-01-11 11:20_

I still think this should cause us to emit a diagnostic. However, having seen the ecosystem report, I think perhaps it should be a different error code to `unsupported-base` (maybe `unsupported-dynamic-base`?). It seems fairly common for users to create dynamic classes with `type[]` types as a base class, and the whole point of dynamic classes is after all that you can create them... dynamically. Making it a different error code to the one we use for unsupported bases in static class definitions will allow users to switch off only the diagnostic regarding dynamic classes, if they find it really noisy. (And maybe we should actually have it disabled by default, since it's quite strict.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:521 on 2026-01-11 11:23_

It might be fun to add this test too:

```py
def make_classes(name1: str, name2: str):
    cls1 = type(name1, (), {})
    cls2 = type(name2, (), {})

    def inner(x: cls1): ...

    # error: [invalid-argument-type] "Argument to function `inner` is incorrect: Expected `main.<locals of function 'make_classes'>.<unknown> @ main.py:2`, found `main.<locals of function 'make_classes'>.<unknown> @ main.py:3`"
    inner(cls2())
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:565 on 2026-01-11 11:24_

```suggestion
When `bases` is a module-level variable holding a tuple of class literals, we can extract the base
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:716 on 2026-01-11 11:40_

It seems like it is _possible_ to create an empty enum using `type()` if you try realy really hard. But yeah, we don't need to support this 😆

```pycon
>>> import enum as e
>>> type("E", (e.Enum,), e.EnumDict())
<enum 'E'>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:702 on 2026-01-11 11:43_

It doesn't need to be a keyword argument. Even if it's a positional-or-keyword argument in the `__init_subclass__` signature, that makes it a required keyword argument when subclassing that class (because there's no other way to pass arguments to a superclass's `__init_subclass__` method except by passing keyword arguments in the class statement or a dynamic `type()` call).

```suggestion
When a base class defines `__init_subclass__` with required arguments, those should be
passed to `type()`. This is not yet supported:

```py
class Base:
    def __init_subclass__(cls, required_arg: str, **kwargs):
        super().__init_subclass__(**kwargs)
        cls.config = required_arg
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1 on 2026-01-11 11:45_

If you do it as

```py
from dataclasses import dataclass

X = dataclass(type("X", (), {}))
```

do we store a `Definition` for the `X` class there? I think it's okay if we don't (this should come up very rarely), I'm just curious

I guess ideally we'd add some dedicated tests for goto-definition in https://github.com/astral-sh/ruff/blob/main/crates/ty_ide/src/goto_type_definition.rs, which would include edge cases like this, but it's fine for that to be a followup. I realise that this PR is dragging on a bit!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:931 on 2026-01-11 11:49_

```suggestion
# TODO: these should pass -- namespace dict attributes are not yet available for autocomplete
static_assert(has_member(DynamicWithDict, "custom_attr"))  # error: [static-assert-error]
static_assert(has_member(DynamicWithDict(), "custom_attr"))  # error: [static-assert-error]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:966 on 2026-01-11 11:50_

```suggestion
# TODO: these should pass; instance members should be available
static_assert(has_member(instance, "base_attr"))  # error: [static-assert-error]
static_assert(has_member(instance, "__repr__"))  # error: [static-assert-error]
static_assert(has_member(instance, "__hash__"))  # error: [static-assert-error]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:598 on 2026-01-11 11:52_

I would also be interested in some tests where the `*args` and/or `**kwargs` arguments "fill up" an unknown number of parameters in the `type()` call, e.g.

```py
def f(*args, **kwargs):
    A = type(*args, **kwargs)
    reveal_type(A)

    B = type("B", *args, **kwargs)
    reveal_type(B)

    C = type("C", (), *args, **kwargs)
    reveal_type(C)

    D = type("D", (), {}, **kwargs)
    reveal_type(D)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:605 on 2026-01-11 12:00_

this doesn't look correct -- I think we should do something like this here:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 3ab1659f9e..48c4049704 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -596,12 +596,11 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns the metaclass instance type for this class.
+    /// Return a type representing "the set of all instances of the metaclass of this class".
     pub(crate) fn metaclass_instance_type(self, db: &'db dyn Db) -> Type<'db> {
-        match self {
-            Self::Static(class) => class.metaclass_instance_type(db),
-            Self::Dynamic(class) => class.metaclass(db),
-        }
+        self.metaclass(db)
+            .to_instance(db)
+            .expect("`Type::to_instance()` should always return `Some()` when called on the type of a metaclass")
     }
 
     /// Returns whether this class is type-check only.
@@ -2585,14 +2584,6 @@ impl<'db> StaticClassLiteral<'db> {
             .unwrap_or_else(|_| SubclassOfType::subclass_of_unknown())
     }
 
-    /// Return a type representing "the set of all instances of the metaclass of this class".
-    pub(super) fn metaclass_instance_type(self, db: &'db dyn Db) -> Type<'db> {
-        self
-            .metaclass(db)
-            .to_instance(db)
-            .expect("`Type::to_instance()` should always return `Some()` when called on the type of a metaclass")
-    }
-
     /// Return the metaclass of this class, or an error if the metaclass cannot be inferred.
     #[salsa::tracked(cycle_initial=try_metaclass_cycle_initial,
         heap_size=ruff_memory_usage::heap_size,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:634 on 2026-01-11 12:02_

It's fine not to support dynamic classes being marked as deprecated, but in that case we should probably emit a diagnostic if somebody tries to do something like

```py
from warnings import deprecated

X = deprecated(type("X", (), {}))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:641 on 2026-01-11 12:02_

same here, I guess ideally we'd emit a diagnostic for this if we don't want to support it

```py
from typing import final

X = final(type("X", (), {}))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-11 12:06_

We should also emit a diagnostic (but don't currently on this branch) if you try to use an `@final` class as the base class for a dynamic class, e.g.

```py
from typing import final

@final
class Base: ...

type("X", (Base,), {})
```

This is maybe a counter-argument to https://github.com/astral-sh/ruff/pull/22291/files#r2678657516 -- if we looped through all dynamic class definitions in `TypeInferenceBuilder::check_class_definitions` as well as all the static class definitions, it might have been harder to forget to implement this check... but I'm guess there's a lot of stuff in that method that currently assumes that we have a `StmtClassDef` AST node, so it may be that what you have right now is the best design.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1742 on 2026-01-11 12:09_

It seems like we should also emit a diagnostic for

```py
from ty_extensions import get_protocol_members

get_protocol_members(type("X", (), {}))
```

?

But this isn't high-priority, since `get_protocol_members` is mostly an internal debugging tool

---

_Comment by @AlexWaygood on 2026-01-11 13:13_

@charliermarsh -- I pushed a commit fixing up and simplifying some of the `type()` expression parsing in `infer/builder.rs`, hope that's okay!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:668 on 2026-01-11 13:18_

Is the reason why this is a separate struct to `MroError` is because the kinds of errors that could occur for dynamic class definitions is only a subset of the kinds of errors that could occur for static class definitions? If so, maybe it's worth saying that in the struct doc-comment?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:682 on 2026-01-11 13:19_

```suggestion
/// These mirror the relevant variants from `MroErrorKind` for static classes.
```

---

_@AlexWaygood reviewed on 2026-01-11 13:22_

---

_Comment by @AlexWaygood on 2026-01-11 13:24_

From my perspective, I think this is basically ready to go now! My remaining concerns are https://github.com/astral-sh/ruff/pull/22291#discussion_r2677500504 (which I'd like @MichaReiser's or @carljm's opinion on) and https://github.com/astral-sh/ruff/pull/22291#discussion_r2679453580.

---

_@charliermarsh reviewed on 2026-01-11 13:42_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-11 13:42_

I believe I did this in https://github.com/astral-sh/ruff/pull/22499. I can revisit the approach in that review (RE whether it's worth using check_class_definitions), if that works?

---

_@AlexWaygood reviewed on 2026-01-11 13:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1 on 2026-01-11 13:48_

yeah sure!

---

_@AlexWaygood reviewed on 2026-01-11 14:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:638 on 2026-01-11 14:08_

```suggestion
    # TODO: `type[Unknown]` would cause fewer false positives
    reveal_type(B)  # revealed: <class 'str'>

    # Has string and tuple, but unknown additional args
    C = type("C", (), *args, **kwargs)
    # TODO: `type[Unknown]` would cause fewer false positives
    reveal_type(C)  # revealed: type

    # All three positional args provided, only **kwargs unknown
    D = type("D", (), {}, **kwargs)
    # TODO: `type[Unknown]` would cause fewer false positives
    reveal_type(D)  # revealed: type
```

---

_@AlexWaygood reviewed on 2026-01-11 14:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:628 on 2026-01-11 14:28_

I think here as well, `type[Unknown]` would cause fewer false positives (so I'd add a TODO here) -- it's ambiguous here whether we should pick the first overload (which would lead to us inferring `<class 'str'>` or the second overload (which would lead to us inferring a dynamic class-literal type). But either `<class 'str'>` or a dynamic class-literal type are both assignable to `type[Unknown]`, and it's a forgiving type that allows you to access most attributes on it.

---

_@charliermarsh reviewed on 2026-01-11 14:54_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:628 on 2026-01-11 14:54_

(Adding TODOs, going to tackle these in a separate PR.)

---
