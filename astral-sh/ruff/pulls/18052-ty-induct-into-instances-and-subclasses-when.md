```yaml
number: 18052
title: "[ty] Induct into instances and subclasses when finding and applying generics"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/recurse-into-generics
created_at: 2025-05-12T18:39:29Z
updated_at: 2025-05-13T01:53:14Z
url: https://github.com/astral-sh/ruff/pull/18052
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Induct into instances and subclasses when finding and applying generics

---

_Pull request opened by @dcreager on 2025-05-12 18:39_

We were not inducting into instance types and subclass-of types when looking for legacy typevars, nor when apply specializations.

This addresses https://github.com/astral-sh/ruff/pull/17832#discussion_r2081502056

```py
from __future__ import annotations
from typing import TypeVar, Any, reveal_type

S = TypeVar("S")

class Foo[T]:
    def method(self, other: Foo[S]) -> Foo[T | S]: ...  # type: ignore[invalid-return-type]

def f(x: Foo[Any], y: Foo[Any]):
    reveal_type(x.method(y))  # revealed: `Foo[Any | S]`, but should be `Foo[Any]`
```

We were not detecting that `S` made `method` generic, since we were not finding it when searching the function signature for legacy typevars.

---

_Review requested from @carljm by @dcreager on 2025-05-12 18:39_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-12 18:39_

---

_Review requested from @sharkdp by @dcreager on 2025-05-12 18:39_

---

_Label `internal` added by @dcreager on 2025-05-12 18:39_

---

_Label `ty` added by @dcreager on 2025-05-12 18:39_

---

_Comment by @github-actions[bot] on 2025-05-12 18:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[type-assertion-failure] tests/type_tests.py:49:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> _GeneratorContextManager[None]`
+ error[type-assertion-failure] tests/type_tests.py:49:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> _GeneratorContextManager[None, None, None]`

async-utils (https://github.com/mikeshardmind/async-utils)
- error[invalid-return-type] src/async_utils/bg_loop.py:64:16: Return type does not match returned value: Expected `Future[T]`, found `Future[_T]`
- error[invalid-argument-type] src/async_utils/bg_loop.py:81:42: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[_T]`
- error[invalid-argument-type] src/async_utils/corofunc_cache.py:117:50: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-argument-type] src/async_utils/corofunc_cache.py:196:50: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-argument-type] src/async_utils/task_cache.py:176:43: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-return-type] src/async_utils/task_cache.py:177:24: Return type does not match returned value: Expected `Task[R]`, found `Task[_T]`
- error[invalid-argument-type] src/async_utils/task_cache.py:262:43: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-return-type] src/async_utils/task_cache.py:263:24: Return type does not match returned value: Expected `Task[R]`, found `Task[_T]`
- Found 20 diagnostics
+ Found 12 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[invalid-return-type] chess/engine.py:2909:16: Return type does not match returned value: Expected `MutableMapping[str, Option]`, found `_T`
- error[invalid-return-type] chess/engine.py:2916:16: Return type does not match returned value: Expected `Mapping[str, str]`, found `_T`
- error[invalid-return-type] chess/engine.py:2922:16: Return type does not match returned value: Expected `T`, found `_T`
- error[invalid-return-type] chess/engine.py:2928:16: Return type does not match returned value: Expected `None`, found `_T`
- error[invalid-return-type] chess/engine.py:2936:16: Return type does not match returned value: Expected `None`, found `_T`
- error[invalid-return-type] chess/engine.py:2942:16: Return type does not match returned value: Expected `None`, found `_T`
- error[invalid-return-type] chess/engine.py:2950:16: Return type does not match returned value: Expected `PlayResult`, found `_T`
- error[invalid-return-type] chess/engine.py:2964:16: Return type does not match returned value: Expected `InfoDict | list[Unknown]`, found `_T`
- error[invalid-argument-type] chess/engine.py:2972:43: Argument to bound method `__init__` is incorrect: Expected `AnalysisResult`, found `_T`
- error[invalid-return-type] chess/engine.py:2978:16: Return type does not match returned value: Expected `None`, found `_T`
- error[invalid-return-type] chess/engine.py:2984:16: Return type does not match returned value: Expected `None`, found `_T`
- error[invalid-return-type] chess/engine.py:3058:16: Return type does not match returned value: Expected `InfoDict`, found `_T`
- error[invalid-return-type] chess/engine.py:3065:16: Return type does not match returned value: Expected `list[Unknown]`, found `_T`
- error[invalid-return-type] chess/engine.py:3074:16: Return type does not match returned value: Expected `BestMove`, found `_T`
- error[invalid-return-type] chess/engine.py:3079:16: Return type does not match returned value: Expected `bool`, found `_T`
- error[invalid-return-type] chess/engine.py:3084:16: Return type does not match returned value: Expected `bool`, found `_T`
- error[invalid-return-type] chess/engine.py:3089:16: Return type does not match returned value: Expected `InfoDict`, found `_T`
- error[invalid-return-type] chess/engine.py:3094:16: Return type does not match returned value: Expected `InfoDict | None`, found `_T`
- error[invalid-return-type] chess/engine.py:3105:20: Return type does not match returned value: Expected `InfoDict`, found `_T`
- Found 58 diagnostics
+ Found 39 diagnostics

