```yaml
number: 20363
title: "[ty] Fix subtyping/assignability of function- and class-literal types to callback protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/callback-proto-false-negatives
created_at: 2025-09-12T10:58:06Z
updated_at: 2025-09-12T21:20:11Z
url: https://github.com/astral-sh/ruff/pull/20363
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Fix subtyping/assignability of function- and class-literal types to callback protocols

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/377.

We were treating any function as being assignable to any callback protocol, because we were trying to figure out a type's `Callable` supertype by looking up the `__call__` attribute on the type's meta-type. But a function-literal's meta-type is `types.FunctionType`, and `types.FunctionType.__call__` is `(...) -> Any`, which is not very helpful!

While working on this PR, I also realised that assignability between class-literals and callback protocols was somewhat broken too, so I fixed that at the same time.

## Test Plan

Added mdtests


---

_Label `ty` added by @AlexWaygood on 2025-09-12 10:58_

---

_Comment by @github-actions[bot] on 2025-09-12 11:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-12 12:44:28.523759463 +0000
+++ new-output.txt	2025-09-12 12:44:31.537763957 +0000
@@ -112,9 +112,23 @@
 callables_kwargs.py:62:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
 callables_kwargs.py:64:11: error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `str`, found `Literal[1]`
 callables_kwargs.py:65:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
+callables_kwargs.py:103:1: error[invalid-assignment] Object of type `def func1(**kwargs: @Todo(`Unpack[]` special form)) -> None` is not assignable to `TDProtocol5`
+callables_kwargs.py:134:1: error[invalid-assignment] Object of type `def func7(*, v1: int, v3: str, v2: str = Literal[""]) -> None` is not assignable to `TDProtocol6`
+callables_protocol.py:35:1: error[invalid-assignment] Object of type `def cb1_bad1(*vals: bytes, *, max_items: int | None) -> list[bytes]` is not assignable to `Proto1`
+callables_protocol.py:36:1: error[invalid-assignment] Object of type `def cb1_bad2(*vals: bytes) -> list[bytes]` is not assignable to `Proto1`
+callables_protocol.py:37:1: error[invalid-assignment] Object of type `def cb1_bad3(*vals: bytes, *, max_len: str | None) -> list[bytes]` is not assignable to `Proto1`
+callables_protocol.py:67:1: error[invalid-assignment] Object of type `def cb2_bad1(*a: bytes) -> Unknown` is not assignable to `Proto2`
+callables_protocol.py:68:1: error[invalid-assignment] Object of type `def cb2_bad2(*a: str, **b: str) -> Unknown` is not assignable to `Proto2`
+callables_protocol.py:69:1: error[invalid-assignment] Object of type `def cb2_bad3(*a: bytes, **b: bytes) -> Unknown` is not assignable to `Proto2`
+callables_protocol.py:70:1: error[invalid-assignment] Object of type `def cb2_bad4(**b: str) -> Unknown` is not assignable to `Proto2`
 callables_protocol.py:97:1: error[invalid-assignment] Object of type `def cb4_bad1(x: int) -> None` is not assignable to `Proto4`
 callables_protocol.py:121:1: error[invalid-assignment] Object of type `def cb6_bad1(*vals: bytes, *, max_len: int | None = None) -> list[bytes]` is not assignable to `NotProto6`
+callables_protocol.py:169:1: error[invalid-assignment] Object of type `def cb8_bad1(x: int) -> Any` is not assignable to `Proto8`
 callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
+callables_protocol.py:238:1: error[invalid-assignment] Object of type `def cb11_bad1(x: int, y: str, /) -> Any` is not assignable to `Proto11`
+callables_protocol.py:260:1: error[invalid-assignment] Object of type `def cb12_bad1(*args: Any, *, kwarg0: Any) -> None` is not assignable to `Proto12`
+callables_protocol.py:284:1: error[invalid-assignment] Object of type `def cb13_no_default(path: str) -> str` is not assignable to `Proto13_Default`
+callables_protocol.py:311:1: error[invalid-assignment] Object of type `def cb14_no_default(*, path: str) -> str` is not assignable to `Proto14_Default`
 callables_subtyping.py:26:5: error[invalid-assignment] Object of type `(int, /) -> int` is not assignable to `(int | float, /) -> int | float`
 callables_subtyping.py:29:5: error[invalid-assignment] Object of type `(int | float, /) -> int | float` is not assignable to `(int, /) -> int`
 classes_classvar.py:38:11: error[invalid-type-form] Type qualifier `typing.ClassVar` expected exactly 1 argument, got 2
@@ -675,6 +689,7 @@
 narrowing_typeis.py:92:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeis.py:96:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
+narrowing_typeis.py:152:26: error[invalid-argument-type] Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
 overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
@@ -854,5 +869,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 855 diagnostics
+Found 870 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @AlexWaygood on 2025-09-12 11:02_

The conformance suite diff looks excellent -- the new diagnostics are all on lines that have `# E` next to them!

---

