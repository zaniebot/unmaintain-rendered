```yaml
number: 19946
title: "[ty] Solve unbound typevar to Unknown & UpperBound"
type: pull_request
state: open
author: hauntsaninja
labels:
  - ty
assignees: []
draft: true
base: main
head: typvarsolve
created_at: 2025-08-17T01:22:12Z
updated_at: 2026-01-09T05:28:53Z
url: https://github.com/astral-sh/ruff/pull/19946
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Solve unbound typevar to Unknown & UpperBound

---

_Pull request opened by @hauntsaninja on 2025-08-17 01:22_

Was just playing around with intersections. I think this is a thing that makes sense and has also come up in real life.

It looks like it is an improvement on some test cases, mostly because ty doesn't yet bind Self. Need to look into the scary cycle error and see if some of the new diagnostics should be suppressed as duplicates

---

_Comment by @github-actions[bot] on 2025-08-17 01:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-09 05:19:53.161452654 +0000
+++ new-output.txt	2026-01-09 05:19:53.484452466 +0000
@@ -429,7 +429,7 @@
 generics_base_class.py:26:26: error[invalid-argument-type] Argument to function `takes_dict_incorrect` is incorrect: Expected `dict[str, list[object]]`, found `SymbolTable`
 generics_base_class.py:29:14: error[invalid-type-form] `typing.Generic` is not allowed in type expressions
 generics_base_class.py:30:8: error[invalid-type-form] `typing.Generic` is not allowed in type expressions
-generics_base_class.py:45:5: error[type-assertion-failure] Type `Iterator[int]` does not match asserted type `Unknown`
+generics_base_class.py:45:5: error[type-assertion-failure] Type `Iterator[int]` does not match asserted type `SupportsNext[Any] & Unknown`
 generics_base_class.py:49:38: error[invalid-type-arguments] Too many type arguments to class `LinkedList`: expected 1, got 2
 generics_base_class.py:61:30: error[invalid-type-arguments] Too many type arguments to class `MyDict`: expected 1, got 2
 generics_basic.py:34:12: error[unsupported-operator] Operator `+` is not supported between two objects of type `AnyStr@concat`
@@ -442,7 +442,7 @@
 generics_basic.py:163:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Protocol`
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:172:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
-generics_basic.py:199:5: error[type-assertion-failure] Type `Iterator[Any]` does not match asserted type `Unknown`
+generics_basic.py:199:5: error[type-assertion-failure] Type `Iterator[Any]` does not match asserted type `SupportsNext[Any] & Unknown`
 generics_defaults.py:24:40: error[invalid-generic-class] Type parameter `T` without a default cannot follow earlier parameter `DefaultStrT` with a default
 generics_defaults.py:30:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults'>`
 generics_defaults.py:31:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
@@ -653,7 +653,9 @@
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
 generics_upper_bound.py:43:1: error[type-assertion-failure] Type `list[int] | set[int]` does not match asserted type `list[Unknown | int] | set[Unknown | int]`
 generics_upper_bound.py:51:8: error[invalid-argument-type] Argument to function `longer` is incorrect: Argument type `Literal[3]` does not satisfy upper bound `Sized` of type variable `ST`
+generics_upper_bound.py:51:8: error[invalid-argument-type] Argument to function `longer` is incorrect: Expected `Sized & Unknown`, found `Literal[3]`
 generics_upper_bound.py:51:11: error[invalid-argument-type] Argument to function `longer` is incorrect: Argument type `Literal[3]` does not satisfy upper bound `Sized` of type variable `ST`
+generics_upper_bound.py:51:11: error[invalid-argument-type] Argument to function `longer` is incorrect: Expected `Sized & Unknown`, found `Literal[3]`
 generics_upper_bound.py:56:10: error[invalid-legacy-type-variable] A `TypeVar` cannot have both a bound and constraints
 generics_variance.py:14:6: error[invalid-legacy-type-variable] A `TypeVar` cannot be both covariant and contravariant
 generics_variance.py:26:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T_co@ImmutableList]`
@@ -709,6 +711,7 @@
 literals_literalstring.py:75:25: error[invalid-assignment] Object of type `Literal[b"test"]` is not assignable to `LiteralString`
 literals_literalstring.py:79:21: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bool`
 literals_literalstring.py:120:22: error[invalid-argument-type] Argument to function `literal_identity` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `TLiteral`
