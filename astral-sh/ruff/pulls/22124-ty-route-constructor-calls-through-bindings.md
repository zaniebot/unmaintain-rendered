```yaml
number: 22124
title: "[ty] route constructor calls through bindings"
type: pull_request
state: open
author: Hugo-Polloli
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: constructor-bindings-refactor
created_at: 2025-12-21T03:30:02Z
updated_at: 2026-01-17T19:11:27Z
url: https://github.com/astral-sh/ruff/pull/22124
synced_at: 2026-01-17T20:10:50Z
```

# [ty] route constructor calls through bindings

---

_@Hugo-Polloli_

## Summary

fixes https://github.com/astral-sh/ty/issues/2124

* route constructor calls through the existing `Type::bindings` machinery via `constructor_bindings()`, ensuring constructor return types are instance-typed (not `__init__`'s `None`)
* delete the legacy constructor call path
* constructor overload resolution is deterministic (first matching overload), and implicit `__new__`/`__init__` lints remain method-specific and fire once per call site (including the missing-`__init__` fallback)

## Test Plan

* mdtest (constructor): added coverage for `@staticmethod __new__`, conflicting `__new__`/`__init__` parameter types, `__new__`-only fallback (object `__init__` accepts args), and generic constructor inference.
* mdtest (decorators): `functools.cached_property` specifically added to address https://github.com/astral-sh/ty/issues/1446
* mdtest (classes): fixed the TODOs for generic class params `C[Unknown]` are now correctly revealed as `C[int]` (and constructor specialization uses first matching overload)
* mdtest (functions): improved constructor path now correctly surfaces both the ill-typed generic call and the resulting invalid assignment


---

_Review requested from @carljm by @Hugo-Polloli on 2025-12-21 03:30_

---

_Review requested from @AlexWaygood by @Hugo-Polloli on 2025-12-21 03:30_

---

_Review requested from @sharkdp by @Hugo-Polloli on 2025-12-21 03:30_

---

_Review requested from @dcreager by @Hugo-Polloli on 2025-12-21 03:30_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 03:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-27 11:33:08.960835361 +0000
+++ new-output.txt	2025-12-27 11:33:11.098858578 +0000
@@ -511,6 +511,7 @@
 generics_paramspec_specialization.py:61:16: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal[""]`
 generics_scoping.py:14:1: error[type-assertion-failure] Type `int` does not match asserted type `Literal[1]`
 generics_scoping.py:15:1: error[type-assertion-failure] Type `str` does not match asserted type `Literal["a"]`
+generics_scoping.py:29:10: error[invalid-argument-type] Argument to bound method `meth_2` is incorrect: Expected `int`, found `Literal["a"]`
 generics_scoping.py:42:1: error[type-assertion-failure] Type `str` does not match asserted type `Literal["abc"]`
 generics_scoping.py:43:1: error[type-assertion-failure] Type `bytes` does not match asserted type `Literal[b"abc"]`
 generics_scoping.py:65:11: error[invalid-generic-class] Generic class `MyGeneric` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
@@ -1030,4 +1031,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1032 diagnostics
+Found 1033 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-21 03:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch/tests/test_configuration.py:108:17: error[no-matching-overload] No overload of class `str` matches arguments
+ src/bandersnatch/tests/test_configuration.py:108:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `str | None`
- Found 80 diagnostics
+ Found 82 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/database.py:652:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `lib.spack.spack.llnl.util.lock.Lock`, found `ForbiddenLock | lib.spack.spack.util.lock.Lock`
+ lib/spack/spack/database.py:656:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `lib.spack.spack.llnl.util.lock.Lock`, found `ForbiddenLock | lib.spack.spack.util.lock.Lock`
- Found 4292 diagnostics
+ Found 4319 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/structures.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `dict[K@MultiDict, V@MultiDict] | dict[K@MultiDict, list[V@MultiDict]]`, found `dict[K@MultiDict, V@MultiDict | list[V@MultiDict]]`
- src/werkzeug/debug/__init__.py:552:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/debug/tbtools.py:266:16: error[invalid-return-type] Return type does not match returned value: expected `list[DebugFrameSummary]`, found `list[FrameSummary | Unknown]`
+ tests/test_datastructures.py:582:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(str | None, /) -> Literal[-1]`, found `<class 'int'>`
+ tests/test_datastructures.py:583:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(str | None, /) -> Literal[-1]`, found `<class 'int'>`
+ tests/test_formparser.py:214:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `FileStorage` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ tests/test_formparser.py:214:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `FileStorage`
+ tests/test_routing.py:1310:9: warning[possibly-missing-attribute] Attribute `endpoint` may be missing on object of type `Rule | None`
+ tests/test_test.py:306:12: warning[possibly-missing-attribute] Attribute `username` may be missing on object of type `Authorization | None`
+ tests/test_test.py:307:12: warning[possibly-missing-attribute] Attribute `password` may be missing on object of type `Authorization | None`
+ tests/test_utils.py:285:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Any, /) -> Unknown`, found `def foo() -> Unknown`
+ tests/test_wrappers.py:179:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:180:12: warning[possibly-missing-attribute] Attribute `username` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:181:12: warning[possibly-missing-attribute] Attribute `password` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:189:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:190:12: warning[possibly-missing-attribute] Attribute `username` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:191:12: warning[possibly-missing-attribute] Attribute `password` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:243:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `bytes`, found `Literal["bar"]`
+ tests/test_wrappers.py:465:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `stream` on type `Request` with custom `__set__` method
+ tests/test_wrappers.py:710:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `bytes`, found `Literal["Hello "]`
+ tests/test_wrappers.py:711:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `bytes`, found `Literal["World!"]`
+ tests/test_wrappers.py:1079:12: warning[possibly-missing-attribute] Attribute `ranges` may be missing on object of type `Range | None`
+ tests/test_wrappers.py:1082:26: warning[possibly-missing-attribute] Attribute `make_content_range` may be missing on object of type `Range | None`
+ tests/test_wrappers.py:1124:28: error[invalid-argument-type] Argument to bound method `writelines` is incorrect: Expected `Iterable[bytes]`, found `list[Unknown | str]`
+ tests/test_wrappers.py:1129:28: error[invalid-argument-type] Argument to bound method `writelines` is incorrect: Expected `Iterable[bytes]`, found `list[Unknown | str]`
+ tests/test_wrappers.py:1133:28: error[invalid-argument-type] Argument to bound method `writelines` is incorrect: Expected `Iterable[bytes]`, found `list[Unknown | str]`
- Found 386 diagnostics
+ Found 410 diagnostics

black (https://github.com/psf/black)
- src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- Found 68 diagnostics
+ Found 66 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
- src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult] | StreamItemResult]`
+ src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[StreamItemResult | CoroutineType[Any, Any, StreamItemResult]]`

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_http_request.py:33:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
+ tests/test_http_request.py:37:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[123]`
+ tests/test_http_request.py:207:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def somecallback() -> Unknown`
+ tests/test_http_request.py:300:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def a_function() -> Unknown`
+ tests/test_http_request.py:307:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def a_function() -> Unknown`
+ tests/test_http_request.py:324:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal["a_function"]`
+ tests/test_http_request.py:329:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal["a_function"]`
+ tests/test_http_request.py:430:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `tuple[tuple[Literal["a"], Literal["one"]], tuple[Literal["a"], Literal["two"]], tuple[Literal["b"], Literal["2"]]]`
+ tests/test_http_request.py:432:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `tuple[tuple[Literal["a"], Literal["one"]], tuple[Literal["a"], Literal["two"]], tuple[Literal["b"], Literal["2"]]]`
+ tests/test_http_request.py:447:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | bytes, Unknown | bytes]`
+ tests/test_http_request.py:465:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | str | bytes, Unknown | bytes | str]`
+ tests/test_http_request.py:474:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | bytes, Unknown | bytes]`
+ tests/test_http_response.py:30:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
+ tests/test_http_response.py:35:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[b"http://example.com"]`
+ tests/test_http_response.py:37:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `dict[Unknown, Unknown]`
+ tests/test_http_response.py:72:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal["301"]`
+ tests/test_http_response.py:75:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal["lala200"]`
- Found 1791 diagnostics
+ Found 1808 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/cli/cmds/logs.py:1228:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
- Found 1107 diagnostics
+ Found 1108 diagnostics

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(int, /) -> list[int | float] | int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(int, /) -> list[int | float] | int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
- Found 2053 diagnostics
+ Found 2093 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/PaginatedList.py:372:101: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Requester.py:888:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Requester.py:896:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 354 diagnostics
+ Found 351 diagnostics