beartype (https://github.com/beartype/beartype)
- error[unsupported-operator] beartype/claw/_clawstate.py:214:16: Operator `not in` is not supported for types `str` and `_S`, in comparing `Literal["."]` with `Unknown | _S`
- Found 555 diagnostics
+ Found 554 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[invalid-argument-type] pyinstrument/__main__.py:323:76: Argument to function `file_is_a_tty` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/__main__.py:392:95: Argument to function `file_is_a_tty` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/__main__.py:513:47: Argument to function `file_supports_unicode` is incorrect: Expected `IO[AnyStr]`, found `TextIO`
- error[invalid-argument-type] pyinstrument/__main__.py:514:43: Argument to function `file_supports_color` is incorrect: Expected `IO[AnyStr]`, found `TextIO`
- error[no-matching-overload] pyinstrument/__main__.py:612:5: No overload of bound method `sort` matches arguments
- error[invalid-argument-type] pyinstrument/context_manager.py:92:43: Argument to function `file_supports_color` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/context_manager.py:93:47: Argument to function `file_supports_unicode` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[unresolved-attribute] pyinstrument/magic/magic.py:286:20: Type `_T` has no attribute `result`
- error[invalid-argument-type] pyinstrument/profiler.py:312:45: Argument to function `file_supports_unicode` is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/profiler.py:314:41: Argument to function `file_supports_color` is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
- Found 100 diagnostics
+ Found 90 diagnostics

git-revise (https://github.com/mystor/git-revise)
- warning[possibly-unbound-attribute] gitrevise/merge.py:90:20: Attribute `decode` on type `Unknown | _S` is possibly unbound
- Found 11 diagnostics
+ Found 10 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[invalid-argument-type] more_itertools/recipes.py:675:29: Argument to bound method `sample` is incorrect: Expected `Sequence[_T] | AbstractSet[_T]`, found `range`
- Found 51 diagnostics
+ Found 50 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[unresolved-attribute] docs/conf.py:19:20: Type `Self` has no attribute `resolve`
- Found 603 diagnostics
+ Found 602 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- error[no-matching-overload] test_htmlgen/element.py:125:9: No overload of bound method `sort` matches arguments
- error[no-matching-overload] test_htmlgen/element.py:153:9: No overload of bound method `sort` matches arguments
- Found 28 diagnostics
+ Found 26 diagnostics

kopf (https://github.com/nolar/kopf)
- error[invalid-return-type] kopf/_core/actions/lifecycles.py:40:12: Return type does not match returned value: Expected `Sequence[Handler]`, found `list[_T] | list[Unknown]`
- error[invalid-argument-type] kopf/_core/actions/lifecycles.py:40:26: Argument to bound method `sample` is incorrect: Expected `Sequence[_T]`, found `Sequence[Handler] & ~AlwaysFalsy`
- error[unsupported-operator] kopf/_core/reactor/observation.py:221:9: Operator `|` is unsupported between objects of type `Unknown | frozenset[Selector]` and `Unknown | frozenset[Selector]`
- error[unsupported-operator] kopf/_core/reactor/observation.py:227:9: Operator `|` is unsupported between objects of type `Unknown | frozenset[Selector]` and `Unknown | frozenset[Selector]`
- error[unsupported-operator] kopf/_core/reactor/processing.py:161:9: Operator `|` is unsupported between objects of type `Unknown | set[Unknown | tuple[@Todo(Generic tuple specializations), ...]]` and `Unknown | set[Unknown | tuple[@Todo(Generic tuple specializations), ...]]`
- error[unresolved-attribute] kopf/cli.py:38:35: Type `_EnumMemberT` has no attribute `name`
- Found 191 diagnostics
+ Found 185 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- error[invalid-parameter-default] koda_validate/decimal.py:33:9: Default value of type `Coercer[A]` is not assignable to annotated parameter type `Coercer[Decimal] | None`
- error[invalid-parameter-default] koda_validate/time.py:30:9: Default value of type `Coercer[A]` is not assignable to annotated parameter type `Coercer[date] | None`
- error[invalid-parameter-default] koda_validate/time.py:59:9: Default value of type `Coercer[A]` is not assignable to annotated parameter type `Coercer[datetime] | None`
- error[invalid-parameter-default] koda_validate/uuid.py:33:9: Default value of type `Coercer[A]` is not assignable to annotated parameter type `Coercer[UUID] | None`
- Found 54 diagnostics
+ Found 50 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[unresolved-attribute] src/graphql/type/validate.py:119:42: Type `_EnumMemberT` has no attribute `value`
- error[unresolved-attribute] src/graphql/type/validate.py:122:49: Type `_EnumMemberT` has no attribute `QUERY`
- error[invalid-argument-type] src/graphql/type/validate.py:127:57: Argument to function `get_operation_type_node` is incorrect: Expected `OperationType`, found `_EnumMemberT`
- error[unresolved-attribute] src/graphql/utilities/extend_schema.py:246:43: Type `_EnumMemberT` has no attribute `value`
- Found 413 diagnostics
+ Found 409 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[unresolved-attribute] src/check_jsonschema/cli/main_command.py:146:24: Type `_EnumMemberT` has no attribute `value`
- error[unresolved-attribute] src/check_jsonschema/cli/main_command.py:155:24: Type `_EnumMemberT` has no attribute `value`
- Found 67 diagnostics
+ Found 65 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-argument-type] strawberry/experimental/pydantic/object_type.py:176:13: Argument to function `ensure_all_auto_fields_in_pydantic` is incorrect: Expected `set[str]`, found `set[Unknown | _S] | set[Unknown]`
- error[invalid-argument-type] strawberry/experimental/pydantic/object_type.py:193:17: Argument to function `_build_dataclass_creation_fields` is incorrect: Expected `set[str]`, found `set[Unknown | _S] | set[Unknown]`
+ warning[unused-ignore-comment] strawberry/federation/schema.py:274:46: Unused blanket `type: ignore` directive
- error[invalid-return-type] strawberry/schema/schema_converter.py:480:16: Return type does not match returned value: Expected `dict[str, GraphQLField]`, found `dict[str, FieldType]`
- error[invalid-return-type] strawberry/schema/schema_converter.py:490:16: Return type does not match returned value: Expected `dict[str, GraphQLInputField]`, found `dict[str, FieldType]`
- error[unresolved-attribute] strawberry/types/enum.py:120:22: Type `_EnumMemberT` has no attribute `value`
- error[unresolved-attribute] strawberry/types/enum.py:121:21: Type `_EnumMemberT` has no attribute `name`
- error[invalid-assignment] strawberry/types/type_resolver.py:74:5: Object of type `dict[_T, type[Any]]` is not assignable to `dict[str, type]`
+ error[invalid-assignment] strawberry/types/type_resolver.py:74:5: Object of type `dict[Unknown, type[Any]]` is not assignable to `dict[str, type]`
- Found 460 diagnostics
+ Found 455 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- warning[call-possibly-unbound-method] tests/extra_python_package/test_files.py:311:31: Method `__getitem__` of type `Unknown | _S` is possibly unbound
- Found 243 diagnostics
+ Found 242 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[non-subscriptable] examples/notebooks/TextCNN.ipynb:40:17: Cannot subscript object of type `_T` with no `__getitem__` method
- error[non-subscriptable] examples/notebooks/TextCNN.ipynb:41:17: Cannot subscript object of type `_T` with no `__getitem__` method
- error[unresolved-attribute] ignite/handlers/time_profilers.py:256:22: Type `_EnumMemberT` has no attribute `name`
- Found 2280 diagnostics
+ Found 2277 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/_internal/_discriminated_union.py:307:20: Return type does not match returned value: Expected `list[str | int]`, found `list[SupportsRichComparisonT]`
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:1239:103: Argument to bound method `_from_decorator` is incorrect: Expected `Decorator[FieldValidatorDecoratorInfo]`, found `Decorator[FieldDecoratorInfoType]`
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:1239:103: Argument to bound method `_from_decorator` is incorrect: Expected `Decorator[FieldValidatorDecoratorInfo]`, found `Decorator[FieldDecoratorInfoType]`
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:1239:103: Argument to bound method `_from_decorator` is incorrect: Expected `Decorator[FieldValidatorDecoratorInfo]`, found `Decorator[FieldDecoratorInfoType]`
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:1239:103: Argument to bound method `_from_decorator` is incorrect: Expected `Decorator[FieldValidatorDecoratorInfo]`, found `Decorator[FieldDecoratorInfoType]`
- error[invalid-assignment] pydantic/deprecated/copy_internals.py:213:9: Object of type `@Todo(map_with_boundness: intersections with negative contributions) | set[Unknown | _S]` is not assignable to `AbstractSet[str]`
+ warning[unused-ignore-comment] pydantic/experimental/pipeline.py:293:62: Unused blanket `type: ignore` directive
- error[no-matching-overload] pydantic/experimental/pipeline.py:285:16: No overload of bound method `constrain` matches arguments
- error[no-matching-overload] pydantic/experimental/pipeline.py:288:16: No overload of bound method `constrain` matches arguments
- error[no-matching-overload] pydantic/experimental/pipeline.py:314:16: No overload of bound method `constrain` matches arguments
- error[invalid-return-type] pydantic/json_schema.py:2461:12: Return type does not match returned value: Expected `tuple[dict[tuple[type[BaseModel] | type[PydanticDataclass], @Todo(Inference of subscript on special form)], dict[str, Any]], dict[str, Any]]`, found `tuple[dict[tuple[JsonSchemaKeyT, Unknown], Unknown | dict[str, Any]], dict[Unknown, Unknown]]`
+ error[invalid-return-type] pydantic/json_schema.py:2461:12: Return type does not match returned value: Expected `tuple[dict[tuple[type[BaseModel] | type[PydanticDataclass], @Todo(Inference of subscript on special form)], dict[str, Any]], dict[str, Any]]`, found `tuple[dict[tuple[Unknown, Unknown], Unknown | dict[str, Any]], dict[Unknown, Unknown]]`
+ error[invalid-return-type] pydantic/type_adapter.py:727:16: Return type does not match returned value: Expected `tuple[dict[tuple[JsonSchemaKeyT, Unknown], Unknown | dict[str, Any]], Unknown | dict[str, Any]]`, found `tuple[dict[tuple[Unknown, Unknown], Unknown | dict[str, Any]], dict[Unknown, Unknown]]`
- error[unresolved-attribute] pydantic/version.py:49:20: Type `Self` has no attribute `resolve`
- Found 787 diagnostics
+ Found 779 diagnostics

black (https://github.com/psf/black)
- error[unresolved-attribute] src/black/__init__.py:256:24: Type `_EnumMemberT` has no attribute `name`
- error[unresolved-attribute] src/black/__init__.py:329:24: Type `_EnumMemberT` has no attribute `name`
- error[call-non-callable] src/black/__init__.py:1453:61: Method `__getitem__` of type `bound method dict[TargetVersion, set[Feature]].__getitem__(key: TargetVersion, /) -> set[Feature]` is not callable on object of type `dict[TargetVersion, set[Feature]]`
- error[unresolved-attribute] src/black/files.py:188:32: Type `_EnumMemberT` has no attribute `value`
- error[unsupported-operator] src/black/handle_ipynb_magics.py:91:16: Operator `|` is unsupported between objects of type `Unknown | frozenset[Unknown]` and `set[str]`
- Found 146 diagnostics
+ Found 141 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- error[invalid-argument-type] pyi.py:492:29: Argument to bound method `contains_in_bases` is incorrect: Expected `AbstractSet[str]`, found `Unknown | frozenset[Unknown | _S]`
- error[invalid-argument-type] pyi.py:497:34: Argument to bound method `contains_in_bases` is incorrect: Expected `AbstractSet[str]`, found `Unknown | frozenset[Unknown | _S]`
- Found 27 diagnostics
+ Found 25 diagnostics

nox (https://github.com/wntrblm/nox)
- error[invalid-return-type] nox/__init__.py:46:12: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[call-non-callable] nox/_resolver.py:165:16: Method `__getitem__` of type `Unknown | (bound method dict[_T, Literal[False]].__getitem__(key: _T, /) -> Literal[False])` is not callable on object of type `Unknown | dict[_T, Literal[False]]`
- error[call-non-callable] nox/_resolver.py:166:13: Method `__getitem__` of type `Unknown | (bound method dict[_T, Literal[False]].__getitem__(key: _T, /) -> Literal[False])` is not callable on object of type `Unknown | dict[_T, Literal[False]]`
- Found 42 diagnostics
+ Found 39 diagnostics

flake8 (https://github.com/pycqa/flake8)
- error[invalid-return-type] src/flake8/processor.py:273:16: Return type does not match returned value: Expected `dict[int, str]`, found `dict[_T, LiteralString]`
+ error[invalid-return-type] src/flake8/processor.py:273:16: Return type does not match returned value: Expected `dict[int, str]`, found `dict[Unknown, LiteralString]`
- error[invalid-return-type] src/flake8/statistics.py:23:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- Found 47 diagnostics
+ Found 46 diagnostics

trio (https://github.com/python-trio/trio)
- error[unsupported-operator] src/trio/_tests/test_exports.py:372:13: Operator `-` is unsupported between objects of type `set[str]` and `set[Unknown | _S] | Unknown`
- error[unsupported-operator] src/trio/_tests/test_exports.py:385:28: Operator `-` is unsupported between objects of type `set[str]` and `set[Unknown | _S] | Unknown`
- error[invalid-argument-type] src/trio/_tests/test_highlevel_open_tcp_listeners.py:341:9: Argument is incorrect: Expected `dict[AddressFamily, int]`, found `dict[_T, int]`
- warning[unused-ignore-comment] src/trio/_tests/test_highlevel_ssl_helpers.py:96:65: Unused blanket `type: ignore` directive
- warning[possibly-unbound-attribute] src/trio/_tests/test_path.py:124:12: Attribute `__name__` on type `Unknown | ((...) -> Awaitable[PathT])` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_path.py:124:12: Attribute `__name__` on type `Unknown | ((...) -> Awaitable[Unknown])` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_path.py:125:12: Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[PathT])` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_path.py:125:12: Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[Unknown])` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_path.py:128:12: Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[PathT])` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_path.py:128:12: Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[Unknown])` is possibly unbound
- error[invalid-argument-type] src/trio/_tests/test_ssl.py:1337:13: Argument to bound method `__init__` is incorrect: Expected `Listener[T_Stream]`, found `SocketListener`
- error[no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:394:10: No overload of bound method `__init__` matches arguments
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:42:5: Actual type `Self` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:42:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:98:5: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:103:5: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:119:5: No overload of bound method `__init__` matches arguments
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:92:51: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:95:49: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:99:57: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:102:48: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:106:67: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:107:69: Unused blanket `type: ignore` directive
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:223:5: Actual type `Unknown | ((BaseExceptionGroup[BaseExcT_co], /) -> bool) | None` is not the same as asserted type `((BaseExceptionGroup[ValueError], /) -> bool) | None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:223:5: Actual type `Unknown | ((BaseExceptionGroup[Unknown], /) -> bool) | None` is not the same as asserted type `((BaseExceptionGroup[ValueError], /) -> bool) | None`
+ error[invalid-super-argument] src/trio/testing/_raises_group.py:543:9: `RaisesGroup[ExcT_1 | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not an instance or subclass of `<class 'RaisesGroup'>` in `super(<class 'RaisesGroup'>, RaisesGroup[ExcT_1 | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]])` call
- warning[possibly-unbound-attribute] src/trio/testing/_raises_group.py:775:12: Attribute `startswith` on type `str | None | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/testing/_raises_group.py:775:12: Attribute `startswith` on type `str | None` is possibly unbound
- error[invalid-argument-type] src/trio/testing/_raises_group.py:776:51: Argument to function `indent` is incorrect: Expected `str`, found `str | None | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[invalid-argument-type] src/trio/testing/_raises_group.py:776:51: Argument to function `indent` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] src/trio/testing/_raises_group.py:1019:70: Argument to function `possible_match` is incorrect: Expected `set[int] | None`, found `set[int | _S] | set[Unknown | _S]`
+ error[invalid-argument-type] src/trio/testing/_raises_group.py:1019:70: Argument to function `possible_match` is incorrect: Expected `set[int] | None`, found `set[int | Unknown] | set[Unknown]`
- Found 1079 diagnostics
+ Found 1077 diagnostics

alerta (https://github.com/alerta/alerta)
- error[call-non-callable] tests/helpers/utils.py:25:24: Method `__getitem__` of type `bound method _Environ[str].__getitem__(key: str) -> str` is not callable on object of type `_Environ[str]`
- Found 483 diagnostics
+ Found 482 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[no-matching-overload] mkosi/__init__.py:431:27: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/__init__.py:431:27: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/__init__.py:433:62: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/__init__.py:433:62: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/__init__.py:436:59: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/__init__.py:436:59: No overload of bound method `get` matches arguments
- error[unsupported-operator] mkosi/__init__.py:687:25: Operator `|` is unsupported between objects of type `dict[str | _T1, str | _T2]` and `dict[str, str]`
+ error[invalid-argument-type] mkosi/__init__.py:687:21: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-return-type] mkosi/__init__.py:1960:16: Return type does not match returned value: Expected `list[Path]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] mkosi/__init__.py:1970:16: Return type does not match returned value: Expected `list[Path]`, found `list[SupportsRichComparisonT]`
- error[invalid-argument-type] mkosi/__init__.py:4191:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | _T1, str | _T2] | Unknown`
+ error[invalid-argument-type] mkosi/__init__.py:4191:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown] | Unknown`
- error[unsupported-operator] mkosi/__init__.py:4383:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
- error[unsupported-operator] mkosi/__init__.py:4424:13: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
+ error[invalid-argument-type] mkosi/__init__.py:4383:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
+ error[invalid-argument-type] mkosi/__init__.py:4424:9: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[unresolved-attribute] mkosi/bootloader.py:261:12: Type `T` has no attribute `is_symlink`
- error[unresolved-attribute] mkosi/bootloader.py:261:31: Type `T` has no attribute `readlink`
- error[unresolved-attribute] mkosi/bootloader.py:262:93: Type `T` has no attribute `readlink`
- error[invalid-return-type] mkosi/bootloader.py:265:16: Return type does not match returned value: Expected `Path | None`, found `T`
- error[unsupported-operator] mkosi/burn.py:41:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
+ error[invalid-argument-type] mkosi/burn.py:41:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-return-type] mkosi/config.py:766:12: Return type does not match returned value: Expected `list[Path]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] mkosi/config.py:1684:16: Return type does not match returned value: Expected `list[str]`, found `list[T]`
- error[no-matching-overload] mkosi/distributions/__init__.py:168:15: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/distributions/__init__.py:169:20: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/distributions/__init__.py:170:18: No overload of bound method `get` matches arguments
- error[no-matching-overload] mkosi/distributions/__init__.py:171:24: No overload of bound method `get` matches arguments
- error[invalid-argument-type] mkosi/initrd.py:396:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | _T1, str | _T2]`
+ error[invalid-argument-type] mkosi/initrd.py:396:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-return-type] mkosi/kmod.py:133:12: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[unsupported-operator] mkosi/kmod.py:365:26: Operator `|` is unsupported between objects of type `set[Path] | set[Unknown]` and `set[Path]`
+ error[invalid-argument-type] mkosi/qemu.py:1002:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
+ error[invalid-argument-type] mkosi/qemu.py:1061:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
+ error[invalid-argument-type] mkosi/qemu.py:1090:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[unsupported-operator] mkosi/qemu.py:1002:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
- error[unsupported-operator] mkosi/qemu.py:1061:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
- error[unsupported-operator] mkosi/qemu.py:1090:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
- error[unresolved-attribute] mkosi/qemu.py:1136:12: Type `_EnumMemberT` has no attribute `open`
- error[unresolved-attribute] mkosi/qemu.py:1138:12: Type `_EnumMemberT` has no attribute `feature`
- error[unresolved-attribute] mkosi/qemu.py:1138:60: Type `_EnumMemberT` has no attribute `available`
- error[unsupported-operator] mkosi/qemu.py:1577:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
- error[unsupported-operator] mkosi/qemu.py:1641:13: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
+ error[invalid-argument-type] mkosi/qemu.py:1641:9: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[unsupported-operator] mkosi/sysupdate.py:81:17: Operator `|` is unsupported between objects of type `_Environ[str]` and `dict[str, str]`
+ error[invalid-argument-type] mkosi/sysupdate.py:81:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[unresolved-attribute] mkosi/user.py:56:80: Type `Self` has no attribute `stat`
- Found 352 diagnostics
+ Found 327 diagnostics

isort (https://github.com/pycqa/isort)
- error[invalid-argument-type] isort/settings.py:663:52: Argument to bound method `union` is incorrect: Expected `Iterable[_S]`, found `Any | None`
+ error[invalid-argument-type] isort/settings.py:663:52: Argument to bound method `union` is incorrect: Expected `Iterable[Unknown]`, found `Any | None`

poetry (https://github.com/python-poetry/poetry)
- error[unresolved-attribute] src/poetry/console/commands/source/add.py:45:27: Type `_EnumMemberT` has no attribute `name`
- error[invalid-return-type] src/poetry/publishing/uploader.py:65:16: Return type does not match returned value: Expected `list[Path]`, found `list[SupportsRichComparisonT]`
- Found 1056 diagnostics
+ Found 1054 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[invalid-return-type] expression/collections/array.py:308:16: Return type does not match returned value: Expected `TypedArray[tuple[int, _TSource]]`, found `TypedArray[_TSource]`
+ error[invalid-return-type] expression/collections/array.py:548:12: Return type does not match returned value: Expected `TypedArray[tuple[int, _TSource]]`, found `TypedArray[tuple[int, Unknown]]`
- error[invalid-return-type] expression/collections/block.py:222:16: Return type does not match returned value: Expected `Block[tuple[int, _TSource]]`, found `Block[_TSource]`
- error[invalid-return-type] expression/collections/block.py:501:16: Return type does not match returned value: Expected `Block[tuple[_TSource, _TResult]]`, found `Block[_TSource]`
+ error[invalid-return-type] expression/collections/block.py:696:12: Return type does not match returned value: Expected `Block[tuple[int, _TSource]]`, found `Block[tuple[int, Unknown]]`
- error[invalid-argument-type] expression/collections/map.py:59:40: Argument to function `change` is incorrect: Expected `(Option[Value], /) -> Option[Value]`, found `(Option[_Value], /) -> Option[_Value]`
+ error[invalid-return-type] expression/collections/block.py:1036:12: Return type does not match returned value: Expected `Block[tuple[_TSource, _TResult]]`, found `Block[tuple[Unknown, Unknown]]`
- error[invalid-return-type] expression/collections/map.py:143:16: Return type does not match returned value: Expected `Block[tuple[_Key, _Value]]`, found `Block[tuple[Key, Value]]`
+ error[invalid-return-type] expression/collections/map.py:143:16: Return type does not match returned value: Expected `Block[tuple[_Key, _Value]]`, found `Block[tuple[Unknown, Unknown]]`
- error[invalid-argument-type] expression/collections/map.py:155:26: Argument to bound method `append` is incorrect: Expected `_Value`, found `Value`
- error[invalid-return-type] expression/collections/map.py:161:16: Return type does not match returned value: Expected `Option[_Value]`, found `Option[Value]`
- error[invalid-return-type] expression/collections/map.py:164:16: Return type does not match returned value: Expected `Option[_Result]`, found `Option[Result]`
- error[invalid-argument-type] expression/collections/map.py:164:33: Argument to function `try_pick` is incorrect: Expected `(Unknown, Unknown, /) -> Option[Result]`, found `(_Key, _Value, /) -> Option[_Result]`
+ error[invalid-argument-type] expression/collections/map.py:177:25: Argument to function `of_block` is incorrect: Expected `Block[tuple[Unknown, Unknown]]`, found `Block[tuple[_Key, _Value]]`
- error[invalid-return-type] expression/collections/map.py:186:16: Return type does not match returned value: Expected `Map[_Key_, _Result]`, found `Map[_Key, _Value]`
- error[invalid-argument-type] expression/collections/map.py:186:24: Argument to function `of_list` is incorrect: Expected `list[tuple[_Key, _Value]]`, found `list[tuple[_Key_, _Result]]`
+ error[invalid-argument-type] expression/collections/map.py:186:24: Argument to function `of_list` is incorrect: Expected `list[tuple[Unknown, Unknown]]`, found `list[tuple[_Key_, _Result]]`
- error[invalid-return-type] expression/collections/map.py:198:16: Return type does not match returned value: Expected `Map[_Key_, _Result]`, found `Map[_Key, _Value]`
- error[invalid-argument-type] expression/collections/map.py:454:32: Argument to function `of_list` is incorrect: Expected `Block[tuple[Key, Value]]`, found `Block[tuple[_Key, _Value]]`
+ error[invalid-argument-type] expression/collections/map.py:454:32: Argument to function `of_list` is incorrect: Expected `Block[tuple[Unknown, Unknown]]`, found `Block[tuple[_Key, _Value]]`
+ error[invalid-return-type] expression/collections/map.py:466:12: Return type does not match returned value: Expected `Block[tuple[_Key, _Value]]`, found `Block[tuple[Unknown, Unknown]]`
+ error[invalid-return-type] expression/collections/maptree.py:438:24: Return type does not match returned value: Expected `Block[tuple[Key, Value]]`, found `Block[tuple[Unknown, Unknown]]`
+ error[invalid-argument-type] expression/collections/maptree.py:438:47: Argument to function `loop` is incorrect: Expected `Block[tuple[Unknown, Unknown]]`, found `Block[tuple[Key, Value]]`
+ error[invalid-return-type] expression/collections/maptree.py:444:12: Return type does not match returned value: Expected `Block[tuple[Key, Value]]`, found `Block[tuple[Unknown, Unknown]]`
- error[too-many-positional-arguments] expression/effect/async_result.py:135:15: Too many positional arguments to class `AsyncResultBuilder`: expected 1, got 2
- error[too-many-positional-arguments] expression/effect/async_result.py:138:23: Too many positional arguments to class `AsyncResultBuilder`: expected 1, got 2
- error[invalid-return-type] expression/effect/option.py:71:16: Return type does not match returned value: Expected `Option[_TSource]`, found `Option[_TResult]`
- error[invalid-return-type] expression/effect/result.py:76:16: Return type does not match returned value: Expected `Result[_TSource, _TError]`, found `Result[_TResult, _TError]`
- error[too-many-positional-arguments] expression/effect/result.py:99:18: Too many positional arguments to class `ResultBuilder`: expected 1, got 2
- error[invalid-return-type] expression/extra/parser.py:247:12: Return type does not match returned value: Expected `Parser[_C]`, found `Parser[_B]`
+ error[invalid-return-type] expression/extra/parser.py:76:16: Return type does not match returned value: Expected `Parser[Option[_A]]`, found `Parser[Option[Unknown]]`
- error[invalid-argument-type] expression/extra/parser.py:247:18: Argument to function `apply` is incorrect: Expected `Parser[(_A, /) -> _B]`, found `Parser[_B]`
- error[invalid-argument-type] expression/extra/parser.py:247:24: Argument to function `apply` is incorrect: Expected `Parser[(_A, /) -> _B]`, found `Parser[(_A, /) -> (_B, /) -> _C]`
+ error[invalid-argument-type] expression/extra/parser.py:247:24: Argument to function `apply` is incorrect: Expected `Parser[(Unknown, /) -> Unknown]`, found `Parser[(_A, /) -> (_B, /) -> _C]`
- error[invalid-argument-type] expression/extra/parser.py:247:42: Argument to function `apply` is incorrect: Expected `Parser[_A]`, found `Parser[_B]`
+ error[invalid-return-type] expression/extra/parser.py:335:12: Return type does not match returned value: Expected `Parser[Block[_A]]`, found `Parser[Block[Unknown]]`
- error[invalid-argument-type] expression/extra/parser.py:325:34: Argument to bound method `ignore_then` is incorrect: Expected `Parser[_B]`, found `Parser[_A]`
- error[no-matching-overload] expression/extra/parser.py:330:12: No overload of bound method `starmap` matches arguments
- error[invalid-argument-type] expression/extra/parser.py:330:23: Argument to bound method `and_then` is incorrect: Expected `Parser[_B]`, found `Parser[Block[_A]]`
- error[invalid-argument-type] expression/extra/parser.py:330:28: Argument to function `many` is incorrect: Expected `Parser[_A]`, found `Parser[_B]`
- error[invalid-argument-type] expression/extra/parser.py:335:36: Argument to bound method `or_else` is incorrect: Expected `Parser[Block[_A]]`, found `Parser[Never]`
+ error[invalid-argument-type] expression/extra/parser.py:335:36: Argument to bound method `or_else` is incorrect: Expected `Parser[Block[Unknown]]`, found `Parser[Never]`
- error[invalid-argument-type] expression/extra/parser.py:339:16: Argument to function `many1` is incorrect: Expected `Parser[_A]`, found `Parser[str]`
- error[invalid-argument-type] expression/extra/parser.py:447:20: Argument to function `many1` is incorrect: Expected `Parser[_A]`, found `Unknown | Parser[str]`
- error[invalid-argument-type] expression/extra/parser.py:451:13: Argument to function `opt` is incorrect: Expected `Parser[_A]`, found `Parser[str]`
- error[invalid-argument-type] expression/extra/parser.py:470:20: Argument to function `many1` is incorrect: Expected `Parser[_A]`, found `Parser[str]`
- error[invalid-return-type] expression/extra/result/traversable.py:50:12: Return type does not match returned value: Expected `Result[Block[_TSource], _TError]`, found `Result[Block[_TResult], _TError]`
- error[invalid-argument-type] expression/extra/result/traversable.py:50:21: Argument to function `traverse` is incorrect: Expected `(Unknown, /) -> Result[_TResult, _TError]`, found `def identity(value: _A) -> _A`
+ error[invalid-argument-type] expression/extra/result/traversable.py:50:21: Argument to function `traverse` is incorrect: Expected `(Unknown, /) -> Result[Unknown, Unknown]`, found `def identity(value: _A) -> _A`
- error[invalid-argument-type] tests/test_array.py:396:22: Argument to bound method `collect` is incorrect: Expected `(int, /) -> TypedArray[_TResult]`, found `(int, /) -> TypedArray[int]`
- error[invalid-argument-type] tests/test_array.py:409:72: Argument to bound method `collect` is incorrect: Expected `(int, /) -> TypedArray[_TResult]`, found `(int, /) -> TypedArray[int]`
- error[invalid-argument-type] tests/test_array.py:420:72: Argument to bound method `collect` is incorrect: Expected `(int, /) -> TypedArray[_TResult]`, found `(int, /) -> TypedArray[int]`
- error[invalid-argument-type] tests/test_array.py:430:72: Argument to bound method `collect` is incorrect: Expected `(int, /) -> TypedArray[_TResult]`, found `(int, /) -> TypedArray[int]`
- error[invalid-argument-type] tests/test_block.py:374:22: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Block[_TResult]`, found `(int, /) -> Block[int]`
- error[invalid-argument-type] tests/test_block.py:387:72: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Block[_TResult]`, found `(int, /) -> Block[int]`
- error[invalid-argument-type] tests/test_block.py:398:72: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Block[_TResult]`, found `(int, /) -> Block[int]`
- error[invalid-argument-type] tests/test_block.py:408:72: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Block[_TResult]`, found `(int, /) -> Block[int]`
- error[invalid-argument-type] tests/test_parser.py:72:35: Argument to bound method `and_then` is incorrect: Expected `Parser[_B]`, found `Parser[str]`
- error[invalid-argument-type] tests/test_seq.py:360:27: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Seq[_TResult]`, found `(int, /) -> Seq[int]`
- error[invalid-argument-type] tests/test_seq.py:373:83: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Seq[_TResult]`, found `(int, /) -> Seq[int]`
- error[invalid-argument-type] tests/test_seq.py:384:83: Argument to bound method `collect` is incorrect: Expected `(int, /) -> Seq[_TResult]`, found `(int, /) -> Seq[int]`
- Found 334 diagnostics
+ Found 305 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[invalid-argument-type] porcupine/pluginloader.py:298:40: Argument to function `_decide_loading_order` is incorrect: Expected `dict[_T, set[_T]]`, found `dict[PluginInfo, set[PluginInfo]]`
+ error[invalid-argument-type] porcupine/pluginloader.py:298:40: Argument to function `_decide_loading_order` is incorrect: Expected `dict[Unknown, set[Unknown]]`, found `dict[PluginInfo, set[PluginInfo]]`
- error[invalid-argument-type] porcupine/pluginloader.py:298:55: Argument to function `_decide_loading_order` is incorrect: Expected `(Sequence[_T], /) -> None`, found `def _handle_circular_dependency(cycle: Sequence[PluginInfo]) -> None`
- error[unresolved-attribute] porcupine/pluginloader.py:299:48: Type `_T` has no attribute `status`
- error[invalid-return-type] porcupine/plugins/filemanager.py:159:20: Return type does not match returned value: Expected `Path | None`, found `Self`
- error[unresolved-attribute] porcupine/settings.py:965:64: Type `_EnumMemberT` has no attribute `name`
- error[invalid-return-type] porcupine/utils.py:106:20: Return type does not match returned value: Expected `Path`, found `Self`
- error[unresolved-attribute] porcupine/utils.py:109:30: Type `Self` has no attribute `glob`
- error[unresolved-attribute] porcupine/utils.py:110:30: Type `Self` has no attribute `glob`
- error[unresolved-attribute] porcupine/utils.py:111:30: Type `Self` has no attribute `glob`
- error[unresolved-attribute] porcupine/utils.py:112:30: Type `Self` has no attribute `glob`
- error[unresolved-attribute] porcupine/utils.py:113:30: Type `Self` has no attribute `glob`
- error[invalid-return-type] porcupine/utils.py:117:12: Return type does not match returned value: Expected `Path`, found `(Self & ~AlwaysFalsy) | Unknown`
- Found 109 diagnostics
+ Found 98 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[unresolved-attribute] src/typeshed_stats/_cli.py:71:25: Type `_EnumMemberT` has no attribute `file_extension`
- error[invalid-return-type] src/typeshed_stats/gather.py:503:20: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[unresolved-attribute] tests/test__cli.py:99:42: Type `_EnumMemberT` has no attribute `name`
- error[invalid-argument-type] tests/test__cli.py:145:63: Argument to function `_OutputOption_to_argparse` is incorrect: Expected `OutputOption`, found `_EnumMemberT`
- error[invalid-argument-type] tests/test__cli.py:473:48: Argument to function `_OutputOption_to_argparse` is incorrect: Expected `OutputOption`, found `_EnumMemberT`
- error[invalid-argument-type] tests/test__cli.py:560:39: Argument to function `_OutputOption_to_argparse` is incorrect: Expected `OutputOption`, found `_EnumMemberT`
- Found 40 diagnostics
+ Found 34 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[no-matching-overload] tornado/ioloop.py:713:13: No overload of function `future_add_done_callback` matches arguments
- Found 508 diagnostics
+ Found 507 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[invalid-return-type] mkdocs/config/config_options.py:1034:16: Return type does not match returned value: Expected `list[str]`, found `list[T]`
- error[no-matching-overload] mkdocs/structure/files.py:552:9: No overload of bound method `sort` matches arguments
- error[no-matching-overload] mkdocs/structure/files.py:553:9: No overload of bound method `sort` matches arguments
- error[invalid-argument-type] mkdocs/structure/nav.py:145:34: Argument to function `_add_previous_and_next_links` is incorrect: Expected `list[Page]`, found `list[T]`
- error[unresolved-attribute] mkdocs/structure/nav.py:166:58: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:166:58: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:166:58: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:166:58: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:166:58: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:166:58: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:168:47: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:170:13: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:176:41: Type `T` has no attribute `url`
- error[unresolved-attribute] mkdocs/structure/nav.py:182:36: Type `T` has no attribute `url`
- error[invalid-argument-type] mkdocs/structure/nav.py:185:30: Argument to bound method `__init__` is incorrect: Expected `list[Page]`, found `list[T]`
- error[invalid-assignment] mkdocs/utils/__init__.py:266:5: Object of type `dict[_T, Any | None]` is not assignable to `dict[Unknown, None]`
+ error[invalid-assignment] mkdocs/utils/__init__.py:266:5: Object of type `dict[Unknown, Any | None]` is not assignable to `dict[Unknown, None]`
- Found 356 diagnostics
+ Found 341 diagnostics

jinja (https://github.com/pallets/jinja)
- error[unresolved-attribute] src/jinja2/compiler.py:855:16: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/compiler.py:856:36: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/compiler.py:857:25: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/compiler.py:861:16: Type `_NodeBound` has no attribute `importname`
- error[unresolved-attribute] src/jinja2/compiler.py:862:23: Type `_NodeBound` has no attribute `importname`
- error[unresolved-attribute] src/jinja2/compiler.py:1194:20: Type `_NodeBound` has no attribute `scoped`
- error[unresolved-attribute] src/jinja2/compiler.py:1245:16: Type `_NodeBound` has no attribute `ctx`
- error[unresolved-attribute] src/jinja2/compiler.py:1245:40: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/compiler.py:1592:16: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/compiler.py:1597:27: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/compiler.py:1598:37: Type `_NodeBound` has no attribute `name`
- error[unresolved-attribute] src/jinja2/ext.py:684:28: Type `_NodeBound` has no attribute `node`
- error[unresolved-attribute] src/jinja2/ext.py:685:16: Type `_NodeBound` has no attribute `node`
- error[unresolved-attribute] src/jinja2/ext.py:691:20: Type `_NodeBound` has no attribute `args`
- error[unresolved-attribute] src/jinja2/ext.py:697:18: Type `_NodeBound` has no attribute `kwargs`
- error[unresolved-attribute] src/jinja2/ext.py:699:12: Type `_NodeBound` has no attribute `dyn_args`
- error[unresolved-attribute] src/jinja2/ext.py:701:12: Type `_NodeBound` has no attribute `dyn_kwargs`
- error[unresolved-attribute] src/jinja2/ext.py:715:28: Type `_NodeBound` has no attribute `node`
+ warning[unused-ignore-comment] src/jinja2/meta.py:80:47: Unused blanket `type: ignore` directive
- Found 320 diagnostics
+ Found 303 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ warning[unused-ignore-comment] test/test_util.py:614:47: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] test/test_util.py:616:51: Unused blanket `type: ignore` directive
- error[invalid-argument-type] test/test_util.py:596:21: Argument to function `rewind_body` is incorrect: Expected `IO[AnyStr]`, found `BytesIO`
- error[invalid-argument-type] test/test_util.py:606:25: Argument to function `rewind_body` is incorrect: Expected `IO[AnyStr]`, found `BytesIO`
- error[invalid-argument-type] test/test_util.py:624:25: Argument to function `rewind_body` is incorrect: Expected `IO[AnyStr]`, found `BadSeek`
- Found 463 diagnostics
+ Found 462 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-argument-type] src/werkzeug/datastructures/file_storage.py:133:38: Argument to function `copyfileobj` is incorrect: Expected `SupportsWrite[AnyStr]`, found `PathLike[str] | IO[bytes] | (Unknown & ~str) | str`
+ error[invalid-argument-type] src/werkzeug/datastructures/file_storage.py:133:38: Argument to function `copyfileobj` is incorrect: Expected `SupportsWrite[Unknown]`, found `PathLike[str] | IO[bytes] | (Unknown & ~str) | str`
- error[unresolved-attribute] src/werkzeug/wrappers/request.py:310:13: Type `V` has no attribute `close`
- Found 462 diagnostics
+ Found 461 diagnostics

operator (https://github.com/canonical/operator)
- error[invalid-assignment] ops/_private/harness.py:3534:13: Object of type `list[SupportsRichComparisonT]` is not assignable to `list[str] | None`
- error[no-matching-overload] ops/_private/harness.py:3535:21: No overload of function `sorted` matches arguments
- error[invalid-argument-type] ops/lib/__init__.py:198:41: Argument to function `_join_and` is incorrect: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-argument-type] ops/lib/__init__.py:199:33: Argument to function `_join_and` is incorrect: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-argument-type] ops/lib/__init__.py:199:71: Argument to function `_join_and` is incorrect: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- Found 205 diagnostics
+ Found 200 diagnostics

paasta (https://github.com/yelp/paasta)
- error[invalid-argument-type] paasta_tools/apply_external_resources.py:22:25: Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Unknown | Literal[".applied"]`
- error[no-matching-overload] paasta_tools/apply_external_resources.py:23:9: No overload of bound method `sort` matches arguments
- error[no-matching-overload] paasta_tools/apply_external_resources.py:53:9: No overload of bound method `sort` matches arguments
- error[unresolved-attribute] paasta_tools/autoscaling/max_all_k8s_services.py:30:29: Type `InstanceConfig_T` has no attribute `get_max_instances`
- error[unresolved-attribute] paasta_tools/autoscaling/max_all_k8s_services.py:32:41: Type `InstanceConfig_T` has no attribute `format_kubernetes_app`
- error[unresolved-attribute] paasta_tools/check_autoscaler_max_instances.py:63:20: Type `InstanceConfig_T` has no attribute `get_autoscaling_metric_spec`
- error[invalid-argument-type] paasta_tools/check_autoscaler_max_instances.py:80:17: Argument to function `autoscaling_status` is incorrect: Expected `LongRunningServiceConfig`, found `InstanceConfig_T`
- error[unresolved-attribute] paasta_tools/check_autoscaler_max_instances.py:85:28: Type `InstanceConfig_T` has no attribute `get_sanitised_deployment_name`
- error[unresolved-attribute] paasta_tools/check_autoscaler_max_instances.py:103:44: Type `InstanceConfig_T` has no attribute `get_autoscaling_params`
- error[unresolved-attribute] paasta_tools/cli/cmds/status.py:1198:13: Type `_EnumMemberT` has no attribute `color`
- error[unresolved-attribute] paasta_tools/cli/cmds/status.py:1198:58: Type `_EnumMemberT` has no attribute `message`
- error[invalid-argument-type] paasta_tools/cli/utils.py:97:32: Argument to function `fnmatch` is incorrect: Expected `str`, found `AnyStr`
- error[invalid-argument-type] paasta_tools/contrib/timeouts_metrics_prom.py:62:49: Argument to function `read_and_write_timeouts_metrics` is incorrect: Expected `str`, found `AnyStr`
- error[invalid-argument-type] paasta_tools/contrib/timeouts_metrics_prom.py:62:55: Argument to function `read_and_write_timeouts_metrics` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] paasta_tools/instance/kubernetes.py:677:43: Argument to function `get_backends_from_mesh_status` is incorrect: Expected `Future[dict[Unknown, Unknown]]`, found `Task[_T]`
- error[invalid-argument-type] paasta_tools/instance/kubernetes.py:688:17: Argument to function `get_pod_status_tasks_by_sha_and_readiness` is incorrect: Expected `Future[dict[Unknown, Unknown]]`, found `Task[_T] | None`
+ error[invalid-argument-type] paasta_tools/instance/kubernetes.py:688:17: Argument to function `get_pod_status_tasks_by_sha_and_readiness` is incorrect: Expected `Future[dict[Unknown, Unknown]]`, found `Task[Unknown] | None`
- error[invalid-argument-type] paasta_tools/instance/kubernetes.py:699:17: Argument to function `get_versions_for_controller_revisions` is incorrect: Expected `Future[Mapping[tuple[str, str], Mapping[bool, Sequence[Future[Mapping[str, Any]]]]]]`, found `Task[_T]`
- error[invalid-argument-type] paasta_tools/instance/kubernetes.py:707:17: Argument to function `get_pod_status_tasks_by_replicaset` is incorrect: Expected `Future[dict[Unknown, Unknown]]`, found `Task[_T] | None`
+ error[invalid-argument-type] paasta_tools/instance/kubernetes.py:707:17: Argument to function `get_pod_status_tasks_by_replicaset` is incorrect: Expected `Future[dict[Unknown, Unknown]]`, found `Task[Unknown] | None`
- error[invalid-argument-type] paasta_tools/instance/kubernetes.py:718:17: Argument to function `get_versions_for_replicasets` is incorrect: Expected `Future[Mapping[str, Sequence[Future[dict[Unknown, Unknown]]]]]`, found `Task[_T]`
- error[invalid-argument-type] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:245:21: Argument to function `get_secrets_used_by_instance` is incorrect: Expected `KubernetesDeploymentConfig`, found `InstanceConfig_T`
- error[invalid-argument-type] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:245:21: Argument to function `get_secrets_used_by_instance` is incorrect: Expected `KubernetesDeploymentConfig`, found `InstanceConfig_T`
- error[invalid-argument-type] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:245:21: Argument to function `get_secrets_used_by_instance` is incorrect: Expected `KubernetesDeploymentConfig`, found `InstanceConfig_T`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:502:37: Type `InstanceConfig_T` has no attribute `get_datastore_credentials`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:547:32: Type `InstanceConfig_T` has no attribute `get_datastore_credentials_signature_name`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:548:29: Type `InstanceConfig_T` has no attribute `get_datastore_credentials_secret_name`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:580:27: Type `InstanceConfig_T` has no attribute `get_crypto_keys_from_config`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:599:63: Type `InstanceConfig_T` has no attribute `get_sanitised_deployment_name`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:612:32: Type `InstanceConfig_T` has no attribute `get_crypto_secret_signature_name`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:615:29: Type `InstanceConfig_T` has no attribute `get_crypto_secret_name`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:665:32: Type `InstanceConfig_T` has no attribute `get_boto_secret_signature_name`
- error[unresolved-attribute] paasta_tools/kubernetes/bin/paasta_secrets_sync.py:666:29: Type `InstanceConfig_T` has no attribute `get_boto_secret_name`
- error[unresolved-attribute] paasta_tools/kubernetes_tools.py:3643:31: Type `_EnumMemberT` has no attribute `name`
- error[unresolved-attribute] paasta_tools/long_running_service_tools.py:676:40: Type `InstanceConfig_T` has no attribute `get_registrations`
- error[unresolved-attribute] paasta_tools/long_running_service_tools.py:677:31: Type `InstanceConfig_T` has no attribute `get_instances`
- error[invalid-argument-type] paasta_tools/setup_prometheus_adapter_config.py:859:25: Argument to function `get_rules_for_service_instance` is incorrect: Expected `KubernetesDeploymentConfig`, found `InstanceConfig_T`
- Found 1009 diagnostics
+ Found 976 diagnostics

vision (https://github.com/pytorch/vision)
- error[unresolved-attribute] test/test_datasets.py:2392:26: Type `_T` has no attribute `relative_to`
- error[invalid-argument-type] test/test_datasets.py:2442:25: Argument to bound method `sample` is incorrect: Expected `Sequence[_T]`, found `range`
- error[invalid-argument-type] test/test_datasets.py:2713:33: Argument to bound method `writerow` is incorrect: Expected `Iterable[Any]`, found `_T`
- error[unresolved-attribute] test/test_extended_models.py:291:29: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:294:28: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:295:31: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:298:34: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:302:45: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:305:33: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:307:24: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:312:24: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:319:20: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:331:42: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:334:16: Type `_EnumMemberT` has no attribute `name`
- error[unresolved-attribute] test/test_extended_models.py:337:35: Type `_EnumMemberT` has no attribute `meta`
- error[unresolved-attribute] test/test_extended_models.py:395:22: Type `_EnumMemberT` has no attribute `transforms`
- error[invalid-return-type] torchvision/datasets/folder.py:46:12: Return type does not match returned value: Expected `tuple[list[str], dict[str, int]]`, found `tuple[list[SupportsRichComparisonT] & ~AlwaysFalsy, @Todo(dict comprehension type)]`
- error[invalid-return-type] torchvision/datasets/phototour.py:197:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] torchvision/datasets/places365.py:128:16: Return type does not match returned value: Expected `tuple[list[str], dict[str, int]]`, found `tuple[list[SupportsRichComparisonT], dict[Unknown, Unknown]]`
- error[no-matching-overload] torchvision/io/video.py:463:5: No overload of bound method `sort` matches arguments
- error[no-matching-overload] torchvision/io/video.py:463:5: No overload of bound method `sort` matches arguments
- error[invalid-assignment] torchvision/models/_api.py:235:13: Object of type `set[Unknown | _S]` is not assignable to `set[str]`
- error[invalid-return-type] torchvision/models/_api.py:244:12: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] torchvision/prototype/datasets/_api.py:35:12: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] torchvision/prototype/datasets/_builtin/caltech.py:151:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] torchvision/prototype/datasets/_builtin/country211.py:81:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] torchvision/prototype/datasets/_builtin/dtd.py:136:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-argument-type] torchvision/prototype/datasets/_builtin/voc.py:220:30: Argument to bound method `insert` is incorrect: Expected `SupportsRichComparisonT`, found `Literal["__background__"]`
- error[invalid-return-type] torchvision/prototype/datasets/_builtin/voc.py:222:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] torchvision/prototype/datasets/_folder.py:51:12: Return type does not match returned value: Expected `tuple[Unknown, list[str]]`, found `tuple[Unknown, list[SupportsRichComparisonT]]`
- error[call-non-callable] torchvision/transforms/transforms.py:575:16: Object of type `_T` is not callable
- Found 1966 diagnostics
+ Found 1935 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- error[unresolved-attribute] src/bandersnatch/simple.py:55:33: Type `_EnumMemberT` has no attribute `name`
- error[unresolved-attribute] src/bandersnatch/simple.py:66:33: Type `_EnumMemberT` has no attribute `name`
- error[invalid-return-type] src/bandersnatch/simple.py:112:16: Return type does not match returned value: Expected `list[str]`, found `list[SupportsRichComparisonT]`
- error[invalid-return-type] src/bandersnatch/simple.py:155:16: Return type does not match returned value: Expected `list[Path]`, found `list[SupportsRichComparisonT] | list[Unknown]`
- error[unresolved-attribute] src/bandersnatch/tests/test_simple.py:52:43: Type `_EnumMemberT` has no attribute `name`
- error[unresolved-attribute] src/bandersnatch/utils.py:127:50: Type `Self` has no attribute `keep_file`
- Found 154 diagnostics
+ Found 148 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[invalid-return-type] boostedblob/boost.py:369:16: Return type does not match returned value: Expected `Task[T]`, found `Task[_T]`
- error[invalid-return-type] boostedblob/boost.py:410:16: Return type does not match returned value: Expected `Task[T]`, found `Task[_T]`
+ warning[unused-ignore-comment] boostedblob/boost.py:550:78: Unused blanket `type: ignore` directive
- error[unsupported-operator] boostedblob/listing.py:300:19: Operator `/` is unsupported between objects of type `LocalPath` and `AnyStr`
- error[invalid-argument-type] boostedblob/listing.py:300:29: Argument is incorrect: Expected `str`, found `AnyStr`
- error[invalid-return-type] boostedblob/read.py:151:12: Return type does not match returned value: Expected `OrderedMappingBoostable[Any, bytes]`, found `OrderedMappingBoostable[Unknown, T]`
- error[invalid-return-type] boostedblob/read.py:194:12: Return type does not match returned value: Expected `UnorderedMappingBoostable[Any, tuple[bytes, @Todo(Inference of subscript on special form)]]`, found `UnorderedMappingBoostable[Unknown, T]`
- Found 67 diagnostics
+ Found 62 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/test/unit/test_container.py:15:40: Argument to function `hasattr` is incorrect: Expected `str`, found `Unknown | _T`
- Found 2763 diagnostics
+ Found 2762 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-return-type] psycopg/psycopg/_acompat.py:50:12: Return type does not match returned value: Expected `Task[None]`, found `Task[_T]`
- error[invalid-return-type] psycopg_pool/p...*[Comment body truncated]*