+literals_literalstring.py:120:22: error[invalid-argument-type] Argument to function `literal_identity` is incorrect: Expected `LiteralString & Unknown`, found `str`
 literals_literalstring.py:134:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `T`
 literals_literalstring.py:134:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LiteralString`, found `str`
 literals_literalstring.py:167:1: error[type-assertion-failure] Type `A` does not match asserted type `B`
@@ -1023,4 +1026,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1025 diagnostics
+Found 1028 diagnostics

```

</details>




---

_Comment by @github-actions[bot] on 2025-08-17 01:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `GeneratorType[(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy), None, None]`
- tests/test_validators.py:634:43: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `member_validator`
+ tests/test_validators.py:634:43: error[unresolved-attribute] Object of type `(Any, Attribute[Iterable[Unknown] & Unknown], Iterable[Unknown] & Unknown, /) -> Any` has no attribute `member_validator`
- tests/test_validators.py:635:45: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `iterable_validator`
+ tests/test_validators.py:635:45: error[unresolved-attribute] Object of type `(Any, Attribute[Iterable[Unknown] & Unknown], Iterable[Unknown] & Unknown, /) -> Any` has no attribute `iterable_validator`
- tests/test_validators.py:786:40: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `key_validator`
+ tests/test_validators.py:786:40: error[unresolved-attribute] Object of type `(Any, Attribute[Mapping[Unknown, Unknown] & Unknown], Mapping[Unknown, Unknown] & Unknown, /) -> Any` has no attribute `key_validator`
- tests/test_validators.py:787:42: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `value_validator`
+ tests/test_validators.py:787:42: error[unresolved-attribute] Object of type `(Any, Attribute[Mapping[Unknown, Unknown] & Unknown], Mapping[Unknown, Unknown] & Unknown, /) -> Any` has no attribute `value_validator`
- tests/test_validators.py:788:44: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `mapping_validator`
+ tests/test_validators.py:788:44: error[unresolved-attribute] Object of type `(Any, Attribute[Mapping[Unknown, Unknown] & Unknown], Mapping[Unknown, Unknown] & Unknown, /) -> Any` has no attribute `mapping_validator`
- Found 616 diagnostics
+ Found 617 diagnostics