flake8 (https://github.com/pycqa/flake8)
+ src/flake8/__init__.py:64:15: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 38 diagnostics
+ Found 39 diagnostics

vision (https://github.com/pytorch/vision)
+ test/test_transforms_v2.py:2572:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[(...) -> Unknown]`, found `(x) -> Unknown`
+ test/test_transforms_v2.py:2572:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[(...) -> Unknown]`, found `(x) -> Unknown`
+ test/test_transforms_v2.py:2572:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[(...) -> Unknown]`, found `(x) -> Unknown`
- Found 1412 diagnostics
+ Found 1415 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/storages/_rdb/storage.py:351:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:351:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:365:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:365:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:367:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:367:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:374:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:374:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:376:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:376:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:384:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:384:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:394:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:394:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:396:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:396:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:651:16: error[invalid-return-type] Return type does not match returned value: expected `int | float`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:651:16: error[invalid-return-type] Return type does not match returned value: expected `int | float`, found `Unknown | Column[int | float]`

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_decorators.py:930:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:932:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:934:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:943:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:945:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:947:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:958:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:960:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:962:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1573 diagnostics
+ Found 1564 diagnostics

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

pybind11 (https://github.com/pybind/pybind11)
+ tests/test_pytypes.py:457:29: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | SupportsBytes | Buffer`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | Buffer`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Buffer`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
- Found 216 diagnostics
+ Found 225 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/types/bigquery.py:231:13: error[invalid-argument-type] Argument is incorrect: Expected `Struct`, found `Type`
+ src/arti/types/pyarrow.py:241:13: error[invalid-argument-type] Argument is incorrect: Expected `Struct`, found `Type`
+ src/arti/types/python.py:158:83: error[invalid-argument-type] Argument is incorrect: Expected `frozenset[Any]`, found `tuple[Any, ...]`
- Found 149 diagnostics
+ Found 152 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ comtypes/test/test_util.py:28:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | float | bytes`
+ comtypes/test/test_util.py:28:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | int | float | bytes`
+ comtypes/test/test_util.py:28:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | bytes | bytearray`, found `Unknown | int | float | bytes`
- Found 391 diagnostics
+ Found 394 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/tests/structure/page_tests.py:539:14: error[no-matching-overload] No overload of function `open` matches arguments
- Found 224 diagnostics
+ Found 225 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/dictionary.py:60:20: error[invalid-return-type] Return type does not match returned value: expected `Valid[Just[A@KeyNotRequired] | Nothing] | Invalid`, found `Valid[Just[Unknown | A@KeyNotRequired]]`
- koda_validate/dictionary.py:67:20: error[invalid-return-type] Return type does not match returned value: expected `Valid[Just[A@KeyNotRequired] | Nothing] | Invalid`, found `Valid[Just[Unknown | A@KeyNotRequired]]`
- Found 407 diagnostics
+ Found 405 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
+ backend/api/global_scoreboard_api.py:104:32: warning[redundant-cast] Value is already of type `str`
- Found 29 diagnostics
+ Found 30 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/read_preferences.py:535:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 448 diagnostics
+ Found 447 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/context.py:485:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/context.py:519:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/converter.py:379:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int | None, int, int]`, found `tuple[(Guild & ~AlwaysTruthy) | None | int, int, int]`
- discord/ext/commands/converter.py:1032:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/converter.py:1036:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/converter.py:1051:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2067:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2069:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2278:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2304:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2429:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/interactions.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionResponse[ClientT@Interaction]`, found `InteractionResponse[Client]`
- Found 555 diagnostics
+ Found 545 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/backends/zarr.py:1090:25: error[invalid-assignment] Object of type `dict[str, bool | dict[Unknown | str, Unknown | bool]]` is not assignable to `dict[str, bool] | dict[str, dict[str, bool]]`
+ xarray/coding/times.py:451:53: error[no-matching-overload] No overload of function `__new__` matches arguments
- xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/resample_cftime.py:409:23: error[no-matching-overload] No overload of function `__new__` matches arguments
- xarray/tests/test_variable.py:2740:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1783 diagnostics
+ Found 1785 diagnostics

apprise (https://github.com/caronc/apprise)
+ apprise/plugins/email/base.py:562:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | int | tuple[Unknown | str] | None`
+ apprise/plugins/email/base.py:562:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | int | tuple[Unknown | str] | None`
+ apprise/plugins/email/base.py:563:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | str | tuple[Unknown | str] | None`
+ apprise/plugins/email/base.py:563:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | str | tuple[Unknown | str] | None`
+ apprise/plugins/fcm/__init__.py:266:13: error[unknown-argument] Argument `user_agent` does not match any known parameter of bound method `__init__`
+ apprise/plugins/fcm/__init__.py:267:13: error[unknown-argument] Argument `timeout` does not match any known parameter of bound method `__init__`
+ apprise/plugins/fcm/__init__.py:268:13: error[unknown-argument] Argument `verify_certificate` does not match any known parameter of bound method `__init__`
- Found 2652 diagnostics
+ Found 2659 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/ninjabackend.py:653:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str] | None`, found `Unknown | _odict_values[str, CustomTarget | BuildTarget]`
+ mesonbuild/cargo/builder.py:191:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `int | str` does not satisfy constraints (`int`, `str`, `bool`) of type variable `TV_TokenTypes`
+ mesonbuild/compilers/detect.py:1151:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | list[str]`, found `None | str | list[str]`
+ mesonbuild/interpreter/interpreter.py:3550:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3550:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3550:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3550:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3551:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ExecutableKeywordArguments`, found `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3551:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `StaticLibraryKeywordArguments`, found `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- Found 1946 diagnostics
+ Found 1955 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_vendor/typing_extensions.py:1416:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:1817:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:1854:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:1958:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:2168:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:2188:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:2248:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- Found 1271 diagnostics
+ Found 1278 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/worksearch/code.py:453:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[tuple[str, str, int]]]`, found `dict[Unknown, Unknown] | None`
+ openlibrary/plugins/worksearch/code.py:453:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[tuple[str, str, int]]]`, found `dict[str, list[tuple[str, str, int]]] | None`

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/cmd/devel/net_convert.py:194:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Unknown, Unknown] | None`, found `MutableMapping[str, Any] | None | Unknown`
- Found 1196 diagnostics
+ Found 1197 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:283:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/federation/schema.py:304:62: warning[unused-ignore-comment] Unused blank

... (truncated 474 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-21 03:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Aconstructor-bindings-refactor?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22124 will **not alter performance**

<sub>Comparing <code>Hugo-Polloli:constructor-bindings-refactor</code> (f298447) with <code>main</code> (da188d5)</sub>



### Summary

`✅ 52` untouched  





---

_Converted to draft by @Hugo-Polloli on 2025-12-21 03:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-21 08:54_

---

_Renamed from "ty: route constructor calls through bindings" to "[ty] route constructor calls through bindings" by @AlexWaygood on 2025-12-21 18:02_

---

_Label `ty` added by @AlexWaygood on 2025-12-21 18:02_

---

_Marked ready for review by @Hugo-Polloli on 2025-12-22 14:59_

---

_Comment by @Hugo-Polloli on 2025-12-22 15:53_

Hi all ! This should be ready for review, though I have a question concerning the test assertions I modified inside functions.md, the previous TODO mentioned no error but to my knowledge the comment I added is correct and the two errors surfaced should indeed appear, so I'd like your input on which it should be ! Thanks :)

---

_Comment by @carljm on 2025-12-23 02:17_

Wow, awesome, thank you for working on this!

It looks like the diagnostics that this PR removes on lines 81 and 82 of the conformance suite, in `constructors_call_type.py`, are supposed to be there, so that looks like a regression in this PR (and maybe some coverage we are missing in mdtests.)

The diagnostic added on line 29 of `generics_scoping.py` is correct, looks like a fix from this PR!

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-23 02:17_

---

_Review comment by @Hugo-Polloli on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:944 on 2025-12-23 12:04_

the previous TODO mentioned no error but to my knowledge the comment I added is correct and the two errors surfaced should indeed appear, so I'd like your input on which it should be, thanks !

---

_@Hugo-Polloli reviewed on 2025-12-23 12:04_

---

_Comment by @Hugo-Polloli on 2025-12-23 12:52_

> Wow, awesome, thank you for working on this!
> 
> It looks like the diagnostics that this PR removes on lines 81 and 82 of the conformance suite, in `constructors_call_type.py`, are supposed to be there, so that looks like a regression in this PR (and maybe some coverage we are missing in mdtests.)
> 
> The diagnostic added on line 29 of `generics_scoping.py` is correct, looks like a fix from this PR!

Indeed looks like a regression ! That was because I missed updating the TypeVar matching arms, so thanks for catching that, I fixed it to validate params against the bound class and added an mdtest (straight copied from typing conformance) to prevent future regression on that side

CI is currently red for reasons that look unrelated to this change: mypy_primer is failing while cloning pypa/twine (git clone … exit 128), and codspeed is panicking. I’ll rerun CI and dig in after Christmas if issues persist :)


---

_Comment by @sinon on 2025-12-23 13:14_

> CI is currently red for reasons that look unrelated to this change: mypy_primer is failing while cloning pypa/twine (git clone … exit 128), and codspeed is panicking. I’ll rerun CI and dig in after Christmas if issues persist :)

The CodSpeed panic is related to your change but not necessarily a blocker to the change. There is an assertion in each benchmarked library that the number of diagnostics are >0 but less than a per-library upperbound but your change means it is now above this bound. If the new diagnostics are not false-positives then you just increase the number in the relevant file (plus some headroom). The relevant setting is here: https://github.com/astral-sh/ruff/blob/5ea30c4c53894dc35fa5af2ca84757ec24f3f7d0/crates/ruff_benchmark/benches/ty_walltime.rs#L197

---

_Comment by @Hugo-Polloli on 2025-12-23 13:52_

> > CI is currently red for reasons that look unrelated to this change: mypy_primer is failing while cloning pypa/twine (git clone … exit 128), and codspeed is panicking. I’ll rerun CI and dig in after Christmas if issues persist :)
> 
> The CodSpeed panic is related to your change but not necessarily a blocker to the change. There is an assertion in each benchmarked library that the number of diagnostics are >0 but less than a per-library upperbound but your change means it is now above this bound. If the new diagnostics are not false-positives then you just increase the number in the relevant file (plus some headroom). The relevant setting is here:
> 
> https://github.com/astral-sh/ruff/blob/5ea30c4c53894dc35fa5af2ca84757ec24f3f7d0/crates/ruff_benchmark/benches/ty_walltime.rs#L197

Thanks for the explanation ! I found the way to run it locally, I'll check the diffs and whether they're regressions/bugs when I get back :)

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 19:56_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 404 | 0 | 20 |
| `unused-ignore-comment` | 1 | 34 | 0 |
| `unknown-argument` | 32 | 0 | 0 |
| `invalid-return-type` | 3 | 5 | 13 |
| `too-many-positional-arguments` | 17 | 0 | 0 |
| `possibly-missing-attribute` | 13 | 0 | 0 |
| `missing-argument` | 12 | 0 | 0 |
| `invalid-assignment` | 2 | 1 | 7 |
| `no-matching-overload` | 7 | 0 | 0 |
| `parameter-already-assigned` | 4 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 3 |
| `redundant-cast` | 2 | 0 | 0 |
| `not-iterable` | 0 | 1 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **497** | **42** | **43** |


**[Full report with detailed diff](https://52074d92.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://52074d92.ty-ecosystem-ext.pages.dev/timing))



---

_Converted to draft by @Hugo-Polloli on 2025-12-26 22:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 23:04_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @Hugo-Polloli on 2025-12-27 11:37_

So I checked the new sympy diagnostics, they come from strict unions of constructors and `type[Self]`. I added corresponding mdtests and TODOs, since some of these can currently be considered false positives, but properly fixing them would require additional modelling (runtime-config constructor aliases, subclass-widened constructors etc...)
I bumped the max diagnostics for sympy

---

_Marked ready for review by @Hugo-Polloli on 2025-12-27 12:37_

---

_Comment by @Hugo-Polloli on 2025-12-27 14:07_

The mypy_primer “duplicates” at `lib/spack/spack/cmd/create.py:1084:29` are from a call on a `Union[type[...]]` (24 variants). `report_diagnostics` currently emits one diagnostic per union element, and with `--output-format concise` the union subdiagnostics aren’t shown so they appear as identical repeated errors.

I found an existing TODO on `report_diagnostics` about aggregating union/overload failures into subdiagnostics, but implementing proper aggregation seems non-trivial and would probably be scope-creep for this PR.

I see two possibilities:
1. we accept the verbose/duplicated-looking output for union calls, follow up later with union-aware diagnostic aggregation (per the TODO).

2. some kind of minimal mitigation in `Bindings::report_diagnostics`, emit only one diagnostic for unions (first failing binding?) and add a subdiagnostic like “omitted N additional union variants”, quicker to do than proper aggregation


---

_Comment by @MichaReiser on 2025-12-29 09:17_

Thank you for working on this. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

---

_Comment by @Hugo-Polloli on 2025-12-29 21:33_

> Thank you for working on this. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

No problem at all, happy holidays! Also wishing the team very well deserved rest!

---

_Comment by @Hugo-Polloli on 2026-01-07 08:59_

Looks like the branch got stale and a conflict appeared, I’m going to rebase onto main to resolve the current conflicts and refresh CI ASAP (prob tonight if everything goes well), I’ll follow up with an update after #22377 lands to adapt this refactor to the new "class decorator calls go through constructor" path.

The open question about mypy_primer remains , if maintainers consider that "duplication" a regression (though from digging a bit in mdtests pertaining to unions it seems consistent with existing behavior?) I can prioritize fixing it before merge.

---

_Comment by @Hugo-Polloli on 2026-01-17 19:11_

I see #22377 has been merged, I will work on starting the rebase work soon :)

---