---

_@dcreager reviewed on 2025-05-12 18:58_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:80 on 2025-05-12 18:58_

This previously failed as `list[T]`, showing that we weren't detecting the `T` typevar when it only appeared inside of an instance-of type

---

_Comment by @github-actions[bot] on 2025-05-12 19:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-05-12 19:23_

(I think this definitely deserves a changelog entry, as it fixes _lots_ of false positives we're emitting on user code -- just look at that primer diff! So I wouldn't apply the `internal` label to this 😃)


---

_Label `internal` removed by @dcreager on 2025-05-12 19:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:1 on 2025-05-12 19:36_

can we add an mdtest that shows us inducting into a `SubclassOf` type, since `SubclassOfType::find_legacy_typevars()` is a different code path to other `find_legacy_typevar` methods? Would also be great to have a dedicated unit test for protocols -- I know that's covered in some ways by the test that uses `list` since `list` has a generic protocol in its MRO, but it's best where possible to have our tests isolated from possible changes typeshed might make (even if it's unlikely that they'd ever change the MRO of `list` any time soon!)

---

_@AlexWaygood approved on 2025-05-12 19:36_

nice!

---

_@dcreager reviewed on 2025-05-12 20:06_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/subclass_of.rs`:1 on 2025-05-12 20:06_

Done — I pulled over all of the new tests from #18021 and #18054.  Note that the `type[T]` tests currently fail because we don't support applying `type[...]` to a typevar yet

---

_Merged by @dcreager on 2025-05-13 01:53_

---

_Closed by @dcreager on 2025-05-13 01:53_

---

_Branch deleted on 2025-05-13 01:53_

---