janus (https://github.com/aio-libs/janus)
+ janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[T@PriorityQueue]`
+ janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)`, found `T@PriorityQueue`
+ janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Expected `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[T@PriorityQueue]`
- Found 3 diagnostics
+ Found 6 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_fileio.py:190:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
+ src/anyio/_core/_fileio.py:190:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer & Unknown] | BinaryIO | IO[Any]`
- src/anyio/_core/_fileio.py:627:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
+ src/anyio/_core/_fileio.py:627:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer & Unknown] | BinaryIO | IO[Any]`

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ tests/type_tests.py:69:5: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...) -> AbstractContextManager[object, (bool & Unknown) | (None & Unknown)] & Unknown`, found `def f() -> None`
- Found 172 diagnostics
+ Found 173 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/audit.py:1341:86: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `set[Spec] & ~AlwaysFalsy`
+ lib/spack/spack/audit.py:1348:63: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `set[Spec] & ~AlwaysFalsy`
+ lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `dict_keys[object, object]`
- lib/spack/spack/cmd/info.py:283:23: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[Unknown]`, found `object`
+ lib/spack/spack/cmd/info.py:283:23: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `object`
+ lib/spack/spack/cmd/providers.py:59:40: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[Spec]`
+ lib/spack/spack/package_base.py:730:25: error[invalid-argument-type] Argument to function `_subkeys` is incorrect: Expected `dict[Spec, dict[SupportsRichComparison & Unknown, Unknown]]`, found `dict[Spec, dict[str, Dependency]]`
+ lib/spack/spack/package_base.py:734:27: error[invalid-argument-type] Argument to function `_by_subkey` is incorrect: Expected `dict[Spec, dict[SupportsRichComparison & Unknown, Unknown]]`, found `dict[Spec, dict[str, Dependency]]`
+ lib/spack/spack/package_base.py:741:16: error[invalid-return-type] Return type does not match returned value: expected `list[str]`, found `list[SupportsRichComparison & Unknown]`
+ lib/spack/spack/package_base.py:741:25: error[invalid-argument-type] Argument to function `_subkeys` is incorrect: Expected `dict[Spec, dict[SupportsRichComparison & Unknown, Unknown]]`, found `dict[Spec, dict[str, Variant]]`
+ lib/spack/spack/package_base.py:750:33: error[invalid-argument-type] Argument to function `_num_definitions` is incorrect: Expected `dict[Spec, dict[SupportsRichComparison & Unknown, Unknown]]`, found `dict[Spec, dict[str, Variant]]`
+ lib/spack/spack/package_base.py:755:29: error[invalid-argument-type] Argument to function `_definitions` is incorrect: Expected `dict[Spec, dict[SupportsRichComparison & Unknown, Variant]]`, found `dict[Spec, dict[str, Variant]]`
+ lib/spack/spack/package_base.py:755:43: error[invalid-argument-type] Argument to function `_definitions` is incorrect: Expected `SupportsRichComparison & Unknown`, found `str`
+ lib/spack/spack/package_base.py:1531:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `set[Spec]`
+ lib/spack/spack/package_base.py:1539:84: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `set[Spec]`
+ lib/spack/spack/solver/asp.py:3137:16: error[invalid-return-type] Return type does not match returned value: expected `list[Spec]`, found `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`
+ lib/spack/spack/test/cmd/deprecate.py:87:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | list[Spec]`
+ lib/spack/spack/test/cmd/deprecate.py:87:44: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | list[Spec]`
+ lib/spack/spack/test/cmd/deprecate.py:89:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | list[Spec]`
+ lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Iterable[Spec]`
+ lib/spack/spack/test/cmd/deprecate.py:90:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | list[Spec]`
+ lib/spack/spack/test/cmd/deprecate.py:90:41: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[Unknown | Spec]`
- lib/spack/spack/test/conftest.py:1886:16: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Element[str] | None`
+ lib/spack/spack/test/conftest.py:1886:16: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Element[(str & Unknown) | (((...) -> Divergent) & Unknown)] | None`
+ lib/spack/spack/test/database.py:440:28: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | list[Spec]`
+ lib/spack/spack/test/spec_semantics.py:2106:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[Spec | Unknown]`
+ lib/spack/spack/test/spec_semantics.py:2107:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `reversed[Spec | Unknown]`
- lib/spack/spack/util/s3.py:140:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `raw` of type `_BufferedReaderStream`
- lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `_BufferedReaderStream | Unknown | None`
+ lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `GeneratorType[(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy), None, None]`
- Found 4302 diagnostics
+ Found 4325 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/network/session.py:246:13: error[invalid-assignment] Object of type `bound method BufferedReader[_BufferedReaderStream].close() -> None` is not assignable to attribute `close` of type `def close(self) -> Unknown`
+ src/pip/_internal/network/session.py:246:13: error[invalid-assignment] Object of type `bound method BufferedReader[_BufferedReaderStream & Unknown].close() -> None` is not assignable to attribute `close` of type `def close(self) -> Unknown`
- src/pip/_vendor/distlib/scripts.py:380:51: warning[possibly-missing-attribute] Attribute `readline` may be missing on object of type `BufferedReader[_BufferedReaderStream] | None`
+ src/pip/_vendor/distlib/scripts.py:380:51: warning[possibly-missing-attribute] Attribute `readline` may be missing on object of type `BufferedReader[_BufferedReaderStream & Unknown] | None`
- src/pip/_vendor/distlib/scripts.py:381:17: warning[possibly-missing-attribute] Attribute `seek` may be missing on object of type `BufferedReader[_BufferedReaderStream] | None`
+ src/pip/_vendor/distlib/scripts.py:381:17: warning[possibly-missing-attribute] Attribute `seek` may be missing on object of type `BufferedReader[_BufferedReaderStream & Unknown] | None`
- src/pip/_vendor/distlib/scripts.py:388:50: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `BufferedReader[_BufferedReaderStream] | None`
+ src/pip/_vendor/distlib/scripts.py:388:50: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `BufferedReader[_BufferedReaderStream & Unknown] | None`
- src/pip/_vendor/urllib3/packages/backports/makefile.py:49:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `BufferedRWPair[SocketIO] | BufferedReader[SocketIO] | BufferedWriter`
+ src/pip/_vendor/urllib3/packages/backports/makefile.py:49:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer & Unknown`, found `BufferedRWPair[SocketIO] | BufferedReader[SocketIO] | BufferedWriter`
- src/pip/_vendor/urllib3/packages/backports/makefile.py:50:5: error[invalid-assignment] Cannot assign to read-only property `mode` on object of type `TextIOWrapper[_WrappedBuffer]`: Attempted assignment to `TextIOWrapper[_WrappedBuffer].mode` here
+ src/pip/_vendor/urllib3/packages/backports/makefile.py:50:5: error[invalid-assignment] Cannot assign to read-only property `mode` on object of type `TextIOWrapper[_WrappedBuffer & Unknown]`: Attempted assignment to `TextIOWrapper[_WrappedBuffer & Unknown].mode` here
- src/pip/_vendor/urllib3/util/ssltransport.py:146:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `BufferedRWPair[SocketIO] | BufferedReader[SocketIO] | BufferedWriter`
+ src/pip/_vendor/urllib3/util/ssltransport.py:146:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer & Unknown`, found `BufferedRWPair[SocketIO] | BufferedReader[SocketIO] | BufferedWriter`
- src/pip/_vendor/urllib3/util/ssltransport.py:147:9: error[invalid-assignment] Cannot assign to read-only property `mode` on object of type `TextIOWrapper[_WrappedBuffer]`: Attempted assignment to `TextIOWrapper[_WrappedBuffer].mode` here
+ src/pip/_vendor/urllib3/util/ssltransport.py:147:9: error[invalid-assignment] Cannot assign to read-only property `mode` on object of type `TextIOWrapper[_WrappedBuffer & Unknown]`: Attempted assignment to `TextIOWrapper[_WrappedBuffer & Unknown].mode` here

bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[Unknown | int | None]`
- Found 80 diagnostics
+ Found 81 diagnostics

