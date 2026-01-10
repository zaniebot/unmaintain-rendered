```yaml
number: 17998
title: "[ty] Understand homogeneous tuple annotations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/homogeneous-tuples
created_at: 2025-05-09T22:29:23Z
updated_at: 2025-05-13T02:11:18Z
url: https://github.com/astral-sh/ruff/pull/17998
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Understand homogeneous tuple annotations

---

_Pull request opened by @AlexWaygood on 2025-05-09 22:29_

## Summary

With this PR, `ty` now understands that `tuple[int, ...]` should be understood as "take the generic `tuple` class in typeshed, and specialize it with a single `int` type argument".

Ironically, more code had to change in our exception-handling logic than in our type-expression-parsing logic here, in order to prevent regressions in our tests.

This PR is stacked on top of https://github.com/astral-sh/ruff/pull/18052, as without the fixes there this PR causes a prohibitive number of false-positive errors.

Closes https://github.com/astral-sh/ty/issues/237

## Test Plan

Existing mdtests updated.


---

_Label `ty` added by @AlexWaygood on 2025-05-09 22:29_

---

_Comment by @github-actions[bot] on 2025-05-09 22:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ error[missing-argument] src/async_utils/corofunc_cache.py:98:20: No arguments provided for required parameters 1, 2
+ error[missing-argument] src/async_utils/corofunc_cache.py:177:20: No arguments provided for required parameters 1, 2
+ error[missing-argument] src/async_utils/task_cache.py:158:20: No arguments provided for required parameters 1, 2
+ error[missing-argument] src/async_utils/task_cache.py:244:20: No arguments provided for required parameters 1, 2
- Found 12 diagnostics
+ Found 16 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ error[unsupported-operator] mypy_primer/main.py:135:42: Operator `<` is not supported for types `_version_info` and `None`, in comparing `_version_info` with `tuple[int, int] | None`
- Found 18 diagnostics
+ Found 19 diagnostics

beartype (https://github.com/beartype/beartype)
- warning[unused-ignore-comment] beartype/__init__.py:122:63: Unused blanket `type: ignore` directive
- Found 554 diagnostics
+ Found 553 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[invalid-exception-caught] aioredis/connection.py:283:16: Cannot catch object of type `Unknown | tuple[Unknown, ...]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[unresolved-attribute] aioredis/connection.py:289:41: Type `BaseException` has no attribute `errno`
- error[invalid-exception-caught] aioredis/connection.py:511:16: Cannot catch object of type `Unknown | tuple[Unknown, ...]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[unresolved-attribute] aioredis/connection.py:517:41: Type `BaseException` has no attribute `errno`

anyio (https://github.com/agronholm/anyio)
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to bound method `put_nowait` is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple[Unknown, ...], Future[Unknown], CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(full tuple[...] support), Future[T_Retval], CancelScope | None]`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to bound method `put_nowait` is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple[Unknown, ...], Future[Unknown], CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(PEP 646), Future[T_Retval], CancelScope | None]`

kornia (https://github.com/kornia/kornia)
- error[unsupported-operator] kornia/augmentation/utils/param_validation.py:52:12: Operator `<` is not supported for types `list[Unknown]` and `int`, in comparing `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` with `Literal[0]`
+ error[unsupported-operator] kornia/augmentation/utils/param_validation.py:52:12: Operator `<` is not supported for types `tuple[int | float, int | float]` and `Literal[0]`, in comparing `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` with `Literal[0]`
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:138:17: Attribute `shape` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:138:17: Attribute `shape` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:138:50: Attribute `shape` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:138:50: Attribute `shape` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- error[invalid-argument-type] kornia/augmentation/utils/param_validation.py:138:82: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float`
+ error[invalid-argument-type] kornia/augmentation/utils/param_validation.py:138:82: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | tuple[Any, ...]`
+ error[unsupported-operator] kornia/augmentation/utils/param_validation.py:139:16: Operator `<` is not supported for types `tuple[Any, ...]` and `int`, in comparing `Unknown | int | float | tuple[Any, ...]` with `Literal[0]`
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:141:31: Attribute `repeat` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:141:31: Attribute `repeat` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:146:18: Attribute `shape` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:146:18: Attribute `shape` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- error[invalid-argument-type] kornia/augmentation/utils/param_validation.py:146:50: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float`
+ error[invalid-argument-type] kornia/augmentation/utils/param_validation.py:146:50: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | tuple[Any, ...]`
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:147:31: Attribute `repeat` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:147:31: Attribute `repeat` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:149:18: Attribute `shape` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:149:18: Attribute `shape` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- error[invalid-argument-type] kornia/augmentation/utils/param_validation.py:149:50: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float`
+ error[invalid-argument-type] kornia/augmentation/utils/param_validation.py:149:50: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | tuple[Any, ...]`
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:150:31: Attribute `unsqueeze` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:150:31: Attribute `unsqueeze` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:154:14: Attribute `shape` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:154:14: Attribute `shape` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:155:31: Attribute `to` on type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:155:31: Attribute `to` on type `Unknown | int | float | tuple[Any, ...]` is possibly unbound
+ error[invalid-argument-type] kornia/contrib/models/sam/model.py:213:17: Argument to function `_build_sam` is incorrect: Expected `tuple[int, ...]`, found `tuple[int, ...] | None`
+ warning[possibly-unbound-attribute] kornia/filters/in_range.py:138:39: Attribute `shape` on type `tuple[Any, ...] | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/in_range.py:138:52: Attribute `shape` on type `tuple[Any, ...] | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/in_range.py:142:17: Attribute `to` on type `tuple[Any, ...] | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/in_range.py:143:17: Attribute `to` on type `tuple[Any, ...] | Unknown` is possibly unbound
- error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[invalid-return-type] kornia/utils/_compat.py:50:82: Function can implicitly return `None`, which is not assignable to return type `tuple[Unknown, ...]`
- Found 963 diagnostics
+ Found 970 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
- warning[unused-ignore-comment] httpx_caching/_policy.py:216:76: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] httpx_caching/_heuristics.py:130:32: Argument to function `timegm` is incorrect: Expected `tuple[int, ...] | struct_time`, found `None`

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[possibly-unbound-attribute] strawberry/schema/name_converter.py:121:44: Attribute `__strawberry_definition__` on type `(@Todo(full tuple[...] support) & ~LazyType[Unknown, Unknown]) | StrawberryType` is possibly unbound
+ error[unresolved-attribute] strawberry/schema/name_converter.py:121:44: Type `StrawberryType` has no attribute `__strawberry_definition__`
- warning[unused-ignore-comment] strawberry/types/info.py:81:35: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] strawberry/types/type_resolver.py:130:21: Attribute `namespace` on type `StrawberryAnnotation | None` is possibly unbound
+ warning[possibly-unbound-attribute] strawberry/types/type_resolver.py:132:17: Attribute `set_namespace_from_field` on type `StrawberryAnnotation | None` is possibly unbound
- Found 453 diagnostics
+ Found 454 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ warning[possibly-unbound-attribute] src/graphql/execution/execute.py:264:42: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[unused-ignore-comment] src/graphql/language/visitor.py:292:75: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] src/graphql/utilities/get_operation_ast.py:29:38: Attribute `value` on type `NameNode | None` is possibly unbound
+ error[unresolved-attribute] src/graphql/validation/rules/defer_stream_directive_on_valid_operations_rule.py:27:20: Type `ValueNode` has no attribute `value`
+ error[unresolved-attribute] src/graphql/validation/rules/known_type_names.py:47:38: Type `DefinitionNode` has no attribute `name`
+ warning[possibly-unbound-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:738:29: Attribute `value` on type `NameNode | None` is possibly unbound
+ error[unresolved-attribute] src/graphql/validation/rules/possible_type_extensions.py:35:13: Type `DefinitionNode` has no attribute `name`
+ error[not-iterable] tests/utils/assert_matching_values.py:11:18: Object of type `T` is not iterable
- Found 407 diagnostics
+ Found 413 diagnostics

trio (https://github.com/python-trio/trio)
- warning[unused-ignore-comment] src/trio/_core/_tests/test_run.py:1733:37: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_core/_tests/test_run.py:1743:39: Unused blanket `type: ignore` directive
- error[invalid-exception-caught] src/trio/_tests/test_highlevel_open_tcp_stream.py:387:12: Cannot catch object of type `@Todo(full tuple[...] support) | type[BaseException] | tuple[()]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:44:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:50:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[SyntaxError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:54:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, SyntaxError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:58:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, SyntaxError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:62:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:62:60: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:71:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[SyntaxError, ExceptionGroup[Exception], ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:73:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:74:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[RuntimeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:88:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[RuntimeError, ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:97:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:110:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:123:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:144:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:144:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:147:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:147:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:153:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:153:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:157:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:157:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:157:53: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:181:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:347:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:351:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:355:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:362:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:372:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:388:30: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:401:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:439:42: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1000:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1007:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1012:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1019:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1023:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1030:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1049:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[OSError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1056:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[OSError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1065:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[OSError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1115:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:24:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:30:71: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:133:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:156:9: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:156:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ error[invalid-argument-type] src/trio/testing/_raises_group.py:593:25: Argument to function `issubclass` is incorrect: Expected `type`, found `@Todo(unsupported type[X] special form) | (@Todo(unsupported type[X] special form) & None) | (@Todo(unsupported type[X] special form) & type[BaseException]) | None`
- Found 1074 diagnostics
+ Found 1118 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ error[invalid-argument-type] mkosi/__init__.py:1373:34: Argument to function `replace` is incorrect: Argument type `Config` does not satisfy upper bound of type variable `_DataclassT`
- Found 325 diagnostics
+ Found 326 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- warning[unused-ignore-comment] src/urllib3/util/ssl_.py:47:48: Unused blanket `type: ignore` directive
+ error[no-matching-overload] src/urllib3/util/url.py:438:16: No overload of function `_normalize_host` matches arguments

pydantic (https://github.com/pydantic/pydantic)
+ error[invalid-argument-type] pydantic/_internal/_decorators.py:389:49: Argument to function `mro` is incorrect: Expected `type[Any]`, found `(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] pydantic/_internal/_fields.py:102:52: Argument to bound method `startswith` is incorrect: Expected `str | tuple[str, ...]`, found `str | Pattern[str]`
+ error[invalid-argument-type] pydantic/_internal/_fields.py:281:61: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type`
+ error[invalid-assignment] pydantic/main.py:863:13: Object of type `tuple[(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
+ error[invalid-argument-type] pydantic/main.py:867:67: Argument to function `map_generic_model_arguments` is incorrect: Expected `tuple[Any, ...]`, found `(type[Any] & tuple[Unknown, ...]) | (tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
+ error[invalid-assignment] pydantic/v1/generics.py:100:13: Object of type `tuple[(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
+ error[invalid-argument-type] pydantic/v1/generics.py:106:37: Argument to function `check_parameters_count` is incorrect: Expected `tuple[Any, ...]`, found `(type[Any] & tuple[Unknown, ...]) | (tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
- Found 779 diagnostics
+ Found 786 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[invalid-parameter-default] tornado/gen.py:442:5: Default value of type `tuple[()]` is not assignable to annotated parameter type `type[Exception] | tuple[type[Exception], Unknown]`
- error[invalid-parameter-default] tornado/gen.py:496:5: Default value of type `tuple[()]` is not assignable to annotated parameter type `type[Exception] | tuple[type[Exception], Unknown]`
- error[invalid-parameter-default] tornado/gen.py:581:5: Default value of type `tuple[()]` is not assignable to annotated parameter type `type[Exception] | tuple[type[Exception], Unknown]`
+ error[no-matching-overload] tornado/web.py:3770:21: No overload of function `utf8` matches arguments
- Found 513 diagnostics
+ Found 511 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ error[invalid-return-type] tests/conftest.py:432:12: Return type does not match returned value: Expected `tuple[int, int, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
- Found 1054 diagnostics
+ Found 1055 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-argument-type] test/datasets_utils.py:422:65: Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(full tuple[...] support) | None`
+ error[invalid-argument-type] test/datasets_utils.py:422:65: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[Any, ...] | None`
- error[invalid-argument-type] test/datasets_utils.py:689:55: Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(full tuple[...] support) | None`
+ error[invalid-argument-type] test/datasets_utils.py:689:55: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[Any, ...] | None`
- error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `Unknown | tuple[int, int, int] | Sequence[int] | int | (((int, /) -> Sequence[int] | int) & ~None) | (def size(idx: int) -> tuple[int, int, int])`
+ error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `Unknown | Sequence[int] | int | (((int, /) -> Sequence[int] | int) & ~None)`
- error[invalid-assignment] test/datasets_utils.py:1043:9: Object of type `LiteralString` is not assignable to `tuple[Unknown, ...]`
+ error[invalid-assignment] test/datasets_utils.py:1043:9: Object of type `LiteralString` is not assignable to `tuple[str, ...]`
- error[invalid-assignment] test/datasets_utils.py:1045:9: Object of type `LiteralString` is not assignable to `tuple[Unknown, ...]`
+ error[invalid-assignment] test/datasets_utils.py:1045:9: Object of type `LiteralString` is not assignable to `tuple[str, ...]`
- warning[unused-ignore-comment] torchvision/datasets/folder.py:78:63: Unused blanket `type: ignore` directive
- error[invalid-argument-type] torchvision/ops/misc.py:90:92: Argument to function `len` is incorrect: Expected `Sized`, found `int | @Todo(full tuple[...] support)`
+ error[invalid-argument-type] torchvision/ops/misc.py:90:92: Argument to function `len` is incorrect: Expected `Sized`, found `int | tuple[int, ...]`
+ error[unsupported-operator] torchvision/ops/poolers.py:190:9: Operator `+` is unsupported between objects of type `tuple[int, Unknown]` and `list[int]`
+ error[invalid-argument-type] torchvision/transforms/_functional_pil.py:179:42: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number & ~list[Unknown]) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number & ~list[Unknown]) | (int & tuple[Unknown, ...] & ~list[Unknown]) | (list[int] & tuple[Unknown, ...] & ~list[Unknown]) | (tuple[int, ...] & tuple[Unknown, ...] & ~list[Unknown]) | (int & list[Unknown] & ~list[Unknown]) | (list[int] & list[Unknown] & ~list[Unknown]) | (tuple[int, ...] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[invalid-argument-type] torchvision/transforms/_functional_pil.py:183:37: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number & ~list[Unknown]) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number & ~list[Unknown]) | (int & tuple[Unknown, ...] & ~list[Unknown]) | (list[int] & tuple[Unknown, ...] & ~list[Unknown]) | (tuple[int, ...] & tuple[Unknown, ...] & ~list[Unknown]) | (int & list[Unknown] & ~list[Unknown]) | (list[int] & list[Unknown] & ~list[Unknown]) | (tuple[int, ...] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(map_with_boundness: intersections with negative contributions)`
- Found 1935 diagnostics
+ Found 1937 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[not-iterable] mitmproxy/addons/strip_dns_https_records.py:29:34: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] mitmproxy/addons/strip_dns_https_records.py:29:34: Object of type `tuple[bytes, ...] | None` may not be iterable
- error[not-iterable] mitmproxy/addons/strip_dns_https_records.py:34:34: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] mitmproxy/addons/strip_dns_https_records.py:34:34: Object of type `tuple[bytes, ...] | None` may not be iterable
+ error[unsupported-operator] mitmproxy/io/compat.py:519:21: Operator `>` is not supported for types `tuple[Unknown, ...]` and `int`, in comparing `(Any & int) | (tuple[Unknown, ...] & int)` with `Unknown | Literal[21]`
- error[invalid-return-type] mitmproxy/net/http/url.py:153:20: Return type does not match returned value: Expected `AnyStr`, found `LiteralString`
+ error[invalid-return-type] mitmproxy/net/http/url.py:153:20: Return type does not match returned value: Expected `AnyStr`, found `str`
+ error[invalid-argument-type] mitmproxy/net/tls.py:261:38: Argument to bound method `add_extra_chain_cert` is incorrect: Expected `X509`, found `Unknown | Certificate`
- error[invalid-exception-caught] mitmproxy/proxy/layers/http/_http2.py:198:24: Cannot catch object of type `Unknown | tuple[<class 'ValueError'>, <class 'IndexError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 2162 diagnostics
+ Found 2163 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-exception-caught] src/werkzeug/serving.py:371:16: Cannot catch object of type `tuple[<class 'ConnectionError'>, <class 'timeout'>, <class 'SSLEOFError'>] | tuple[<class 'ConnectionError'>, <class 'timeout'>] | @Todo(full tuple[...] support)` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 460 diagnostics
+ Found 459 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-exception-caught] psycopg/psycopg/_pipeline.py:204:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/_pipeline.py:244:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/_server_cursor.py:105:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/_server_cursor_async.py:105:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/connection.py:110:20: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/connection.py:257:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/connection.py:364:32: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/connection_async.py:126:20: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/connection_async.py:274:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/connection_async.py:386:32: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor.py:97:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor.py:127:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor.py:158:20: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor.py:268:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor_async.py:97:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor_async.py:127:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor_async.py:158:20: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] psycopg/psycopg/cursor_async.py:270:16: Cannot catch object of type `Unknown | tuple[<class 'Error'>, <class 'KeyboardInterrupt'>, <class 'CancelledError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 1259 diagnostics
+ Found 1241 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[not-iterable] comtypes/_memberspec.py:445:33: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] comtypes/_memberspec.py:445:33: Object of type `tuple[Unknown, ...] | None` may not be iterable
- error[not-iterable] comtypes/_memberspec.py:453:33: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] comtypes/_memberspec.py:453:33: Object of type `tuple[Unknown, ...] | None` may not be iterable
- error[not-iterable] comtypes/_memberspec.py:459:33: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] comtypes/_memberspec.py:459:33: Object of type `tuple[Unknown, ...] | None` may not be iterable
- error[not-iterable] comtypes/_memberspec.py:507:76: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] comtypes/_memberspec.py:507:76: Object of type `tuple[Unknown, ...] | None` may not be iterable
+ error[invalid-argument-type] comtypes/_memberspec.py:509:58: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
- error[invalid-argument-type] comtypes/_vtbl.py:284:42: Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(full tuple[...] support) | None`
+ error[invalid-argument-type] comtypes/_vtbl.py:284:42: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[Unknown, ...] | None`
- error[invalid-argument-type] comtypes/_vtbl.py:286:42: Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(full tuple[...] support) | None`
+ error[invalid-argument-type] comtypes/_vtbl.py:286:42: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/server/register.py:472:25: Argument to function `run` is incorrect: Expected `Sequence[type[COMObject]]`, found `tuple[type, ...]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:38:54: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:83:54: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:111:54: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:138:54: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:158:64: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:283:62: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:491:54: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[Unknown, ...]`, found `tuple[Unknown, ...] | None`
- Found 677 diagnostics
+ Found 686 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ error[unresolved-attribute] scrapy/item.py:38:27: Type `type` has no attribute `_class`
- Found 1425 diagnostics
+ Found 1426 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
+ error[no-matching-overload] dedupe/convenience.py:77:12: No overload of function `__new__` matches arguments
- Found 67 diagnostics
+ Found 68 diagnostics

colour (https://github.com/colour-science/colour)
+ error[no-matching-overload] colour/io/luts/lut.py:1995:27: No overload of function `reshape` matches arguments
+ error[no-matching-overload] colour/io/luts/lut.py:1999:27: No overload of function `reshape` matches arguments
- Found 655 diagnostics
+ Found 657 diagnostics

meson (https://github.com/mesonbuild/meson)
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:520:61: Argument to bound method `single_use` is incorrect: Expected `str`, found `Unknown | str | None`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:520:85: Argument to bound method `single_use` is incorrect: Expected `str`, found `Unknown | str | None`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:523:68: Argument to bound method `single_use` is incorrect: Expected `str`, found `Unknown | str | None`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:523:97: Argument to bound method `single_use` is incorrect: Expected `str`, found `Unknown | str | None`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:526:45: Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:527:54: Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[call-non-callable] mesonbuild/interpreterbase/decorators.py:531:31: Object of type `None` is not callable
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:536:45: Argument to function `emit_feature_change` is incorrect: Expected `dict[Unknown, Unknown]`, found `Unknown | dict[Unknown, Unknown] | None`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:539:45: Argument to function `emit_feature_change` is incorrect: Expected `dict[Unknown, Unknown]`, found `Unknown | dict[Unknown, Unknown] | None`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:546:45: Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:546:197: Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[call-non-callable] mesonbuild/interpreterbase/decorators.py:554:41: Object of type `None` is not callable
- Found 1438 diagnostics
+ Found 1450 diagnostics

altair (https://github.com/vega/altair)
+ error[invalid-argument-type] altair/utils/core.py:48:58: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
+ error[invalid-argument-type] altair/utils/core.py:49:60: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[ParamSpec, typing.TypeVar]`
+ error[invalid-argument-type] altair/utils/core.py:53:56: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar, typing.TypeVar]`
+ error[invalid-argument-type] altair/utils/core.py:56:54: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar, ParamSpec, typing.TypeVar]`
+ error[invalid-argument-type] altair/utils/plugin_registry.py:25:52: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
+ error[invalid-argument-type] altair/utils/schemapi.py:903:56: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
+ error[invalid-argument-type] altair/vegalite/v5/schema/_typing.py:105:61: Argument to bound method `__init__` is incorrect: Expected `tuple[TypeVar | ParamSpec | TypeVarTuple, ...]`, found `tuple[typing.TypeVar]`
- Found 1556 diagnostics
+ Found 1563 diagnostics

mypy (https://github.com/python/mypy)
+ error[invalid-assignment] mypy/semanal.py:7660:9: Object of type `tuple[(str & ~tuple[Unknown, ...]) | (tuple[str, ...] & ~tuple[Unknown, ...])]` is not assignable to `str | tuple[str, ...]`
+ error[invalid-assignment] mypy/types.py:3581:9: Object of type `tuple[(str & ~tuple[Unknown, ...]) | (tuple[str, ...] & ~tuple[Unknown, ...])]` is not assignable to `str | tuple[str, ...]`
- error[unsupported-operator] mypy/typeshed/stdlib/inspect.pyi:425:29: Operator `|` is unsupported between objects of type `<class 'list[@Todo(full tuple[...] support)]'>` and `<class 'list[Unknown]'>`
+ error[unsupported-operator] mypy/typeshed/stdlib/inspect.pyi:425:29: Operator `|` is unsupported between objects of type `<class 'list[tuple[type, ...]]'>` and `<class 'list[Unknown]'>`
+ error[invalid-return-type] mypyc/test-data/fixtures/ir.py:215:41: Function can implicitly return `None`, which is not assignable to return type `tuple[T_co, ...]`
+ error[invalid-return-type] mypyc/test-data/fixtures/ir.py:216:42: Function can implicitly return `None`, which is not assignable to return type `tuple[T_co, ...]`
- Found 3330 diagnostics
+ Found 3334 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-argument-type] tests/annotations/declarations.py:434:26: Argument to function `make_config` is incorrect: Expected `tuple[type[DataClass_], ...]`, found `tuple[type[DataClass], Unknown, <class 'P3'>, Unknown]`
- warning[unused-ignore-comment] tests/annotations/declarations.py:441:45: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:442:38: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:443:35: Unused blanket `type: ignore` directive
- error[type-assertion-failure] tests/annotations/declarations.py:951:5: Actual type `FullBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 652 diagnostics
+ Found 649 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[unused-ignore-comment] static_frame/core/archive_npy.py:132:48: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/archive_npy.py:140:48: Unused blanket `type: ignore` directive
+ error[no-matching-overload] static_frame/core/util.py:1355:21: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] static_frame/test/unit/test_util.py:2127:27: Argument to function `_ufunc_set_1d` is incorrect: Expected `(ndarray[Any, Any], ndarray[Any, Any], /) -> ndarray[Any, Any]`, found `Overload[(a: @Todo(Support for `typing.TypeAlias`), axis: None = None, out: None = None, keepdims: Literal[False, 0] | _NoValueType = EllipsisType, *, where: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType) -> bool[bool], (a: @Todo(Support for `typing.TypeAlias`), axis: int | tuple[int, ...] | None = None, out: None = None, keepdims: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType, *, where: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType) -> @Todo(Support for `typing.TypeAlias`), (a: @Todo(Support for `typing.TypeAlias`), axis: int | tuple[int, ...] | None, out: _ArrayT, keepdims: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType, *, where: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType) -> _ArrayT, (a: @Todo(Support for `typing.TypeAlias`), axis: int | tuple[int, ...] | None = None, *, out: _ArrayT, keepdims: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType, where: @Todo(Support for `typing.TypeAlias`) | _NoValueType = EllipsisType) -> _ArrayT]`

optuna (https://github.com/optuna/optuna)
- error[not-iterable] optuna/visualization/matplotlib/_slice.py:159:20: Object of type `@Todo(full tuple[...] support) | None` may not be iterable
+ error[not-iterable] optuna/visualization/matplotlib/_slice.py:159:20: Object of type `tuple[Unknown, ...] | None` may not be iterable
- error[unsupported-operator] tests/study_tests/test_study.py:55:12: Operator `+` is unsupported between objects of type `int | float` and `None`
- Found 1317 diagnostics
+ Found 1316 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-exception-caught] src/_pytest/fixtures.py:1176:12: Cannot catch object of type `Unknown | tuple[<class 'OutcomeException'>, <class 'Exception'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-argument-type] src/_pytest/fixtures.py:1365:9: Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | Sequence[object] | (((Any, /) -> object) & ~None) | tuple[Unknown, ...]`
+ error[no-matching-overload] src/_pytest/pytester.py:1404:25: No overload of function `fspath` matches arguments
+ error[invalid-argument-type] src/_pytest/raises.py:1254:41: Argument to function `_exception_type_name` is incorrect: Expected `type[BaseException] | tuple[type[BaseException], ...]`, found `type[BaseException] | (AbstractRaises[BaseException] & type)`
- error[invalid-exception-caught] src/_pytest/runner.py:515:20: Cannot catch object of type `Unknown | tuple[<class 'OutcomeException'>, <class 'Exception'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] src/_pytest/runner.py:547:24: Cannot catch object of type `Unknown | tuple[<class 'OutcomeException'>, <class 'Exception'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- warning[unused-ignore-comment] testing/test_recwarn.py:112:49: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] testing/test_recwarn.py:114:70: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] testing/test_recwarn.py:116:84: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] testing/test_recwarn.py:352:33: Unused blanket `type: ignore` directive
- Found 942 diagnostics
+ Found 938 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ error[invalid-argument-type] pymongo/message.py:738:34: Argument to function `_inflate_bson` is incorrect: Expected `bytes`, found `memoryview[Unknown]`
- error[invalid-exception-caught] pymongo/network_layer.py:107:20: Cannot catch object of type `Unknown | tuple[<class 'BlockingIOError'>, @Todo(starred expression), @Todo(starred expression)]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] pymongo/network_layer.py:156:20: Cannot catch object of type `Unknown | tuple[<class 'BlockingIOError'>, @Todo(starred expression), @Todo(starred expression)]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-argument-type] pymongo/network_layer.py:351:52: Argument to bound method `recv_into` is incorrect: Expected `bytes`, found `memoryview[Unknown]`
- error[invalid-exception-caught] pymongo/pyopenssl_context.py:125:20: Cannot catch object of type `Unknown | tuple[<class 'WantReadError'>, <class 'WantWriteError'>, <class 'WantX509LookupError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 555 diagnostics
+ Found 554 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ error[unsupported-operator] ext/tests/test_monkeypatching.py:110:39: Operator `<=` is not supported for types `tuple[@Todo(Generic tuple specializations), ...]` and `None`, in comparing `Unknown | tuple[@Todo(Generic tuple specializations), ...]` with `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- Found 1173 diagnostics
+ Found 1174 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[invalid-exception-caught] aiohttp/connector.py:1135:16: Cannot catch object of type `Unknown | tuple[<class 'SSLCertVerificationError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] aiohttp/connector.py:1137:16: Cannot catch object of type `Unknown | tuple[<class 'SSLError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-argument-type] aiohttp/connector.py:1136:71: Argument to bound method `__init__` is incorrect: Expected `Exception`, found `BaseException`
- error[invalid-exception-caught] aiohttp/connector.py:1230:16: Cannot catch object of type `Unknown | tuple[<class 'SSLCertVerificationError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] aiohttp/connector.py:1232:16: Cannot catch object of type `Unknown | tuple[<class 'SSLError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-argument-type] aiohttp/connector.py:1231:71: Argument to bound method `__init__` is incorrect: Expected `Exception`, found `BaseException`
- error[invalid-assignment] aiohttp/http_writer.py:152:17: Object of type `bytes | bytearray | int | @Todo(map_with_boundness: intersections with negative contributions)` is not assignable to `bytes | bytearray | memoryview[int] | memoryview[bytes]`
- Found 178 diagnostics
+ Found 175 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ error[invalid-return-type] src/bokeh/resources.py:111:12: Return type does not match returned value: Expected `tuple[str, ...]`, found `set[Unknown]`
- Found 1003 diagnostics
+ Found 1004 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[invalid-argument-type] ddtrace/contrib/dbapi_async.py:92:70: Argument to function `dispatch_with_results` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/patch.py:236:9: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/patch.py:261:60: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/patch.py:270:17: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/patch.py:279:64: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:64:21: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:68:74: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:83:17: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:87:74: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:98:74: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:113:17: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:134:74: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:140:73: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:397:64: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:434:9: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:444:69: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/bedrock.py:525:74: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:41:13: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:91:72: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:101:21: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:157:72: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:166:80: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:168:81: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:173:76: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/kinesis.py:179:21: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/sqs.py:75:59: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/sqs.py:107:72: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/sqs.py:112:17: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/sqs.py:160:68: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unknown]`
+ error[invalid-argument-type] ddtrace/contrib/internal/botocore/services/sqs.py:167:80: Argument to function `dispatch` is incorrect: Expected `tuple[Any, ...]`, found `list[Unk...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-05-09 22:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fhomogeneous-tuples)

### Merging #17998 will **not alter performance**

<sub>Comparing <code>alex/homogeneous-tuples</code> (f65958a) with <code>main</code> (f301931)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_@AlexWaygood reviewed on 2025-05-09 22:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:24 on 2025-05-09 22:44_

this looks similar to https://github.com/astral-sh/ruff/pull/17832#discussion_r2081502056

---

_Closed by @AlexWaygood on 2025-05-12 22:20_

---

_Reopened by @AlexWaygood on 2025-05-12 22:20_

---

_Marked ready for review by @AlexWaygood on 2025-05-12 22:48_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-12 22:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-12 22:48_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-12 22:48_

---

_Comment by @carljm on 2025-05-12 22:52_

Looking at the primer results, it seems like we might want assignability of homogenous tuple to Sequence before we land this?

---

_Comment by @AlexWaygood on 2025-05-12 22:52_

> ```diff
> + error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:347:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
> ```

I added tests that demonstrate that we do understand a heterogeneous tuples as being subtypes of `Sequence`. I think this is a bug in TypeVar solving that's being exposed by this PR, rather than a bug in this PR.

---

_Comment by @carljm on 2025-05-12 22:53_

Yeah I was just reaching the same conclusion 👍 

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:20 on 2025-05-12 22:56_

Do you have any insight into what we are missing here?

---

_Comment by @AlexWaygood on 2025-05-12 22:57_

I added some more tests, just for safety 😆

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_fully_static.md`:51 on 2025-05-12 23:00_

I guess in general we don't descend into generic instance types when detecting fully-staticness? Do we have an issue for this already? It seems important and not too hard to fix?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2487 on 2025-05-12 23:04_

I guess we can be sure that any generic alias will always have at least arity-1 specialization, and this will never panic?

(Leaving aside the fact that we already know its `KnownClass::Tuple`, which can only violate our assumptions if a custom typeshed were used.

---

_@carljm approved on 2025-05-12 23:07_

Excellent!

---

_@AlexWaygood reviewed on 2025-05-12 23:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_fully_static.md`:51 on 2025-05-12 23:09_

> I guess in general we don't descend into generic instance types when detecting fully-staticness?

that's correct, yes.

> Do we have an issue for this already?

Yes -- https://github.com/astral-sh/ty/issues/131

---

_@AlexWaygood reviewed on 2025-05-12 23:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:2487 on 2025-05-12 23:12_

Ah, I forgot about custom typesheds -- I was just justifying this to myself on the grounds of "we know how many type parameters `tuple` has". But yes, I think you're right that any generic alias will have at least arity-1 specialisation, so I don't think this can ever panic!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:20 on 2025-05-12 23:20_

not really... there's so many generics fixes happening at once, though, that I feel like I'm sort-of losing track 😄

---

_@AlexWaygood reviewed on 2025-05-12 23:20_

---

_@carljm reviewed on 2025-05-12 23:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:20 on 2025-05-12 23:55_

that's fine, not a blocker here

---

_Merged by @AlexWaygood on 2025-05-13 02:02_

---

_Closed by @AlexWaygood on 2025-05-13 02:02_

---

_Branch deleted on 2025-05-13 02:02_

---

_@AlexWaygood reviewed on 2025-05-13 02:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:20 on 2025-05-13 02:11_

Looks like @dcreager's fixing it over in #18021 😁

---