_Comment by @github-actions[bot] on 2025-09-12 11:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
antidote (https://github.com/Finistere/antidote)
- tests/lib/interface/test_custom.py:434:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:437:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:440:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:443:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:446:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:449:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 316 diagnostics
+ Found 310 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ tests/test_formparser.py:437:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TStreamFactory | None`, found `def stream_factory() -> Unknown`
- Found 370 diagnostics
+ Found 371 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/typing_example.py:100:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `def int_row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[int], /) -> int`
+ tests/typing_example.py:110:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `bound method <class 'Person'>.row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[str], /) -> Person`
+ tests/typing_example.py:134:43: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `AsyncRowFactory[tuple[Any, ...]] | None`, found `def int_row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[int], /) -> int`
+ tests/typing_example.py:144:43: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `AsyncRowFactory[tuple[Any, ...]] | None`, found `bound method <class 'Person'>.row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[str], /) -> Person`
- Found 690 diagnostics
+ Found 694 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/test_styler.py:63:21: error[no-matching-overload] No overload of bound method `apply` matches arguments
+ tests/test_styler.py:259:9: error[type-assertion-failure] Argument does not have asserted type `Styler`
+ tests/test_styler.py:260:26: error[invalid-argument-type] Argument to bound method `map` is incorrect: Expected `_MapCallable`, found `def color_negative(v: @Todo(Support for `typing.TypeAlias`), /, color: str) -> str | None`
- Found 4964 diagnostics
+ Found 4967 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] Fix false negatives when attempting to assign functions to callback protocols" to "[ty] Fix subtyping/assignability of functions and classes to callback protocols" by @AlexWaygood on 2025-09-12 12:31_

---

_Renamed from "[ty] Fix subtyping/assignability of functions and classes to callback protocols" to "[ty] Fix subtyping/assignability of function- and class-literal types to callback protocols" by @AlexWaygood on 2025-09-12 12:32_

---

_Comment by @AlexWaygood on 2025-09-12 16:34_

# Mypy_primer analysis

## psycopg

```diff
psycopg (https://github.com/psycopg/psycopg)
+ tests/typing_example.py:100:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `def int_row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[int], /) -> int`
+ tests/typing_example.py:110:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `bound method <class 'Person'>.row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[str], /) -> Person`
+ tests/typing_example.py:134:43: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `AsyncRowFactory[tuple[Any, ...]] | None`, found `def int_row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[int], /) -> int`
+ tests/typing_example.py:144:43: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `AsyncRowFactory[tuple[Any, ...]] | None`, found `bound method <class 'Person'>.row_factory(cursor: Cursor[Any] | AsyncCursor[Any]) -> (Sequence[str], /) -> Person`
```

This is pretty interesting. Here's a minimal repro of what's going on here:

```py
from __future__ import annotations
from typing import Protocol, Any

def int_maker() -> int:
    raise NotImplementedError

class RowMaker[Row = tuple[Any, ...]](Protocol):
    def __call__(self) -> Row: ...

class Connection[Row = tuple[Any, ...]]:
    @classmethod
    def connect(cls, factory: RowMaker[Row]) -> Row:
        raise NotImplementedError

connect = Connection.connect
reveal_type(connect(int_maker))
```

Mypy issues no complaints on this code and reveals `int` on the last line. Pyright, however, says:

```
Argument of type "() -> int" cannot be assigned to parameter "factory" of type "RowMaker[tuple[Any, ...]]" in function "connect"
  Type "() -> int" is not assignable to type "() -> tuple[Any, ...]"
    Function return type "int" is incompatible with type "tuple[Any, ...]"
      "int" is not assignable to "tuple[Any, ...]"  (reportArgumentType)
Type of "connect(int_maker)" is "tuple[Any, ...]"
```

And with this PR, ty would match pyright's behaviour. The difference comes down to what the inferred type of `connect` is after the `connect = Connection.connect` assignment. Both ty and pyright specialize the method as a result of the binding, and they use the TypeVar default to specialize the method since no explicit specialization was provided for the `Connect` class. This then causes the `connect(int_maker)` call to fail, since `int` is not assignable to `tuple[Any, ...]`, the default specialization of `Connection`. But mypy appears to defer solving the TypeVar somehow, which means that it is able to solve the type variable to `int`.

Anyway, I think the fact that we are emulating pyright's behaviour here means that this is pretty defensible behaviour from us!

## werkzeug

```diff
werkzeug (https://github.com/pallets/werkzeug)
+ tests/test_formparser.py:437:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TStreamFactory | None`, found `def stream_factory() -> Unknown`
```

This seems like a clear true positive. [`TStreamFactory`](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/formparser.py#L40-L47) has a `__call__` method with 3 required parameters, and [`stream_factory`](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/tests/test_formparser.py#L434-L435) will fail at runtime if you provide any parameters to it.

---

Not sure I have the energy on a Friday evening to trawl through the pandas codebase figuring out what the new diagnostics there are about...

---

_Marked ready for review by @AlexWaygood on 2025-09-12 16:36_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-12 16:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-12 16:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-12 16:36_

---

_@carljm approved on 2025-09-12 21:03_

---

_Merged by @AlexWaygood on 2025-09-12 21:20_

---

_Closed by @AlexWaygood on 2025-09-12 21:20_

---

_Branch deleted on 2025-09-12 21:20_

---