asynq (https://github.com/quora/asynq)
+ asynq/decorators.py:280:39: error[invalid-argument-type] Argument to function `__get__` is incorrect: Expected `DecoratorBase[_T@DecoratorBase] & Unknown`, found `(...) -> Unknown`
- Found 217 diagnostics
+ Found 218 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ tests/middleware/test_shared_data.py:45:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_SupportsClose & Unknown`, found `Iterable[bytes]`
+ tests/middleware/test_shared_data.py:54:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_SupportsClose & Unknown`, found `Iterable[bytes]`
- tests/test_wsgi.py:158:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_BufferedReaderStream`, found `LimitedStream`
+ tests/test_wsgi.py:158:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_BufferedReaderStream & Unknown`, found `LimitedStream`
- Found 382 diagnostics
+ Found 384 diagnostics

black (https://github.com/psf/black)
- src/black/trans.py:297:20: error[invalid-raise] Cannot use object of type `object` as an exception cause
- src/black/trans.py:306:24: error[invalid-raise] Cannot use object of type `object` as an exception cause
- src/black/trans.py:493:13: error[unresolved-attribute] Unresolved attribute `__cause__` on type `object`.
- src/black/trans.py:494:13: error[invalid-assignment] Object of type `object` is not assignable to attribute `__cause__` of type `BaseException | None`
- src/black/trans.py:1107:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `(Ok[None] & Top[Err[Unknown]]) | Err[CannotTransform]`
+ src/black/trans.py:1107:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `(Ok[None] & Top[Err[Exception & Unknown]]) | Err[CannotTransform]`
- Found 54 diagnostics
+ Found 50 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/capture.py:200:26: error[unresolved-attribute] Object of type `object` has no attribute `replace`
+ src/_pytest/capture.py:573:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/raises.py:298:16: error[invalid-return-type] Return type does not match returned value: expected `RaisesExc[BaseException] | ExceptionInfo[E@raises]`, found `ExceptionInfo[BaseException]`
- Found 427 diagnostics
+ Found 426 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_contracts.py:263:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WritelnDecorator`, found `None`
+ tests/test_contracts.py:263:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_TextTestStream & Unknown`, found `None`
- tests/test_contracts.py:565:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WritelnDecorator`, found `None`
+ tests/test_contracts.py:565:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_TextTestStream & Unknown`, found `None`

paasta (https://github.com/yelp/paasta)
- paasta_tools/contrib/bounce_log_latency_parser.py:48:54: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:48:54: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> (SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown), (s: slice[Any, Any, Any], /) -> list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]]` cannot be called with key of type `int | float` on object of type `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`
- paasta_tools/contrib/bounce_log_latency_parser.py:49:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:49:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> (SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown), (s: slice[Any, Any, Any], /) -> list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]]` cannot be called with key of type `int | float` on object of type `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`
- paasta_tools/contrib/bounce_log_latency_parser.py:50:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:50:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> (SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown), (s: slice[Any, Any, Any], /) -> list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]]` cannot be called with key of type `int | float` on object of type `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/utilities/lexicographic_sort_schema.py:55:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLList[Unknown] | GraphQLNonNull[Unknown] | GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/utilities/lexicographic_sort_schema.py:55:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLList[GraphQLType & Unknown] | GraphQLNonNull[(GraphQLScalarType & Unknown) | (GraphQLObjectType & Unknown) | (GraphQLInterfaceType & Unknown) | ... omitted 4 union elements] | GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/utilities/lexicographic_sort_schema.py:55:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(GraphQLScalarType & Unknown) | (GraphQLObjectType & Unknown) | (GraphQLInterfaceType & Unknown) | ... omitted 4 union elements`, found `GraphQLList[GraphQLType & Unknown] | GraphQLNonNull[(GraphQLScalarType & Unknown) | (GraphQLObjectType & Unknown) | (GraphQLInterfaceType & Unknown) | ... omitted 4 union elements] | GraphQLNamedType`
+ tests/type/test_validation.py:74:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(GraphQLScalarType & Unknown) | (GraphQLObjectType & Unknown) | (GraphQLInterfaceType & Unknown) | ... omitted 4 union elements`, found `GraphQLNamedType`
- Found 199 diagnostics
+ Found 201 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/views/alerts.py:332:26: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[Unknown | None | datetime]`
- Found 555 diagnostics
+ Found 556 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/aiokits/aiotime.py:20:25: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `list[~None | Unknown] & ~AlwaysFalsy`
- Found 267 diagnostics
+ Found 268 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, (bool & Unknown) | (None & Unknown)]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
- dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, (bool & Unknown) | (None & Unknown)]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/symilar.py:366:41: error[invalid-argument-type] Argument to function `decoding_stream` is incorrect: Expected `BufferedReader[_BufferedReaderStream] | BytesIO`, found `(TextIO & BufferedIOBase) | BufferedReader[_BufferedReaderStream] | BytesIO`
+ pylint/checkers/symilar.py:366:41: error[invalid-argument-type] Argument to function `decoding_stream` is incorrect: Expected `BufferedReader[_BufferedReaderStream & Unknown] | BytesIO`, found `(TextIO & BufferedIOBase) | BufferedReader[_BufferedReaderStream & Unknown] | BytesIO`

rich (https://github.com/Textualize/rich)
- tests/test_console.py:167:13: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `TextIOWrapper[_WrappedBuffer] | None`
+ tests/test_console.py:167:13: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `TextIOWrapper[_WrappedBuffer & Unknown] | None`
- tests/test_console.py:168:13: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `TextIOWrapper[_WrappedBuffer] | None`
+ tests/test_console.py:168:13: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `TextIOWrapper[_WrappedBuffer & Unknown] | None`
- tests/test_console.py:169:13: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `TextIOWrapper[_WrappedBuffer] | None`
+ tests/test_console.py:169:13: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `TextIOWrapper[_WrappedBuffer & Unknown] | None`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/engine/test_engine.py:1222:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `dataiter` on type `Unknown | State`
+ tests/ignite/engine/test_engine.py:1222:9: error[invalid-assignment] Object of type `SupportsNext[Any] & Unknown` is not assignable to attribute `dataiter` on type `Unknown | State`

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/inference.py:379:72: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (RestrictLexicon & Top[dict[Unknown, Unknown]]) | dict[str, RestrictLexicon]`
- Found 416 diagnostics
+ Found 417 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/distribution/opensuse.py:290:18: warning[possibly-missing-attribute] Attribute `iter` may be missing on object of type `Element[str] | None`
+ mkosi/distribution/opensuse.py:290:18: warning[possibly-missing-attribute] Attribute `iter` may be missing on object of type `Element[(str & Unknown) | (((...) -> Divergent) & Unknown)] | None`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/lib/database/redis_utest.py:32:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `redis_client` of type `Redis[Unknown]`
+ dragonchain/lib/database/redis_utest.py:32:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `redis_client` of type `Redis[(str & Unknown) | (bytes & Unknown)]`
- dragonchain/lib/database/redis_utest.py:38:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `redis_client_lru` of type `Redis[Unknown]`
+ dragonchain/lib/database/redis_utest.py:38:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `redis_client_lru` of type `Redis[(str & Unknown) | (bytes & Unknown)]`
- dragonchain/lib/database/redis_utest.py:102:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:102:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:106:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:106:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:110:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].get(name: str | bytes) -> Unknown | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:110:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].get(name: str | bytes) -> (str & Unknown) | (bytes & Unknown) | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:114:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].delete(*names: str | bytes) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:114:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].delete(*names: str | bytes) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:122:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].flushall(asynchronous: bool = False, **kwargs: Any) -> bool` has no attribute `assert_called_once`
+ dragonchain/lib/database/redis_utest.py:122:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].flushall(asynchronous: bool = False, **kwargs: Any) -> bool` has no attribute `assert_called_once`
- dragonchain/lib/database/redis_utest.py:126:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].hdel(name: str | bytes, *keys: str | bytes) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:126:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].hdel(name: str | bytes, *keys: str | bytes) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:130:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].lpush(name: bytes | int | float | str, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:130:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].lpush(name: bytes | int | float | str, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:134:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].rpush(name: bytes | int | float | str, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:134:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].rpush(name: bytes | int | float | str, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:138:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].delete(*names: str | bytes) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:138:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].delete(*names: str | bytes) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:146:9: error[unresolved-attribute] Object of type `Overload[(keys: Iterable[bytes | int | float | str] | int | float, timeout: Literal[0] | None = 0) -> tuple[Unknown, Unknown], (keys: Iterable[bytes | int | float | str] | int | float, timeout: int | float) -> tuple[Unknown, Unknown] | None]` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:146:9: error[unresolved-attribute] Object of type `Overload[(keys: Iterable[bytes | int | float | str] | int | float, timeout: Literal[0] | None = 0) -> tuple[(str & Unknown) | (bytes & Unknown), (str & Unknown) | (bytes & Unknown)], (keys: Iterable[bytes | int | float | str] | int | float, timeout: int | float) -> tuple[(str & Unknown) | (bytes & Unknown), (str & Unknown) | (bytes & Unknown)] | None]` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:150:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].brpoplpush(src, dst, timeout: int | None = 0) -> Unknown` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:150:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].brpoplpush(src, dst, timeout: int | None = 0) -> Unknown` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:154:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].get(name: str | bytes) -> Unknown | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:154:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].get(name: str | bytes) -> (str & Unknown) | (bytes & Unknown) | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:158:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].lindex(name: str | bytes, index: int) -> Unknown | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:158:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].lindex(name: str | bytes, index: int) -> (str & Unknown) | (bytes & Unknown) | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:162:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:162:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:166:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].ltrim(name: str | bytes, start: int, end: int) -> bool` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:166:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].ltrim(name: str | bytes, start: int, end: int) -> bool` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:170:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].hget(name: str | bytes, key: str | bytes) -> Unknown | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:170:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].hget(name: str | bytes, key: str | bytes) -> (str & Unknown) | (bytes & Unknown) | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:174:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].sadd(name: str | bytes, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:174:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].sadd(name: str | bytes, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:178:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].sismember(name: str | bytes, value: bytes | int | float | str) -> bool` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:178:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].sismember(name: str | bytes, value: bytes | int | float | str) -> bool` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:182:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].smembers(name: str | bytes) -> set[Unknown]` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:182:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].smembers(name: str | bytes) -> set[(str & Unknown) | (bytes & Unknown)]` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:186:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].srem(name: str | bytes, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:186:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].srem(name: str | bytes, *values: bytes | int | float | str) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:190:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].lrange(name: str | bytes, start: int, end: int) -> list[Unknown]` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:190:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].lrange(name: str | bytes, start: int, end: int) -> list[(str & Unknown) | (bytes & Unknown)]` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:194:9: error[unresolved-attribute] Object of type `Redis[Unknown]` has no attribute `pipelineassert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:194:9: error[unresolved-attribute] Object of type `Redis[(str & Unknown) | (bytes & Unknown)]` has no attribute `pipelineassert_called_once_with`
- dragonchain/lib/database/redis_utest.py:198:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].llen(name: str | bytes) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:198:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].llen(name: str | bytes) -> int` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:202:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].rpoplpush(src, dst) -> Unknown` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:202:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].rpoplpush(src, dst) -> Unknown` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:206:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].lpop(name, count: int | None = None) -> Unknown` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:206:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].lpop(name, count: int | None = None) -> Unknown` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:210:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].hgetall(name: str | bytes) -> dict[Unknown, Unknown]` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:210:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].hgetall(name: str | bytes) -> dict[(str & Unknown) | (bytes & Unknown), (str & Unknown) | (bytes & Unknown)]` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:214:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].hexists(name: str | bytes, key: str | bytes) -> bool` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:214:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].hexists(name: str | bytes, key: str | bytes) -> bool` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:218:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].zadd(name: str | bytes, mapping: Mapping[str | bytes, bytes | int | float | str], nx: bool = False, xx: bool = False, ch: bool = False, incr: bool = False, gt: Any | None = False, lt: Any | None = False) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:218:9: error[unresolved-attribute] Object of type `bound method Redis[(str & Unknown) | (bytes & Unknown)].zadd(name: str | bytes, mapping: Mapping[str | bytes, bytes | int | float | str], nx: bool = False, xx: bool = False, ch: bool = False, incr: bool = False, gt: Any | None = False, lt: Any | None = False) -> int` has no attribute `assert_called_once_with`

poetry (https://github.com/python-poetry/poetry)
- src/poetry/publishing/uploader.py:220:17: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, str]`, found `tuple[Literal["content"], tuple[str, BufferedReader[_BufferedReaderStream], Literal["application/octet-stream"]]]`
+ src/poetry/publishing/uploader.py:220:17: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, str]`, found `tuple[Literal["content"], tuple[str, BufferedReader[_BufferedReaderStream & Unknown], Literal["application/octet-stream"]]]`

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/specs/openapi/schemas.py:187:48: error[invalid-argument-type] Argument is incorrect: Expected `dict[Unknown, Unknown] & Unknown`, found `None`
+ src/schemathesis/specs/openapi/schemas.py:189:17: error[invalid-argument-type] Argument is incorrect: Expected `ResponsesContainer[Unknown] & Unknown`, found `None`
- Found 278 diagnostics
+ Found 280 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[Unknown]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:382:37: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)`, found `_T@_put`
- tornado/queues.py:385:30: error[invalid-argument-type] Argument to function `heappop` is incorrect: Expected `list[Unknown]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:385:30: error[invalid-argument-type] Argument to function `heappop` is incorrect: Expected `list[(SupportsDunderLT[Any] & Unknown) | (SupportsDunderGT[Any] & Unknown)]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
- Found 328 diagnostics
+ Found 329 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/contrib/wbxml/ASWBXML.py:874:29: error[invalid-argument-type] Argument to bound method `appendChild` is incorrect: Expected `(Comment & Unknown) | (DocumentType & Unknown) | (Element & Unknown) | (ProcessingInstruction & Unknown)`, found `Unknown | CDATASection`
+ mitmproxy/contrib/wbxml/ASWBXML.py:878:29: error[invalid-argument-type] Argument to bound method `appendChild` is incorrect: Expected `(Comment & Unknown) | (DocumentType & Unknown) | (Element & Unknown) | (ProcessingInstruction & Unknown)`, found `Unknown | Text`
- test/mitmproxy/proxy/test_mode_servers.py:65:29: error[unresolved-attribute] Object of type `ServerInstance[Unknown]` has no attribute `_servers`
+ test/mitmproxy/proxy/test_mode_servers.py:65:29: error[unresolved-attribute] Object of type `ServerInstance[ProxyMode & Unknown]` has no attribute `_servers`
- web/gen/state_js.py:30:5: error[unresolved-attribute] Unresolved attribute `_servers` on type `ServerInstance[Unknown]`.
+ web/gen/state_js.py:30:5: error[unresolved-attribute] Unresolved attribute `_servers` on type `ServerInstance[ProxyMode & Unknown]`.
- web/gen/state_js.py:35:5: error[unresolved-attribute] Unresolved attribute `_server` on type `ServerInstance[Unknown]`.
+ web/gen/state_js.py:35:5: error[unresolved-attribute] Unresolved attribute `_server` on type `ServerInstance[ProxyMode & Unknown]`.
- web/gen/state_js.py:36:5: error[unresolved-attribute] Object of type `ServerInstance[Unknown]` has no attribute `_server`
+ web/gen/state_js.py:36:5: error[unresolved-attribute] Object of type `ServerInstance[ProxyMode & Unknown]` has no attribute `_server`
- Found 2147 diagnostics
+ Found 2149 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/_gp/optim_mixed.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float, bool]`, found `tuple[Unknown, ndarray[Unknown, dtype[Any]], Literal[True]]`
+ optuna/_gp/optim_mixed.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown], int | float, bool]`, found `tuple[Unknown, ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown], Literal[True]]`
- optuna/_gp/optim_mixed.py:137:43: error[invalid-argument-type] Argument to function `find_nearest_index` is incorrect: Expected `int | float`, found `ndarray[Unknown, dtype[Any]]`
+ optuna/_gp/optim_mixed.py:137:43: error[invalid-argument-type] Argument to function `find_nearest_index` is incorrect: Expected `int | float`, found `ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown]`
- optuna/_gp/optim_mixed.py:329:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float]`, found `tuple[ndarray[Unknown, dtype[Any]], ndarray[Unknown, dtype[Any]]]`
+ optuna/_gp/optim_mixed.py:329:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown], int | float]`, found `tuple[ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown], ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown]]`
- optuna/_gp/optim_sample.py:19:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float]`, found `tuple[Unknown | ndarray[Unknown, dtype[Any]], ndarray[Unknown, dtype[Any]]]`
+ optuna/_gp/optim_sample.py:19:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown], int | float]`, found `tuple[Unknown | ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown], ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown]]`
- optuna/_gp/search_space.py:73:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `_ScaleType`, found `Unknown | ndarray[Unknown, Unknown]`
+ optuna/_gp/search_space.py:73:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `_ScaleType`, found `Unknown | ndarray[Unknown, dtype[generic[Any] & Unknown] & Unknown]`
- optuna/_gp/search_space.py:74:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `tuple[int | float, int | float]`, fo

... (truncated 10516 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-08-17 01:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/hauntsaninja%3Atypvarsolve?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **not alter performance**

<sub>Comparing <code>hauntsaninja:typvarsolve</code> (a2cd2d0) with <code>main</code> (f9f7a69)</sub>



### Summary

`✅ 14` untouched benchmarks  
`⏩ 39` skipped benchmarks[^skipped]  




[^skipped]: 39 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/hauntsaninja%3Atypvarsolve?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @hauntsaninja on 2025-08-17 01:38_

Great, this also fixes false negatives like:
```
from typing import *

class Base: ...

BaseT = TypeVar("BaseT", bound=Base)

def f(x: Callable[[], BaseT]) -> BaseT:
    return x()

class Unrelated: ...

def make_unrelated() -> Unrelated:
    return Unrelated()

f(Unrelated)  # error
f(make_unrelated)  # error
```

---

_Label `ty` added by @AlexWaygood on 2025-08-17 08:44_

---

_Renamed from "Solve unbound typevar to Unknown & UpperBound" to "[ty] Solve unbound typevar to Unknown & UpperBound" by @AlexWaygood on 2025-08-17 08:45_

---

_Comment by @carljm on 2026-01-09 02:29_

We have much better cycle handling now; would be cool to try reviving this.

---

_Comment by @hauntsaninja on 2026-01-09 05:19_

Did a basic rebase / test update.
Unfortunately it looks like we stack overflow on two tests and hang on two more.
The tests that do run look to mostly have positive changes. We both infer more and issue new diagnostics (like in the test case in my comment above). Errors are a little double reported now, but that can be fixed.

---

_Comment by @carljm on 2026-01-09 05:22_

Hmm, that's a little surprising, especially the hangs. I'll take a look tomorrow. Thanks for giving it a shot. 

---

_Comment by @hauntsaninja on 2026-01-09 05:27_

Seeing what looks like a lot of recursion involving `recursive_type_normalized_impl`

---
