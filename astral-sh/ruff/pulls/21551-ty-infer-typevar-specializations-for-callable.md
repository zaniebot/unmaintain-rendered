```yaml
number: 21551
title: "[ty] Infer typevar specializations for `Callable` types"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/callable-return
created_at: 2025-11-20T23:14:12Z
updated_at: 2025-12-16T17:16:51Z
url: https://github.com/astral-sh/ruff/pull/21551
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Infer typevar specializations for `Callable` types

---

_Pull request opened by @dcreager on 2025-11-20 23:14_

This is a first stab at solving https://github.com/astral-sh/ty/issues/500, at least in part, with the old solver. We add a new `TypeRelation` that lets us opt into using constraint sets to describe when a typevar is assignability to some type, and then use that to calculate a constraint set that describes when two callable types are assignable. If the callable types contain typevars, that constraint set will describe their valid specializations. We can then walk through all of the ways the constraint set can be satisfied, and record a type mapping in the old solver for each one.

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 23:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-16 16:22:20.701518109 +0000
+++ new-output.txt	2025-12-16 16:22:24.208547350 +0000
@@ -150,7 +150,7 @@
 callables_protocol.py:169:7: error[invalid-assignment] Object of type `def cb8_bad1(x: int) -> Any` is not assignable to `Proto8`
 callables_protocol.py:186:5: error[invalid-assignment] Object of type `Literal["str"]` is not assignable to attribute `other_attribute` of type `int`
 callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`.
-callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[(x: int), Unknown]` has no attribute `other_attribute2`
+callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[(x: int), str]` has no attribute `other_attribute2`
 callables_protocol.py:238:8: error[invalid-assignment] Object of type `def cb11_bad1(x: int, y: str, /) -> Any` is not assignable to `Proto11`
 callables_protocol.py:260:8: error[invalid-assignment] Object of type `def cb12_bad1(*args: Any, *, kwarg0: Any) -> None` is not assignable to `Proto12`
 callables_protocol.py:284:27: error[invalid-assignment] Object of type `def cb13_no_default(path: str) -> str` is not assignable to `Proto13_Default`
@@ -236,37 +236,38 @@
 constructors_call_type.py:59:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
 constructors_call_type.py:81:5: error[missing-argument] No argument provided for required parameter `y` of function `__new__`
 constructors_call_type.py:82:12: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `Literal[2]`
-constructors_callable.py:36:13: info[revealed-type] Revealed type: `(x: int) -> Unknown`
-constructors_callable.py:37:1: error[type-assertion-failure] Type `Class1` does not match asserted type `Unknown`
+constructors_callable.py:36:13: info[revealed-type] Revealed type: `(x: int) -> Class1`
 constructors_callable.py:38:1: error[missing-argument] No argument provided for required parameter `x`
 constructors_callable.py:39:1: error[missing-argument] No argument provided for required parameter `x`
 constructors_callable.py:39:4: error[unknown-argument] Argument `y` does not match any known parameter
-constructors_callable.py:49:13: info[revealed-type] Revealed type: `() -> Unknown`
-constructors_callable.py:50:1: error[type-assertion-failure] Type `Class2` does not match asserted type `Unknown`
+constructors_callable.py:49:13: info[revealed-type] Revealed type: `() -> Class2`
 constructors_callable.py:51:4: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
-constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:64:1: error[type-assertion-failure] Type `Class3` does not match asserted type `Unknown`
+constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Class3`
 constructors_callable.py:73:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-constructors_callable.py:77:13: info[revealed-type] Revealed type: `(x: int) -> Unknown`
-constructors_callable.py:78:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
+constructors_callable.py:77:13: info[revealed-type] Revealed type: `(x: int) -> int`
 constructors_callable.py:79:1: error[missing-argument] No argument provided for required parameter `x`
 constructors_callable.py:80:1: error[missing-argument] No argument provided for required parameter `x`
 constructors_callable.py:80:4: error[unknown-argument] Argument `y` does not match any known parameter
 constructors_callable.py:97:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:100:5: error[type-assertion-failure] Type `Never` does not match asserted type `Unknown`
 constructors_callable.py:105:5: error[type-assertion-failure] Type `Never` does not match asserted type `Unknown`
-constructors_callable.py:125:13: info[revealed-type] Revealed type: `() -> Unknown`
-constructors_callable.py:126:1: error[type-assertion-failure] Type `Class6Proxy` does not match asserted type `Unknown`
+constructors_callable.py:125:13: info[revealed-type] Revealed type: `() -> Class6Proxy`
 constructors_callable.py:127:4: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
-constructors_callable.py:142:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:162:5: info[revealed-type] Revealed type: `Overload[(x: int) -> Unknown, (x: str) -> Unknown]`
-constructors_callable.py:164:1: error[type-assertion-failure] Type `Class7[int]` does not match asserted type `Unknown`
-constructors_callable.py:165:1: error[type-assertion-failure] Type `Class7[str]` does not match asserted type `Unknown`
-constructors_callable.py:182:13: info[revealed-type] Revealed type: `(x: list[Unknown], y: list[Unknown]) -> Unknown`
-constructors_callable.py:183:1: error[type-assertion-failure] Type `Class8[str]` does not match asserted type `Unknown`
-constructors_callable.py:193:13: info[revealed-type] Revealed type: `(x: list[T@__init__], y: list[T@__init__]) -> Unknown`
-constructors_callable.py:194:1: error[type-assertion-failure] Type `Class9` does not match asserted type `Unknown`
+constructors_callable.py:141:27: error[invalid-argument-type] Argument to function `accepts_callable` is incorrect: Expected `() -> Any | Class6Any`, found `<class 'Class6Any'>`
+constructors_callable.py:142:13: info[revealed-type] Revealed type: `() -> Any | Class6Any`
+constructors_callable.py:143:1: error[type-assertion-failure] Type `Any` does not match asserted type `Any | Class6Any`
+constructors_callable.py:144:8: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+constructors_callable.py:162:5: info[revealed-type] Revealed type: `Overload[(x: int) -> Class7[int] | Class7[str], (x: str) -> Class7[int] | Class7[str]]`
+constructors_callable.py:164:1: error[type-assertion-failure] Type `Class7[int]` does not match asserted type `Class7[int] | Class7[str]`
+constructors_callable.py:165:1: error[type-assertion-failure] Type `Class7[str]` does not match asserted type `Class7[int] | Class7[str]`
+constructors_callable.py:182:13: info[revealed-type] Revealed type: `(x: list[T@Class8], y: list[T@Class8]) -> Class8[T@Class8]`
+constructors_callable.py:183:1: error[type-assertion-failure] Type `Class8[str]` does not match asserted type `Class8[T@Class8]`
+constructors_callable.py:183:16: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | str]`
+constructors_callable.py:183:22: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | str]`
+constructors_callable.py:184:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | int]`
+constructors_callable.py:184:9: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | str]`
+constructors_callable.py:193:13: info[revealed-type] Revealed type: `(x: list[T@__init__], y: list[T@__init__]) -> Class9`
 constructors_callable.py:194:16: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
 constructors_callable.py:194:22: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
 constructors_callable.py:195:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | int]`
@@ -304,6 +305,10 @@
 dataclasses_transform_converter.py:25:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@model_field`
 dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter1() -> int`
 dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter2(*, x: int) -> int`
+dataclasses_transform_converter.py:102:42: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | bytes, /) -> ConverterClass`, found `<class 'ConverterClass'>`
+dataclasses_transform_converter.py:103:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | list[str], /) -> int`, found `Overload[(s: str) -> int, (s: list[str]) -> int]`
+dataclasses_transform_converter.py:104:30: error[invalid-assignment] Object of type `dict[str, str] | dict[bytes, bytes]` is not assignable to `dict[str, str]`
+dataclasses_transform_converter.py:104:42: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Iterable[list[str]] | Iterable[list[bytes]], /) -> dict[str, str] | dict[bytes, bytes]`, found `<class 'dict'>`
 dataclasses_transform_converter.py:107:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["f1"]`
 dataclasses_transform_converter.py:107:14: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["f2"]`
 dataclasses_transform_converter.py:107:20: error[invalid-argument-type] Argument is incorrect: Expected `ConverterClass`, found `Literal[b"f3"]`
@@ -333,7 +338,8 @@
 dataclasses_transform_converter.py:121:29: error[invalid-argument-type] Argument is incorrect: Expected `ConverterClass`, found `Literal["f6"]`
 dataclasses_transform_converter.py:121:35: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["1"]`
 dataclasses_transform_converter.py:121:40: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str]`, found `tuple[tuple[Literal["a"], Literal["1"]], tuple[Literal["b"], Literal["2"]]]`
-dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
+dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | Literal[1], /) -> int`, found `def converter_simple(s: str) -> int`
+dataclasses_transform_converter.py:133:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | int, /) -> int`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(type[T@create_model], /) -> type[T@create_model]`
 dataclasses_transform_field.py:64:16: error[unknown-argument] Argument `id` does not match any known parameter
 dataclasses_transform_field.py:75:1: error[missing-argument] No argument provided for required parameter `name`
@@ -357,6 +363,7 @@
 dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
 dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
 dataclasses_usage.py:83:13: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+dataclasses_usage.py:88:14: error[invalid-assignment] Object of type `dataclasses.Field[str | int]` is not assignable to `int`
 dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_usage.py:130:1: error[missing-argument] No argument provided for required parameter `y` of bound method `__init__`
 dataclasses_usage.py:179:6: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
@@ -407,7 +414,7 @@
 enums_member_values.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 enums_members.py:82:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x) -> Unknown)`
 enums_members.py:82:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:83:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x: int) -> Unknown)`
+enums_members.py:83:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x: int) -> int)`
 enums_members.py:83:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:84:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `property`
 enums_members.py:84:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -1026,4 +1033,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1028 diagnostics
+Found 1035 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-20 23:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ mypy_primer/model.py:189:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Generator[Path, None, None]`
- Found 4 diagnostics
+ Found 5 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
+ more_itertools/more.py:1057:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `repeat[tuple[Unknown, ...]]`
+ more_itertools/recipes.py:826:25: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `repeat[list[Unknown]]`
+ more_itertools/recipes.py:1106:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, (base: _SupportsPow2[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3NoneOnly[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3[_E_contra@pow, _M_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: _M_contra@pow) -> _T_co@pow, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
- Found 32 diagnostics
+ Found 35 diagnostics

attrs (https://github.com/python-attrs/attrs)
- tests/test_converters.py:113:24: error[unresolved-attribute] Object of type `Converter[Unknown, Unknown]` has no attribute `__call__`
+ tests/test_converters.py:113:24: error[unresolved-attribute] Object of type `Converter[str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc, int]` has no attribute `__call__`
+ tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
+ tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
+ tests/test_validators.py:297:18: error[no-matching-overload] No overload of function `attrib` matches arguments
+ tests/test_validators.py:544:24: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Unknown, ...]`, found `list[Unknown | int]`
+ tests/test_validators.py:555:24: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Unknown, ...]`, found `list[Unknown | int | str]`
+ tests/test_validators.py:700:24: error[invalid-argument-type] Argument is incorrect: Expected `dict[Unknown, Unknown]`, found `None`
+ tests/test_validators.py:787:21: error[invalid-argument-type] Argument to function `and_` is incorrect: Expected `(Any, Attribute[int], int, /) -> Any`, found `(Any, Attribute[Literal[10]], Literal[10], /) -> Any`
- tests/test_validators.py:1143:64: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `exc_types`
+ tests/test_validators.py:1143:64: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown | int], Unknown | int, /) -> Any` has no attribute `exc_types`
+ tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["spam"]`
+ typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
- Found 608 diagnostics
+ Found 617 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:1554:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `StreamProtocol`, found `Unknown | BaseProtocol`
+ src/anyio/_backends/_asyncio.py:2564:50: error[invalid-assignment] Object of type `Future[T_Retval@run_async_from_thread] | Future[_T@run_coroutine_threadsafe]` is not assignable to `Future[T_Retval@run_async_from_thread]`
+ src/anyio/_backends/_asyncio.py:2702:12: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `exception`
+ src/anyio/_backends/_asyncio.py:2704:19: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `exception`
+ src/anyio/_backends/_asyncio.py:2707:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
+ src/anyio/_backends/_asyncio.py:2709:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
+ src/anyio/_backends/_asyncio.py:2928:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `StreamProtocol`, found `BaseProtocol`
+ src/anyio/_backends/_asyncio.py:2939:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
+ src/anyio/_backends/_asyncio.py:2946:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
+ src/anyio/_core/_fileio.py:190:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
+ src/anyio/_core/_fileio.py:627:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
- Found 90 diagnostics
+ Found 101 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/_internal/robot/utils.py:248:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, object]`, found `Mapping[str, object]`
- Found 173 diagnostics
+ Found 174 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/binary_distribution.py:2044:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Sized`
+ lib/spack/spack/detection/common.py:363:25: error[no-matching-overload] No overload of function `__new__` matches arguments
+ lib/spack/spack/detection/common.py:377:20: error[no-matching-overload] No overload of function `__new__` matches arguments
+ lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1672:24: error[no-matching-overload] No overload of function `basename` matches arguments
+ lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `str | PathLike[str]`, found `Unknown | Sized`
+ lib/spack/spack/relocate.py:61:46: error[invalid-argument-type] Argument to function `escape` is incorrect: Argument type `Sized` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ lib/spack/spack/relocate.py:71:59: error[invalid-argument-type] Argument to function `escape` is incorrect: Argument type `Sized` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ lib/spack/spack/relocate.py:78:56: error[invalid-argument-type] Argument to function `escape` is incorrect: Argument type `Sized` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ lib/spack/spack/spec_parser.py:501:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Spec]`, found `Iterator[Spec | None]`
+ lib/spack/spack/vendor/jinja2/environment.py:1494:20: error[invalid-return-type] Return type does not match returned value: expected `list[tuple[int, int]]`, found `list[tuple[int, ...] | Unknown]`
+ lib/spack/spack/verify_libraries.py:155:29: error[no-matching-overload] No overload of function `join` matches arguments
+ lib/spack/spack/verify_libraries.py:164:46: error[invalid-argument-type] Argument to function `candidate_matches` is incorrect: Expected `bytes`, found `bytes | Unknown | str | PathLike[str] | PathLike[bytes]`
+ lib/spack/spack/verify_libraries.py:165:17: error[invalid-assignment] Invalid subscript assignment with key of type `bytes | Unknown | str | PathLike[str] | PathLike[bytes]` and value of type `bytes | Unknown | str | PathLike[str] | PathLike[bytes]` on object of type `dict[bytes, bytes]`
+ lib/spack/spack/verify_libraries.py:170:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[bytes]`, found `list[bytes | Unknown | str | PathLike[str] | PathLike[bytes]]`
+ lib/spack/spack/verify_libraries.py:170:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[bytes]`, found `list[bytes | Unknown | str | PathLike[str] | PathLike[bytes]]`
- Found 4281 diagnostics
+ Found 4297 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `__name__`
+ asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `__name__`
- asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `__name__`
+ asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `__name__`
- asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, ()].asynq(...) -> AsyncTask[Any]`.
+ asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, (...)].asynq(...) -> AsyncTask[Any]`.
- asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `dirty`
+ asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `dirty`
- asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `dirty`
+ asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `dirty`
- asynq/tests/test_typing.py:16:9: error[type-assertion-failure] Type `FutureBase[str]` does not match asserted type `FutureBase[Unknown]`
- asynq/tests/typing_example/param_spec.py:15:21: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `int`, found `Literal["1"]`
- asynq/tests/typing_example/param_spec.py:15:31: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `str`, found `Literal[2]`
- Found 218 diagnostics
+ Found 215 diagnostics

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/asyncio/client.py:471:16: error[invalid-return-type] Return type does not match returned value: expected `ClientConnection`, found `BaseProtocol`
+ src/websockets/asyncio/client.py:799:15: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `response`
+ src/websockets/cli.py:119:43: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `messages`
- Found 41 diagnostics
+ Found 44 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/operations/check.py:99:37: error[no-matching-overload] No overload of function `sorted` matches arguments
+ src/pip/_internal/operations/check.py:101:41: error[no-matching-overload] No overload of function `sorted` matches arguments
+ src/pip/_internal/req/req_uninstall.py:101:13: error[unresolved-attribute] Object of type `Sized` has no attribute `startswith`
+ src/pip/_internal/req/req_uninstall.py:102:17: error[non-subscriptable] Cannot subscript object of type `Sized` with no `__getitem__` method
+ src/pip/_internal/req/req_uninstall.py:106:29: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str`, found `Sized`
+ src/pip/_internal/req/req_uninstall.py:125:16: error[no-matching-overload] No overload of function `normcase` matches arguments
+ src/pip/_internal/req/req_uninstall.py:132:42: error[invalid-argument-type] Argument to function `norm_join` is incorrect: Expected `str`, found `Sized | Unknown`
+ src/pip/_internal/req/req_uninstall.py:133:40: error[invalid-argument-type] Argument to function `norm_join` is incorrect: Expected `str`, found `Sized | Unknown`
+ src/pip/_internal/req/req_uninstall.py:139:27: error[unsupported-operator] Operator `+` is not supported between objects of type `Sized | Unknown` and `LiteralString`
+ src/pip/_vendor/rich/text.py:1285:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(SupportsIndex & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- Found 603 diagnostics
+ Found 613 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/environment.py:1496:20: error[invalid-return-type] Return type does not match returned value: expected `list[tuple[int, int]]`, found `list[tuple[int, ...] | Unknown]`
- Found 181 diagnostics
+ Found 182 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/api.py:636:50: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Iterator[str | Path]`
- Found 32 diagnostics
+ Found 33 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/_reloader.py:149:22: error[not-iterable] Object of type `Sized` is not iterable
+ src/werkzeug/routing/rules.py:510:43: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Mapping[str, Any] & ~AlwaysFalsy`
- tests/test_datastructures.py:582:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(Unknown, /) -> Literal[-1]`, found `<class 'int'>`
- tests/test_datastructures.py:583:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(Unknown, /) -> Literal[-1]`, found `<class 'int'>`
+ tests/test_wrappers.py:729:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `content_length` on type `Response` with custom `__set__` method
- Found 385 diagnostics
+ Found 386 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/setup_prometheus_adapter_config.py:946:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterConfig`, found `dict[Unknown | str, Unknown | list[PrometheusAdapterRule]]`
+ paasta_tools/setup_prometheus_adapter_config.py:946:12: error[invalid-return-type] Return type does not match returned value: expected `PrometheusAdapterConfig`, found `dict[Unknown | str, Unknown | list[PrometheusAdapterRule | Unknown]]`
+ paasta_tools/utils.py:2926:20: error[unresolved-attribute] Object of type `str` has no attribute `decode`
- Found 1105 diagnostics
+ Found 1106 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/path.py:318:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/_py/path.py:759:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
+ src/_pytest/_py/path.py:763:41: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
+ src/_pytest/_py/path.py:808:52: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(path: str | PathLike[str] | None = None) -> Unknown, (path: bytes | PathLike[bytes]) -> Unknown, (path: int) -> Unknown]`, found `Unknown | str`
+ src/_pytest/_py/path.py:817:48: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(path: str | PathLike[str] | None = None) -> Unknown, (path: bytes | PathLike[bytes]) -> Unknown, (path: int) -> Unknown]`, found `Unknown | str`
- Found 440 diagnostics
+ Found 443 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `str`, found `Unknown | str | Confidence`
+ pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `str`, found `Unknown | str | Confidence`
+ pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `Confidence`, found `Unknown | str | Confidence`
+ pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `str`, found `Unknown | str | Confidence`
- Found 212 diagnostics
+ Found 216 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/endpoints.py:42:15: error[call-non-callable] Object of type `CoroutineType[Any, Any, Response]` is not callable
- Found 209 diagnostics
+ Found 210 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/utilities/lexicographic_sort_schema.py:115:32: error[invalid-argument-type] Argument to function `replace_named_type` is incorrect: Expected `GraphQLNamedType`, found `GraphQLNamedType | GraphQLDirective | DirectiveLocation`
+ src/graphql/utilities/lexicographic_sort_schema.py:170:37: error[invalid-argument-type] Argument to function `sort_named_type` is incorrect: Expected `GraphQLNamedType`, found `GraphQLNamedType | GraphQLDirective | DirectiveLocation`
+ src/graphql/utilities/lexicographic_sort_schema.py:177:28: error[invalid-argument-type] Argument to function `sort_directive` is incorrect: Expected `GraphQLDirective`, found `GraphQLDirective | GraphQLNamedType | DirectiveLocation`
+ src/graphql/validation/rules/possible_type_extensions.py:48:29: error[invalid-assignment] Object of type `str | bytes` is not assignable to `str | None`
- tests/pyutils/test_async_reduce.py:50:22: error[invalid-await] `Awaitable[Literal["foo"]] | Literal["foo"]` is not awaitable
+ tests/pyutils/test_async_reduce.py:50:22: error[invalid-await] `Awaitable[Unknown | Literal["foo"]] | Unknown | Literal["foo"]` is not awaitable
- tests/validation/test_no_deprecated.py:23:8: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 427 diagnostics
+ Found 430 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/test_utils.py:147:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = Literal[False], seed_train: int = Literal[13], seed_dev: int = Literal[13], source_text_prefix_token: str = Literal[""], with_n_source_factors: int = Literal[0], with_n_target_factors: int = Literal[0]) -> Iterator[Unknown]`, found `def tmp_digits_dataset(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = Literal[False], seed_train: int = Literal[13], seed_dev: int = Literal[13], source_text_prefix_token: str = Literal[""], with_n_source_factors: int = Literal[0], with_n_target_factors: int = Literal[0]) -> dict[str, Any]`
+ sockeye/test_utils.py:147:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def tmp_digits_dataset(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = Literal[False], seed_train: int = Literal[13], seed_dev: int = Literal[13], source_text_prefix_token: str = Literal[""], with_n_source_factors: int = Literal[0], with_n_target_factors: int = Literal[0]) -> dict[str, Any]`

porcupine (https://github.com/Akuli/porcupine)
+ porcupine/pluginmanager.py:132:49: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `Unknown | str`
- Found 17 diagnostics
+ Found 18 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_downloadermiddleware_httpcompression.py:419:16: error[unresolved-attribute] Object of type `Response` has no attribute `encoding`
+ tests/test_downloadermiddleware_retry.py:37:16: warning[possibly-missing-attribute] Attribute `priority` may be missing on object of type `Unknown | Request | Response`
- Found 1792 diagnostics
+ Found 1794 diagnostics

rich (https://github.com/Textualize/rich)
- examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(length: int = Literal[1]) -> Iterator[Unknown]`, found `def beat(length: int = Literal[1]) -> None`
+ examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def beat(length: int = Literal[1]) -> None`
+ rich/text.py:1285:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(SupportsIndex & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- Found 348 diagnostics
+ Found 349 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/diff_tree.py:421:26: error[invalid-argument-type] Argument to function `_all_eq` is incorrect: Expected `(Unknown, /) -> Literal["delete"]`, found `def change_type(c: TreeChange) -> str`
+ dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/walk.py:474:35: error[invalid-argument-type] Argument to bound method `_reorder` is incorrect: Expected `Iterator[WalkEntry]`, found `Iterator[WalkEntry | None]`
- Found 226 diagnostics
+ Found 228 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
+ ppb_vector/__init__.py:468:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | Vector | Sequence[SupportsFloat]` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ ppb_vector/__init__.py:468:13: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `int | float`, found `Unknown | Vector | Sequence[SupportsFloat]`
- Found 21 diagnostics
+ Found 23 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/inspection/info.py:466:9: error[invalid-assignment] Object of type `Literal["directory"]` is not assignable to attribute `_source_type` on type `PackageInfo | None | Unknown`
+ src/poetry/inspection/info.py:466:9: error[invalid-assignment] Object of type `Literal["directory"]` is not assignable to attribute `_source_type` on type `PackageInfo | None`
- src/poetry/inspection/info.py:467:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `_source_url` on type `PackageInfo | None | Unknown`
+ src/poetry/inspection/info.py:467:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `_source_url` on type `PackageInfo | None`
- src/poetry/inspection/info.py:469:16: error[invalid-return-type] Return type does not match returned value: expected `PackageInfo`, found `PackageInfo | None | Unknown`
+ src/poetry/inspection/info.py:469:16: error[invalid-return-type] Return type does not match returned value: expected `PackageInfo`, found `PackageInfo | None`
- tests/utils/env/test_env.py:518:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/utils/test_password_manager.py:325:5: error[unresolved-attribute] Object of type `bound method PoetryKeyring.get_password(name: str, username: str) -> str | None` has no attribute `assert_not_called`
+ tests/utils/test_password_manager.py:343:5: error[unresolved-attribute] Object of type `bound method PoetryKeyring.get_password(name: str, username: str) -> str | None` has no attribute `assert_not_called`
- Found 969 diagnostics
+ Found 970 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/kcidb/test_kcidb_file.py:83:14: error[missing-argument] No argument provided for required parameter `kcidb_data`
+ tests/test_gitlab.py:231:35: warning[possibly-missing-attribute] Attribute `attributes` may be missing on object of type `Unknown | None`
+ tests/test_gitlab.py:236:35: warning[possibly-missing-attribute] Attribute `attributes` may be missing on object of type `Unknown | None`
+ tests/test_gitlab.py:241:38: warning[possibly-missing-attribute] Attribute `attributes` may be missing on object of type `Unknown | None`
- Found 259 diagnostics
+ Found 261 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/openapi/checks.py:118:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Unknown | list[str | int]`
+ src/schemathesis/specs/openapi/checks.py:491:42: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `set[Unknown | int] | set[int]`
- Found 270 diagnostics
+ Found 272 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/gen.py:239:45: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `Overload[(i: SupportsNext[_T@next], /) -> Unknown, (i: SupportsNext[_T@next], default: _VT@next, /) -> Unknown]`, found `(Generator[Any, Any, _T@coroutine] & Generator[object, None, None]) | (_T@coroutine & Generator[object, None, None])`
+ tornado/gen.py:255:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Generator[None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown], Any, Unknown]`, found `(Generator[Any, Any, _T@coroutine] & Generator[object, None, None]) | (_T@coroutine & Generator[object, None, None])`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:532:47: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]]`, found `dict_values[object, object] | (Sequence[None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]] & ~Top[dict[Unknown, Unknown]]) | (Mapping[Any, None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]] & ~Top[dict[Unknown, Unknown]])`
+ tornado/test/simple_httpclient_test.py:536:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes | None | Unknown, /) -> str | bytes | None | Unknown`, found `Overload[(value: str) -> str, (value: bytes) -> str, (value: None) -> None]`
+ tornado/test/simple_httpclient_test.py:557:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes | None | Unknown, /) -> str | bytes | None | Unknown`, found `Overload[(value: str) -> str, (value: bytes) -> str, (value: None) -> None]`
- tornado/wsgi.py:180:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 323 diagnostics
+ Found 328 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/addons/save.py:101:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BinaryIO`, found `IO[Any]`
+ mitmproxy/dns.py:303:17: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `chain[Question | ResourceRecord]`
+ mitmproxy/proxy/mode_specs.py:217:17: error[invalid-assignment] Property `address` defined in `Self@__post_init__` is read-only
+ mitmproxy/proxy/mode_specs.py:235:9: error[invalid-assignment] Property `scheme` defined in `Self@__post_init__` is read-only
+ mitmproxy/proxy/mode_specs.py:235:22: error[invalid-assignment] Property `address` defined in `Self@__post_init__` is read-only
- test/mitmproxy/addons/test_clientplayback.py:18:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(handle_conn, **server_args) -> AsyncIterator[Unknown]`, found `def tcp_server(handle_conn, **server_args) -> tuple[str, int]`
+ test/mitmproxy/addons/test_clientplayback.py:18:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `def tcp_server(handle_conn, **server_args) -> tuple[str, int]`
- test/mitmproxy/addons/test_next_layer.py:492:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:492:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[HttpLayer] | partial[HttpStream]]`
- test/mitmproxy/addons/test_next_layer.py:524:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:524:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[HttpLayer] | partial[HttpStream]]`
- test/mitmproxy/addons/test_next_layer.py:536:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:536:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[HttpLayer] | partial[HttpStream]]`
- test/mitmproxy/addons/test_next_layer.py:554:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:554:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[HttpLayer] | partial[HttpStream]]`
- test/mitmproxy/addons/test_next_layer.py:575:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:575:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[HttpLayer] | partial[HttpStream]]`
- test/mitmproxy/addons/test_proxyserver.py:337:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(handle_datagram: (DatagramTransport, bytes, tuple[str, int], /) -> None) -> AsyncIterator[Unknown]`, found `def udp_server(handle_datagram: (DatagramTransport, bytes, tuple[str, int], /) -> None) -> tuple[str, int]`
+ test/mitmproxy/addons/test_proxyserver.py:337:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `def udp_server(handle_datagram: (DatagramTransport, bytes, tuple[str, int], /) -> None) -> tuple[str, int]`
+ test/mitmproxy/addons/test_proxyserver.py:491:9: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `close`
+ test/mitmproxy/tools/console/test_quickhelp.py:63:19: error[invalid-argument-type] Argument to bound method `unbind` is incorrect: Expected `Binding`, found `Binding | None`
- Found 2135 diagnostics
+ Found 2142 diagnostics

nox (https://github.com/wntrblm/nox)
+ nox/_option_set.py:63:66: error[invalid-assignment] Object of type `dataclasses.Field[Unknown | str | None]` is not assignable to `None | Literal["auto", "never", "always"]`
+ nox/registry.py:91:16: error[invalid-return-type] Return type does not match returned value: expected `Func | ((((...) -> Any) | Func, /) -> Func)`, found `partial[Func | ((((...) -> Any) | Func, /) -> Func)]`
- Found 23 diagnostics
+ Found 25 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ README.py:637:11: error[invalid-assignment] Object of type `Unknown | range | int` is not assignable to `int`
+ expression/collections/array.py:376:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `Unknown | Self@sum_by | int`
+ expression/collections/array.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `TypedArray[_TSource@TypedArray]`, found `TypedArray[_TSource@TypedArray] | Unknown | _TState@unfold`
+ expression/collections/array.py:655:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[_TSource@unfold] | None`, found `Unknown | _TState@unfold`
+ expression/collections/block.py:274:16: error[invalid-return-type] Return type does not match returned value: expected `_TResult@sum_by`, found `Unknown | Block[_TSourceSum@sum_by]`
+ expression/collections/block.py:486:16: error[invalid-return-type] Return type does not match returned value: expected `Block[_TSource@Block]`, found `Block[_TSource@Block] | Unknown | _TState@unfold`
- expression/collections/block.py:940:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ expression/collections/block.py:595:12: error[invalid-return-type] Return type does not match returned value: expected `Block[_TSource@concat]`, found `Block[_TSource@concat] | Unknown | Iterable[Block[_TSource@concat]]`
+ expression/collections/block.py:1014:18: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[_TSource@unfold]`, found `Unknown | _TState@unfold`
+ expression/collections/map.py:252:24: error[no-matching-overload] No overload of bound method `join` matches arguments
+ expression/collections/seq.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `_TSupportsSum@sum_by`, found `Unknown | Self@sum_by`
+ expression/collections/seq.py:353:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[_TSource@Seq]`, found `Unknown | _TState@unfold | Iterable[_TSource@Seq]`
+ expression/collections/seq.py:446:12: error[invalid-return-type] Return type does not match returned value: expected `Iterable[_TResult@choose]`, found `Unknown | Iterable[_TSource@choose] | Iterable[_TResult@choose]`
+ expression/core/result.py:121:24: error[invalid-return-type] Return type does not match returned value: expected `Result[_TResult@map2, _TErrorOut@Result]`, found `Result[_TResult@map2 | Unknown | _TOther@map2, _TErrorOut@Result]`
- expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some[_T1](value: _T1@Some) -> Option[_T1@Some], (Any, /) -> Option[Any], /) -> def Some[_T1](value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
+ expression/effect/result.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Result[_TResult@bind, _TError@ResultBuilder]`, found `Result[_TResult@bind, _TError@ResultBuilder] | Unknown | Result[_TSource@ResultBuilder, _TError@ResultBuilder]`
+ expression/extra/parser.py:413:13: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Unknown | Block[str] | str | ... omitted 4 union elements, /) -> int`, found `<class 'int'>`
+ expression/extra/parser.py:441:13: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Unknown | Block[str] | str | ... omitted 3 union elements, /) -> float`, found `<class 'float'>`
- expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any], (Any, /) -> Result[Any, Any], /) -> def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
+ tests/test_array.py:47:27: error[invalid-assignment] Object of type `TypedArray[str] | Unknown | TypedArray[Unknown | int]` is not assignable to `TypedArray[str]`
+ tests/test_array.py:58:35: error[invalid-assignment] Object of type `TypedArray[uint8] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[uint8]`
+ tests/test_array.py:69:36: error[invalid-assignment] Object of type `TypedArray[uint16] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[uint16]`
+ tests/test_array.py:80:36: error[invalid-assignment] Object of type `TypedArray[uint32] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[uint32]`
+ tests/test_array.py:91:37: error[invalid-assignment] Object of type `TypedArray[float32] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[float32]`
+ tests/test_array.py:99:35: error[invalid-assignment] Object of type `TypedArray[int16] | Unknown | TypedArray[Unknown | int]` is not assignable to `TypedArray[int16]`
- tests/test_array.py:307:38: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
- tests/test_array.py:322:49: error[invalid-argument-type] Argument to function `unfold` is incorrect: Expected `(Literal[0], /) -> Option[tuple[Unknown, Literal[0]]]`, found `def unfolder(state: int) -> Option[tuple[int, int]]`
- tests/test_block.py:274:38: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
- tests/test_block.py:289:39: error[invalid-argument-type] Argument to function `unfold` is incorrect: Expected `(Literal[0], /) -> Option[tuple[Unknown, Literal[0]]]`, found `def unfolder(state: int) -> Option[tuple[int, int]]`
+ tests/test_seq.py:154:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Literal[0]`
- Found 210 diagnostics
+ Found 226 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/storages/_rdb/alembic/versions/v2.4.0.a.py:181:42: error[invalid-argument-type] Argument to bound method `drop_constraint` is incorrect: Expected `str`, found `str | None`
+ tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:44:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
+ tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:45:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
+ tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:113:13: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
+ tests/samplers_tests/tpe_tests/test_multi_objective_sampler.py:114:13: error[unresolved-attribute] Unresolved attribute `return_value` on type `CallableMixin`.
- Found 547 diagnostics
+ Found 552 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/graphs/__init__.py:358:36: error[invalid-argument-type] Argument to bound method `write_artifact_and_graph_partitions` is incorrect: Expected `Artifact`, found `Artifact | TypedBox[Artifact]`
+ tests/arti/graphs/test_graph.py:450:20: error[unresolved-attribute] Object of type `CallableMixin` has no attribute `called`
+ tests/arti/graphs/test_graph.py:452:16: error[unresolved-attribute] Object of type `CallableMixin` has no attribute `called`
- Found 148 diagnostics
+ Found 151 diagnostics

vision (https://github.com/pytorch/vision)
+ references/depth/stereo/transforms.py:124:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None, Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None], tuple[Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None, Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None, ...], tuple[Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None, ...]]`
+ test/test_datasets.py:873:17: error[invalid-argument-type] Method `__getitem__` of type `bound method VisionDataset.__getitem__(index: int) -> Any` cannot be called with key of type `slice[None, Literal[2], None]` on object of type `VisionDataset`
+ torchvision/utils.py:763:14: error[invalid-assignment] Object of type `list[tuple[int, int, int] | tuple[int, int, int, int] | Unknown]` is not assignable to `None | str | tuple[int, int, int] | list[str | tuple[int, int, int]]`
- Found 1409 diagnostics
+ Found 1412 diagnostics

antidote (https://github.com/Finistere/antidote)
- tests/core/test_thread_safety.py:277:61: error[invalid-argument-type] Argument to bound method `set_value` is incorrect: Expected `() -> Literal["a"]`, found `def callback() -> str`
- Found 273 diagnostics
+ Found 272 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/ext/apidoc/_generate.py:43:21: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["__init__"]` and `Sized | @Todo(StarredExpression)`
+ sphinx/util/_serialise.py:49:16: error[no-matching-overload] No overload of function `sorted` matches arguments
+ sphinx/util/matching.py:112:17: error[no-matching-overload] No overload of function `__new__` matches arguments
- Found 337 diagnostics
+ Found 340 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg/psycopg/_typeinfo.py:200:48: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `(Unknown & ~AlwaysFalsy) | (tuple[int, ...] & ~AlwaysFalsy)`
+ tests/typing_example.py:171:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `BaseRowFactory[int | float]`
+ tests/typing_example.py:174:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `BaseRowFactory[int]`
+ tests/utils.py:114:47: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `(Unknown & ~AlwaysFalsy) | (tuple[int, ...] & ~AlwaysFalsy)`
+ tests/utils.py:123:47: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `(Unknown & ~AlwaysFalsy) | (tuple[int, ...] & ~AlwaysFalsy)`
- Found 682 diagnostics


... (truncated 1735 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~159MB
+ TOTAL MEMORY USAGE: ~167MB
-     struct metadata = ~10MB
+     struct metadata = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~287MB
+ TOTAL MEMORY USAGE: ~301MB


```

</details>




---

_Label `ty` added by @AlexWaygood on 2025-11-21 08:26_

---

_Renamed from "[ty] Infer typevar specializations for `Callable` return types" to "[ty] Infer typevar specializations for `Callable` types" by @dcreager on 2025-11-26 21:26_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:382 on 2025-11-26 23:21_

This is an interesting nuance: `Callable[[A], B]` creates a signature containing positional-only parameters, so we have to make the signature here positional-only as well to make `identity` pass the assignability check for it to be a valid argument to the `fn` parameter.

(That said, I'm not convinced that's...correct? Are we mixing up the ordering of the assignability check operands somewhere?)

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:561 on 2025-11-26 23:29_

This TODO is not also removed because we end up inferring this constraint set when comparing `head` to `Callable[[A], B]`:

```
(B@invoke ≤ T@head) ∧ (list[T@head] ≤ A@invoke)
```

We then try to remove `T@head` from the constraint set by calculating

```
∃T@head ⋅ (B@invoke ≤ T@head) ∧ (list[T@head] ≤ A@invoke)
```

We should be able to pick `T@head = B@invoke` and simplify that to

```
(B@invoke = *) ∧ (list[B@invoke] ≤ A@invoke)
```

which I think would then be enough to propagate through the return type to discharge this TODO. I think this would require adding more derived facts to the sequent map.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/literal_promotion.md`:307 on 2025-11-26 23:30_

This is because we're now using `specialize_recursive` instead of `specialize_partial`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:1718 on 2025-11-26 23:32_

This is the meat of the change. We use the new `ConstraintAssignability` type relation here to calculate a constraint set describing when the two callables are assignable. That will recurse into any typevars in the callables, and find whatever constraint set allows them to unify.

We then use this new `for_each_path` method to find each way that constraint set can be satisfied, and add new typevar bindings to the old solver for whatever we find.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:1343 on 2025-11-26 23:33_

We need to combine this with `result` to hang on to any typevar mapping our constraint set has recorded for the return type comparison above. That's need to support something like

```
Callable[[int], int] ≤ Callable[..., T]
```

and have it return `int ≤ T`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:2100 on 2025-11-26 23:34_

This is what #20093 is trying to add to _replace_ assignability across the board. In the meantime, I've added a new `TypeRelation` that lets us opt into the new behavior only in certain places.

---

_@dcreager reviewed on 2025-11-26 23:34_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-11-26 23:38_

---

_Marked ready for review by @dcreager on 2025-11-26 23:38_

---

_Review requested from @carljm by @dcreager on 2025-11-26 23:38_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-26 23:38_

---

_Review requested from @sharkdp by @dcreager on 2025-11-26 23:38_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 23:46_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 262 | 110 | 143 |
| `type-assertion-failure` | 4 | 56 | 51 |
| `invalid-return-type` | 88 | 0 | 16 |
| `invalid-assignment` | 75 | 0 | 28 |
| `invalid-await` | 101 | 0 | 1 |
| `possibly-missing-attribute` | 63 | 6 | 27 |
| `unresolved-attribute` | 62 | 2 | 28 |
| `unsupported-operator` | 56 | 0 | 19 |
| `missing-argument` | 0 | 59 | 0 |
| `no-matching-overload` | 36 | 8 | 0 |
| `unused-ignore-comment` | 0 | 27 | 0 |
| `not-iterable` | 21 | 0 | 0 |
| `non-subscriptable` | 7 | 0 | 13 |
| `unknown-argument` | 14 | 1 | 0 |
| `too-many-positional-arguments` | 11 | 1 | 0 |
| `call-non-callable` | 9 | 0 | 0 |
| `invalid-context-manager` | 1 | 0 | 2 |
| `invalid-raise` | 0 | 0 | 1 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **811** | **270** | **329** |

**[Full report with detailed diff](https://dcreager-callable-return.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-callable-return.ecosystem-663.pages.dev/timing))




---

_@dhruvmanila reviewed on 2025-11-27 04:30_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:382 on 2025-11-27 04:30_

Yeah, this should work, it does when there aren't any type variables involved (https://play.ty.dev/1663d70f-4daa-492f-84d7-c35c9009fbba):

```py
from typing import Callable

def f(c: Callable[[int], int]): ...

def c(a: int) -> int: return 1

f(c)
```

---

_@dhruvmanila reviewed on 2025-11-27 05:14_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:382 on 2025-11-27 05:14_

Ok, I looked into this a bit more and what I'm seeing is that:

In `infer_map_impl`, the formal signature is `Callable[[A], B]` and the actual signature is the `identity` function and you invoke `formal_signature.when_constraint_set_assignable_to(..., actual_signature, ...)` which means we're checking when is `Callable[[A], B]` assignable to `identity` function which leads to checking when is the positional-only parameter (`A`) is assignable to positional-or-keyword parameter (`a: int`) which is never. This is because you cannot substitute a callable that takes a positional-or-keyword parameter with a callable that takes a positional-only parameter (https://play.ty.dev/7b31e7a0-19c8-4d2b-8bf6-507764372c0d).

This can be tested using `Protocol` to define a callable that takes a positional-or-keyword parameter:
```py
from typing import Protocol

class PositionalOrKeyword(Protocol):
    def __call__(self, a: int) -> None: ...

def positional_only(a: int, /) -> None: ...
def positional_or_keyword(a: int) -> None: ...

def test(c: PositionalOrKeyword) -> None:
    c(a=1)
```

Calling `test` using `positional_only` would fail at runtime.

---

_Comment by @carljm on 2025-12-02 20:51_

Took a look at the conformance suite. Everything looks good except that it seems like we've lost our ability to recognize the `deprecated` decorator? See the conformance suite changes on `directives_deprecated.py`.

---

_Comment by @dcreager on 2025-12-02 20:58_

> Took a look at the conformance suite. Everything looks good except that it seems like we've lost our ability to recognize the `deprecated` decorator? See the conformance suite changes on `directives_deprecated.py`.

Yeah this showed up in the `deprecated` mdtest too. I'm looking at whether it's affected by the same cycle-merging issue as `functools.cache`.

---

_Comment by @AlexWaygood on 2025-12-02 21:14_

Looks like you need to update the number of expected diagnostics for the pandas benchmark

---

_Comment by @dcreager on 2025-12-02 23:13_

> I'm looking at whether it's affected by the same cycle-merging issue as `functools.cache`.

And this is the case...applying `@deprecated` produces a union of two function literals (one with the bottom signature and one with the real signature) due to the oscillation-preventing union. Our overload walking logic assumes that each overload will be a single, bare `Type::FunctionType`, not a union of functions, so it doesn't consider the deprecated overload to be part of the overload chain.

---

_Comment by @dcreager on 2025-12-02 23:26_

> And this is the case...applying `@deprecated` produces a union of two function literals (one with the bottom signature and one with the real signature) due to the oscillation-preventing union. Our overload walking logic assumes that each overload will be a single, bare `Type::FunctionType`, not a union of functions, so it doesn't consider the deprecated overload to be part of the overload chain.

I've pushed up a very hacky temporary fix for this — when walking overloads, if we come across a union of function literals, we pull out the one that came from the last salsa cycle iteration and use that as the type of that overload. I'm also looking at trying to eliminate the cycles entirely, but that's proving to be an invasive change, and I don't want to block this further.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-02 23:29_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-02 23:29_

---

_Comment by @dcreager on 2025-12-02 23:45_

That's a whole lot of ecosystem churn that I need to dig into

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-03 00:02_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-03 00:02_

---

_Comment by @carljm on 2025-12-03 01:00_

Conformance suite results (and local testing with CLI runs) still says that even with the hacky fix, `@deprecated` is no longer working for functions. Sadly it seems to work in mdtests, but the same examples do not work when run in CLI, so the cycle problem is sensitive to something that's different in the way mdtests are run (and thus mdtests are not giving us an accurate picture of what functionality actually works, either.)

Another option for the cycle problems is to introduce hacks in `cycle_normalized` to avoid creating unions of certain known-problematic types like `FunctionLiteral` (and cross our fingers that this doesn't also reintroduce oscillation panics involving those types).

---

_Comment by @dcreager on 2025-12-03 02:47_

> Conformance suite results (and local testing with CLI runs) still says that even with the hacky fix, `@deprecated` is no longer working for functions. Sadly it seems to work in mdtests, but the same examples do not work when run in CLI, so the cycle problem is sensitive to something that's different in the way mdtests are run (and thus mdtests are not giving us an accurate picture of what functionality actually works, either.)

The first attempt at the hack only engaged when walking the overload chain, which meant that it never engaged for the implementation of an overloaded function, or for non-overloaded functions. We weren't seeing this in the mdtest suite because this wasn't enough to trigger a salsa cycle:

```py
@deprecated("use OtherClass")
def myfunc(): ...
```

Adding a parameter to `myfunc` is enough to trigger a cycle, and then we get the same failure as in the conformance suite.

> Another option for the cycle problems is to introduce hacks in `cycle_normalized` to avoid creating unions of certain known-problematic types like `FunctionLiteral` (and cross our fingers that this doesn't also reintroduce oscillation panics involving those types).

I went with this. The first hack attempt was crossing fingers like this anyway, but only for overloads. I moved the logic into `cycle_normalized` to make it engage for all parts of a function definition. Let's see how it affects the conformance suite and ecosystem...

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-03 02:55_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-03 02:55_

---

_Comment by @codspeed-hq[bot] on 2025-12-03 03:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fcallable-return?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21551 will **degrade performances by 6.04%**

<sub>Comparing <code>dcreager/callable-return</code> (0cc5e03) with <code>main</code> (7f7485d)</sub>



### Summary

`❌ 2 (👁 2)` regressions  
`✅ 50` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fcallable-return?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5 s | 5.3 s | -6.04% |
| 👁 | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fcallable-return?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 209.3 s | 219.1 s | -4.49% |


---

_Comment by @dcreager on 2025-12-03 15:39_

The remaining conformance suite changes show us correctly inferring a specialization from the return type of a `Callable` parameter in most cases :tada:  There are still some places where we're not handling `ParamSpec`s in those callables, but that's coming soon!

[`Class7`](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/constructors_callable.py#L151-L165) and [`Class8`](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/constructors_callable.py#L176-L184) are the examples where we are still not inferring the _correct_ return type specialization. Both of those are generic.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-03 21:48_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-03 21:48_

---

_Comment by @carljm on 2025-12-03 22:57_

@dcreager Just came to take another review look at this -- looks like this is introducing a new fuzzer panic, and a number of other CI jobs are timing out? I'm sure you're on it, but let me know if you'd like a second pair of eyes (or just a rubber duck) on any of those issues.

---

_Comment by @dcreager on 2025-12-04 02:41_

> looks like this is introducing a new fuzzer panic, and a number of other CI jobs are timing out?

I fixed the fuzzer panic, but still seeing timeouts on some of the test repos, which is affecting the mypy_primer job and one of the wallclock jobs. `xarray` is one of the problematic repos, and in particular the `xarray/groupers.py` file in the repo. I'm letting Codex take a stab at diagnosing where the increased running time is coming from.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-04 15:01_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-04 15:01_

---

_Comment by @dcreager on 2025-12-04 15:05_

> `xarray` is one of the problematic repos, and in particular the `xarray/groupers.py` file in the repo. I'm letting Codex take a stab at diagnosing where the increased running time is coming from.

Codex helped pinpoint that the issue was [this call](https://github.com/pydata/xarray/blob/50218d01e4e0ede24c9f34c8974545b9cb0dc6fd/xarray/groupers.py#L569-L571) involving `slice`:

```py
group_indices: GroupIndices = tuple(
    list(itertools.starmap(slice, pairwise(sbins))) + [slice(sbins[-1], None)]
)
```

It turns out that `slice.__new__` is very overloaded, and many of the overloads are generic. When checking assignability of an overloaded `Callable`, I was taking the constraint sets for each overload (each _pair_ of overloads, actually, since both sides can be overloaded) and merging them together using OR. Then I would use `walk_path` on that single constraint set to pull out any assignments we should add to the old solver.

That was building up a very large BDD, that was taking a long time to iterate through. But since each of those overload pairs is independent, we can just do the path walk on each one separately. That gets rid of the combinatorial explosion. `mypy_primer` has already passed without timeout, and `xarray` worked fine locally even in the `debug` profile.

So I'm waiting for CI to confirm, but I think the timeouts have been solved now too.

---

_Comment by @dcreager on 2025-12-04 15:48_

Still in the middle of reviewing the ecosystem results, but this one stands out as interesting:

```
[error] invalid-argument-type - [:1257:20] - Argument is incorrect: Expected `int | float`, found `Literal["spam"]`
```

It looks like we're now correctly catching a [runtime `attrs` error](https://github.com/python-attrs/attrs/blob/c0111e6aad3a70080b7191aa9c7eda459a25f3dc/tests/test_validators.py#L1257)  during type-checking!

---

_Converted to draft by @dcreager on 2025-12-05 23:42_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-09 21:33_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-09 21:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md`:886 on 2025-12-10 00:58_

This is an error condition anyway, and I find it better to infer `Unknown` here than to let the typevar leak through when we were supposed to have solved it.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/decorators.md`:152 on 2025-12-10 01:00_

The lingering `Unknown`s are because of the function definition salsa cycle issue https://github.com/astral-sh/ty/issues/1729

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:738 on 2025-12-10 01:08_

This change makes us group all of the typevars bound by a single definition together in a constraint set BDD

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2024 on 2025-12-10 01:10_

This corresponds with [this TODO](https://github.com/astral-sh/ruff/pull/21551/files#r2566717765) in the mdtests

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:1837 on 2025-12-10 01:14_

This logic has moved over to `CallableSignature`

---

_@dcreager reviewed on 2025-12-10 01:33_

---

_Marked ready for review by @dcreager on 2025-12-10 01:33_

---

_Comment by @dcreager on 2025-12-10 01:54_

There are quite a lot of new ecosystem diagnostics, but they are primarily due to not inferring specializations involving generic protocols yet.

---

_Comment by @carljm on 2025-12-10 02:38_

Looking at the conformance suite results:

* The new error at https://github.com/python/typing/blob/main/conformance/tests/callables_protocol.py#L199 looks pretty weird. I think it's the cycle problem... suggests that getting that fixed will be pretty important to seeing practical benefits from this PR in decorator inference?
* ... have to go, will need to pick this up later...

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-11 21:22_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-11 21:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 23:31_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-12 00:38_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-12 00:38_

---

_Comment by @carljm on 2025-12-12 01:09_

In this conformance suite test (from `constructors_callable.py`), we infer the callable type `() -> Class3` (instead of the expected `(x: int) -> Class3`), and then we emit an error (on the same call to `accepts_callable` where we infer that type) that `Class3` is not assignable to `() -> Class3`. We can file a separate issue to follow up on this, just wanted to highlight it.

```py
P = ParamSpec("P")
R = TypeVar("R")

def accepts_callable(cb: Callable[P, R]) -> Callable[P, R]:
    return cb

class Class3:
    """__new__ and __init__"""

    def __new__(cls, *args, **kwargs) -> Self: ...

    def __init__(self, x: int) -> None: ...


r3 = accepts_callable(Class3)
reveal_type(r3)  # `def (x: int) -> Class3`
assert_type(r3(3), Class3)
r3()  # E
r3(y=1)  # E
r3(1, 2)  # E
```

---

_Comment by @carljm on 2025-12-12 01:14_

Similarly in this test we infer `() -> Never` instead of the expected `(*args: Any, **kwargs: Any) -> Never`:

```py
class Meta1(type):
    def __call__(cls, *args: Any, **kwargs: Any) -> NoReturn:
        raise NotImplementedError("Class not constructable")


class Class5(metaclass=Meta1):
    """Custom metaclass that overrides type.__call__"""

    def __new__(cls, *args: Any, **kwargs: Any) -> Self:
        """This __new__ is ignored for purposes of conversion"""
        return super().__new__(cls)

r5 = accepts_callable(Class5)
reveal_type(r5)  # `def (*args: Any, **kwargs: Any) -> NoReturn`
```

This looks similar to the previous issue, like we have an issue with defaulting ParamSpecs to "zero args" when inferring them from a class constructor.

---

_Comment by @carljm on 2025-12-12 01:18_

EDIT: this is clearly marked as a TODO already in the tests, we can ignore for now (but I'll leave this here and create an issue from it.)

In this case we infer a return type of `Class7[int] | Class7[str]` for both overloads, where we should have a return type of `Class7[int]` on one overload and `Class7[str]` on the other:

```py
class Class7(Generic[T]):
    @overload
    def __init__(self: "Class7[int]", x: int) -> None: ...
    @overload
    def __init__(self: "Class7[str]", x: str) -> None: ...
    def __init__(self, x: int | str) -> None:
        pass


r7 = accepts_callable(Class7)
reveal_type(
    r7
)  # overload of `def (x: int) -> Class7[int]` and `def (x: str) -> Class7[str]`
assert_type(r7(0), Class7[int])
assert_type(r7(""), Class7[str])
```

---

_Comment by @carljm on 2025-12-12 01:23_

EDIT: this is also already clearly marked as a TODO in the tests.

In both of these cases, we don't re-bind the type variable to the inferred callable type, so we aren't able to infer it in calls to that callable:

```py
class Class8(Generic[T]):
    def __new__(cls, x: list[T], y: list[T]) -> Self:
        return super().__new__(cls)


r8 = accepts_callable(Class8)
reveal_type(r8)  # `def [T] (x: list[T], y: list[T]) -> Class8[T]`
assert_type(r8([""], [""]), Class8[str])
r8([1], [""])  # E

class Class9:
    def __init__(self, x: list[T], y: list[T]) -> None:
        pass


r9 = accepts_callable(Class9)
reveal_type(r9)  # `def [T] (x: list[T], y: list[T]) -> Class9`
assert_type(r9([""], [""]), Class9)
r9([1], [""])  # E
```

---

_Comment by @carljm on 2025-12-12 01:25_

(I'm not assuming we should fix these in this PR, they can be follow-ups; this is just a convenient place to record my findings for now, can convert them to new issues when this lands.)

---

_Comment by @carljm on 2025-12-12 02:14_

I'm seeing a regression introduced by this PR in a number of ecosystem projects, where it looks like we are taking the bottom materialization of argument types when we infer a ParamSpec, where before this PR we did not. Minimal repro:

```py
from typing import Callable

def callable_identity[**P, R](func: Callable[P, R]) -> Callable[P, R]:
    return func

@callable_identity
def f(env: dict) -> None:
    pass

reveal_type(f)
```

Before this PR we reveal `(env: dict[Unknown, Unknown]) -> Unknown`.
On this PR we reveal `(env: Bottom[dict[Unknown, Unknown]]) -> None`.

This is an improvement on the return type, of course, but a major problem for the argument type, since there is no way to provide a value assignable to `Bottom[dict[Unknown, Unknown]]`.

This also manifests in some cases as wrongly inferring a callable that takes no parameters, when something like `staticmethod(...)` is called on a callable with gradual parameter list `...`. The reason is that we currently have some (wrong) logic that the bottom materialization of a gradual callable is the empty parameter list. Pretty sure this is also the root cause of some of the conformance suite issues I noted above where we end up wrongly inferring an empty argument list. 

Correct materialization of gradual callables is probably higher priority now that we have `ParamSpec` inference support, though it wouldn't matter here if we weren't wrongly taking the bottom materialization.

I'm seeing these issues in enough ecosystem projects that I think it might be a blocker here. It often comes up, for example, with use of `contextlib.contextmanager`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:382 on 2025-12-12 02:27_

It looks like this comment still needs follow-up? Why are we checking assignability in the wrong direction (requiring the formal signature to be assignable to the actual signature)? I don't think the change here and below to make `identity` and `head` take a positional-only parameter should be required.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:493 on 2025-12-12 02:29_

Similarly here, I think these changes should not be required, and it suggests a problem in this PR if they are.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:823 on 2025-12-12 02:37_

On this test (and maybe some others, which look derived from the conformance suite) we are now getting the return type correct, but (untested here) we are regressing on the signature, due to the issue mentioned elsewhere where we are now wrongly taking bottom materializations of inferred signatures.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:682 on 2025-12-12 02:44_

Hmm, why do we lose the parameters here?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:214 on 2025-12-12 02:48_

Is this where the wrong bottom materializations are coming from?

I think in the ecosystem I also saw some cases of top materializations inferred where they shouldn't be...

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:766 on 2025-12-12 02:52_

It seems like we are making this change a lot, which suggests we would be better able to handle gradual bounds now?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:1887 on 2025-12-12 03:01_

Is it backwards to check assignability of the formal callable to the actual callable?

Is this why we wrongly had to adjust some tests to make the argument callables take a positional-only parameter?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:936 on 2025-12-12 03:08_

We could check that the two function literals are coming from the same definition (though if they are the result of the same query it seems like maybe they would have to be?)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1863 on 2025-12-12 03:11_

Is this the place where we'd want to re-bind typevars to the new callable type? Should we add a TODO for that?

---

_@carljm reviewed on 2025-12-12 03:19_

Looking great overall! A few questions and issues that I see in the ecosystem report.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:358 on 2025-12-12 12:18_

Can you add a TODO comment here stating that this might need to be updated once `Concatenate` is supported?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:389 on 2025-12-12 12:29_

Can you say why? Is it about overloads? I think it should still be on `Signature` given that it could be different for each overload.

I think for an overloaded function that's being matched against e.g., `Callable[P, int]`, the typevar would need to be constrained to match only those callables whose return type is assignable to `int`. There might be other examples.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:397 on 2025-12-12 12:44_

Would this need to be moved to `Signature` once overloads are supported?

During the initial implementation of `ParamSpec`, when I started off by trying to use the new solver, I had a similar `match` expression in `Signature::has_relation_impl_inner` after the `is_gradual` checks where I'd try to add similar constraints.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:12548 on 2025-12-12 12:47_

Should this return a slice instead (`&[CallableType<'db>]`) ?

---

_@dhruvmanila reviewed on 2025-12-12 12:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:493 on 2025-12-12 18:06_

Fixed with the parameter ordering change you mentioned below

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:382 on 2025-12-12 18:06_

Fixed with the parameter ordering change you mention below

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:682 on 2025-12-13 02:56_

This is an example where we're now collectively taking into account all of the constraints across the entire call site. In this case, we're correctly flagging `invalid-argument-type`, because `change_return_type` expects a callable that returns an `int`, which neither overload of `str_str` does. Before we went ahead and inferred a specialization of `P` from the callable parameter, even though it was invalid. But now, since the argument is invalid, we don't infer anything for `P`, and fall back on its `Unknown` equivalent `(...)`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:936 on 2025-12-13 03:01_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:1887 on 2025-12-13 03:03_

<img width="100" height="100" alt="Image" src="https://github.com/user-attachments/assets/55546547-ba7f-4188-a011-b8de5578f330" />

Yes, yes it was. `infer` has the parameters in opposite order from `has_relation_to`, and I copy-pasted this badly. Had to add a bit of extra logic ot `add_type_mappings_from_constraint_set`, but this is now in the correct order.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:766 on 2025-12-13 03:04_

As discussed in discord, we're going ahead and doing this in a separate PR, https://github.com/astral-sh/ruff/pull/21957. That means that as of now, constraint sets cannot be used to track subtyping queries, but we will revisit that in the future if/when we need it.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1863 on 2025-12-13 03:05_

I'm not sure I follow — which rebinding do you mean?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:358 on 2025-12-13 03:06_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:389 on 2025-12-13 03:17_

My thinking is that the following are two different things:

```py
def f(*args: P.args, **kwargs: P.kwargs) -> whatever: ...
Callable[P, whatever]
```

The first one is definitely scoped to a single overload. `P` is always in non-inferable position when you use `P.args` and `P.kwargs`.

But the second one seems like `P` is scoped to more than one overload. It's often in inferable position. When it is, and we're checking if some other callable is assignable to the `Callable[P, ...]`, we need to look at all of the overloads of the other callable to see which ones match. I'm not sure how you'd get different (inferable) `P`s for different overloads. Do you have an example?

---

_@dcreager reviewed on 2025-12-13 03:26_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-13 12:22_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-13 12:22_

---

_@AlexWaygood reviewed on 2025-12-13 21:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 21:38_

`order_elements(true)` orders the union elements using `union_or_intersection_elements_ordering`. That comparison function _doesn't_ guarantee stable ordering of union elements between different runs of ty, which is what would be required for stable mdtest revealed types. It only guarantees stable ordering within the context of a single invocation of ty.

---

_@carljm reviewed on 2025-12-13 21:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 21:43_

Oops that was my suggestion (on the previous PR)

---

_@AlexWaygood reviewed on 2025-12-13 21:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 21:48_

hmm, yeah... not sure it's ever really correct to set `builder.order_elements(true)` outside of a `Type::normalized(_impl)` call? The uses in `constraints.rs` look a bit suspect to me. (Were those the ones that were added on the previous PR? If it made the `reveal_type` output stable in that PR, I worry that that might have just been luck/chance...)

We should add some docs to the `UnionBuilder::order_elements()` method that make this limitation clear.

---

_@carljm reviewed on 2025-12-13 21:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 21:55_

The `order_elements()` method itself was added in the previous PR, in order to enable the uses in `constraints.rs`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 21:56_

no it wasn't, you added it in https://github.com/astral-sh/ruff/pull/20147 back in August

---

_@AlexWaygood reviewed on 2025-12-13 21:56_

---

_@carljm reviewed on 2025-12-13 21:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 21:59_

It was added to intersection builder in the previous PR, which is what is used in constraints.rs; previously it existed only on union builder

---

_@AlexWaygood reviewed on 2025-12-13 22:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 22:00_

oh _you're_ talking about `IntersectionBuilder::order_elements()`? That one was added yesterday, yes

---

_@carljm reviewed on 2025-12-13 22:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 22:01_

Yes, and that's the one this thread is about, given the code it is commenting on 😆

---

_@AlexWaygood reviewed on 2025-12-13 22:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 22:01_

> It was added to intersection builder in the previous PR, which is what is used in constraints.rs

(no, constraints.rs uses both `UnionBuilder::order_elements()` _and_ `IntersectionBuilder::order_elements()`)

---

_@AlexWaygood reviewed on 2025-12-13 22:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 22:04_

> Yes, and that's the one this thread is about, given the code it is commenting on 😆

um, well, the first two lines of the codeframe my original comment is attached to are

```rs
                let new_greatest_lower_bound = UnionBuilder::new(db)
                    .order_elements(true)
```

so... no, `IntersectionBuilder::order_elements()` is not what this thread was originally about? 😆


---

_@carljm reviewed on 2025-12-13 22:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-13 22:19_

Sorry, shouldn't comment from my phone. The ordering changes in yesterday's PR were originally motivated by intersections, I didn't notice that it started ordering unions too. We should revert all uses of order_elements here (separate PR maybe) and figure out a different way to address the original instability. 

---

_@dcreager reviewed on 2025-12-14 01:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-14 01:33_

I jumped right back into the terminal window this evening without seeing this thread until now. Trying to clarify my understanding: Do we have anything currently that guarantees a stable ordering across runs? Does `normalized` do that? If so, is that because it applies `order_elements` recursively to the elements of the types as well? If not, how are we not running into these same instabilities in other mdtests? Is it because without `order_elements`, unions and intersections retain the ordering of the caller, and we're very careful to build up unions/intersections in all of those places in a reproducible way?

---

_@AlexWaygood reviewed on 2025-12-14 01:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-14 01:42_

> Do we have anything currently that guarantees a stable ordering across runs?

Not, currently, no

> Does `normalized` do that?

No, only within a single run 

> If not, how are we not running into these same instabilities in other mdtests? Is it because without `order_elements`, unions and intersections retain the ordering of the caller, and we're very careful to build up unions/intersections in all of those places in a reproducible way?

Yes, we always retain insertion order for unions and intersections. Unions and intersections are generally created either directly as a result of a user annotation, or as a result of control flow. In either case, insertion order is always guaranteed to be stable between runs of ty

---

_@dcreager reviewed on 2025-12-15 03:04_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-15 03:04_

Thanks for clarifying that, @AlexWaygood! I've got most of way done with adding a proper stable ordering on https://github.com/astral-sh/ruff/pull/21983, just a little bit more to clean up on that tomorrow.

---

_Comment by @dcreager on 2025-12-15 16:15_

> I'm seeing a regression introduced by this PR in a number of ecosystem projects, where it looks like we are taking the bottom materialization of argument types when we infer a ParamSpec, where before this PR we did not. Minimal repro:
> 
> ```python
> from typing import Callable
> 
> def callable_identity[**P, R](func: Callable[P, R]) -> Callable[P, R]:
>     return func
> 
> @callable_identity
> def f(env: dict) -> None:
>     pass
> 
> reveal_type(f)
> ```

I added this as an mdtest

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:12548 on 2025-12-15 16:16_

Yes, thank you!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:397 on 2025-12-15 16:18_

I think this is related to your suggestion above — I had this in `Signature` at first too, but had to move it up here to make some of the tests pass. (The ones where the paramspec needs to find a full overload set of the other side).

But if we need to have this match the subset of the overloads that match the paramspec, then I think you're right? I'll add a note.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:214 on 2025-12-15 16:20_

This is now moot since we allow gradual lower/upper bounds

---

_@dcreager reviewed on 2025-12-15 17:16_

> In this conformance suite test (from `constructors_callable.py`), we infer the callable type `() -> Class3` (instead of the expected `(x: int) -> Class3`), and then we emit an error (on the same call to `accepts_callable` where we infer that type) that `Class3` is not assignable to `() -> Class3`. We can file a separate issue to follow up on this, just wanted to highlight it.

This is `ClassWithNewAndInit` in the mdtests. I've updated it to be fully in line with `Class3` from the conformance suite. We now infer `(...) -> Class3` as the callable that we map `**P` to, and the assignability check passes. Ideally this would map `**P` to a union of `(...) -> Class3` and `(x: int) -> Class3`, but there is an existing TODO from the paramspec implementation where we don't currently unify multiple solutions of a single paramspec.

> Similarly in this test we infer `() -> Never` instead of the expected `(*args: Any, **kwargs: Any) -> Never`:

`Class5` is `ClassWithNoReturnMetatype` in the mdtests. We're now inferring `(...) -> Unknown` — so we're getting the arguments correct now, but the return type is not being inferred. This can be follow-on work; I've added a TODO explaining why.

---

_@dcreager reviewed on 2025-12-15 17:18_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:823 on 2025-12-15 17:18_

See below; we're now getting the return wrong wrong, but the signature correct.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-15 17:21_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-15 17:21_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-15 17:31_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-15 17:31_

---

_@dcreager reviewed on 2025-12-15 20:03_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1350 on 2025-12-15 20:03_

#21983 is merged; rebased this onto it

---

_Comment by @carljm on 2025-12-15 23:37_

Conformance suite is looking pretty good!

On `constructors_callable.py` the only unexpected (that is, not already TODOed) issue I see is that we are now erroring on this call itself: https://github.com/python/typing/blob/77bbb7e7366568501a8b78b36317181fc939d11d/conformance/tests/constructors_callable.py#L141 It seems like a mismatch between callable inference and callable assignability: we infer the type `() -> Any | Class6Any` from the call (which seems wrong, not sure where the `| Class6Any` part comes from), and then we don't think the class `Class6Any` is assignable to that callable type.

In `dataclasses_transform_converter.py` we are newly wrongly erroring on these two lines: https://github.com/python/typing/blob/77bbb7e7366568501a8b78b36317181fc939d11d/conformance/tests/dataclasses_transform_converter.py#L102-L103  Seems like an issue where with an overloaded callable, we end up inferring the union of the argument types from the overloads, and then (correctly) fail to actually assign to that type from the overloaded callable.

(I guess this general category of issue, where we infer a specialization and then fail to assign to that same specialization, is something that will go away when we are fully using constraint sets and get rid of the old specialization builder?)

I think both of these issues are things we can tackle as follow-ups.

---

_Comment by @carljm on 2025-12-16 00:08_

I explored the Expression ecosystem hits a bit. The issue seems to have something to do with their [`Option` type](https://github.com/dbrattli/Expression/blob/main/expression/core/option.py#L43). I tried to simplify a repro without that type, and [it works fine](https://github.com/dbrattli/Expression/blob/main/expression/core/option.py#L43).

---

_Comment by @carljm on 2025-12-16 00:36_

Hmm, no, I was doing something wrong before. This problem repros with a pretty simple case. Minimal repro:

```py
from typing import TypeVar, Generic, Callable, Mapping

Key = TypeVar("Key", bound=int)
Value = TypeVar("Value")

class MapTree(Generic[Key, Value]):
    pass

def exists(f: Callable[[Key, Value], bool], m: MapTree[Key, Value]) -> bool:
    return True

_Key = TypeVar("_Key", bound=int)
_Value = TypeVar("_Value")

def _(p: Callable[[_Key, _Value], bool], m: MapTree[_Key, _Value]):
    exists(p, m)
```

---

_Comment by @carljm on 2025-12-16 00:39_

Even more minimal repro of the Expression problem; it doesn't require that we are matching up multiple arguments:

```py
from typing import TypeVar, Generic, Callable

Key = TypeVar("Key", bound=int)
Value = TypeVar("Value")

def exists(f: Callable[[Key, Value], bool]) -> bool:
    return True

_Key = TypeVar("_Key", bound=int)
_Value = TypeVar("_Value")

def _(p: Callable[[_Key, _Value], bool]):
    exists(p)  # false positive here
```

If we remove the upper bounds on `Key` and `_Key` typevars, it's fine. But with the upper bounds, at some point we wrongly simplify a typevar to its upper bound in constraint solving, and then that causes an assignability error.

---

_Comment by @dcreager on 2025-12-16 00:43_

> If we remove the upper bounds on `Key` and `_Key` typevars, it's fine. But with the upper bounds, at some point we wrongly simplify a typevar to its upper bound in constraint solving, and then that causes an assignability error.

A-ha! I ran into this on the generic protocols PR, too. This commit 9db0e3975c3796b53ebf1f52ec8d472b558228a8 fixes that issue by tracking that the bounds/constraints of a typevar should be used to limit which specializations are allowed, but shouldn't be surfaced as a specialization directly.

---

_Comment by @carljm on 2025-12-16 00:48_

Awesome! Do you think it makes sense to backport that commit here? Or land it as a separate PR with tests specific to constraint sets?

---

_Comment by @carljm on 2025-12-16 00:53_

The new diagnostics on homeassistant look like true positives. [`EventType` is invariant](https://github.com/home-assistant/core/blob/dev/homeassistant/util/event_type.pyi#L11-L15), so `EventType[Mapping[str, Never]]` should not be assignable to `EventType[Mapping[str, object]]`. And the `Never` here is not an inference bug, it's [explicit](https://github.com/home-assistant/core/blob/dev/homeassistant/helpers/typing.py#L15).

---

_Comment by @carljm on 2025-12-16 00:57_

Actually I might have spoken too soon on the homeassistant one; the `Never` is explicit, but the `object` is inferred. It's a typevar with a gradual upper bound of `Mapping[str, Any]`. Looking closer...

---

_Comment by @dcreager on 2025-12-16 00:59_

> Awesome! Do you think it makes sense to backport that commit here? Or land it as a separate PR with tests specific to constraint sets?

That one is chunky enough that I think it does deserve a separate PR.

---

_Comment by @carljm on 2025-12-16 01:13_

Yeah, I was wrong, homeassistant does look like a bug. Other type checkers are fine with it. Minimal repro:

```py
from typing import Generic, TypeVar, Mapping, Any, Callable, Never

T = TypeVar("T", bound=Mapping[str, Any], default=Mapping[str, Any])

class Event(Generic[T]):
    pass

class EventType(Generic[T]):
    pass

STOP: EventType[Mapping[str, Never]] = EventType()

def listen(et: EventType[T], cb: Callable[[Event[T]], None]):
    pass

def mycb(event: Event) -> None:
    pass

def _():
    listen(STOP, mycb)
```

We have a false positive on the last line, we solve `T` to `Mapping[str, object]` instead of `Mapping[str, Any]`, and then fail to assign `STOP` (which is `EventType[Mapping[str, Never]]` to `EventType[T]`.

Do we still take top materializations of gradual upper bounds? That seems like it might be the problem here.

---

_Comment by @carljm on 2025-12-16 01:20_

> That one is chunky enough that I think it does deserve a separate PR.

Are you thinking to get that PR landed first and then rebase this and revisit its ecosystem report? Or would you prefer to just land this with its remaining issues and tackle them all as follow-ups?

---

_Comment by @dcreager on 2025-12-16 01:26_

> Are you thinking to get that PR landed first and then rebase this and revisit its ecosystem report? Or would you prefer to just land this with its remaining issues and tackle them all as follow-ups?

Given the time, another option would be to combine this PR and the generic protocol one and try to land it all together. I'm down to one last mdtest file with test regressions to fix.

---

_Comment by @carljm on 2025-12-16 01:38_

We can try that. Do you have a lot of other fixes in the protocol PR that might help this PR, too? If so, combining might be most expedient.

The downside might be if the combined PR is harder to review ecosystem impact and land, because its so big?

If you want, I could also pull out that commit into its own PR and add tests for it, while you keep working on the protocol PR?

---

_Comment by @dcreager on 2025-12-16 01:57_

> If you want, I could also pull out that commit into its own PR and add tests for it, while you keep working on the protocol PR?

Sure, that works! I've got it started on the `dcreager/explicit-constraints` branch

---

_Comment by @dcreager on 2025-12-16 03:21_

> We have a false positive on the last line, we solve `T` to `Mapping[str, object]` instead of `Mapping[str, Any]`, and then fail to assign `STOP` (which is `EventType[Mapping[str, Never]]` to `EventType[T]`.
> 
> Do we still take top materializations of gradual upper bounds? That seems like it might be the problem here.

For this homeassistant error, I think it's the same as this TODO: https://github.com/astral-sh/ruff/pull/21551/changes#diff-d08dc0c815ce12a46b39702c611395d769e53bc49746c3a0c8533ed8af014863R814-R815

(That same comment appears as a TODO in some other mdtests, too.)

The issue is that we're creating a constraint set of the form `Never ≤ T`, where we mean that we actually want to map `T = Never`, but we're confusing that constraint set with "`T` has no lower bound". The new explicit markings should help take care of this, but I think we can do that as a follow-on.

---

_Comment by @carljm on 2025-12-16 03:33_

Hmm, not sure about that, because the issue repros even without `Never`. If I change the `Never` to `int`, we still have the assignability error where we infer `Mapping[str, object]` and try to assign `Mapping[str, int]`.

But removing the default and changing the upper bound `Mapping[str, Any]` to `Mapping[str, object]` doesn't solve the problem, either, so it's not related to typevar defaults or to gradual upper bounds. Here's the even-more-simplified example (removing `Never` and `Any` and typevar defaults):

```py
from typing import Generic, TypeVar, Mapping, Any, Callable, Never

T = TypeVar("T", bound=Mapping[str, object])

class Event(Generic[T]):
    pass

class EventType(Generic[T]):
    pass

STOP: EventType[Mapping[str, int]] = EventType()

def listen(et: EventType[T], cb: Callable[[Event[T]], None]):
    pass

def mycb(event: Event) -> None:
    pass

def _():
    listen(STOP, mycb)
```

Other typecheckers are still happy with this; we emit an error on the bottom line yet.

Maybe this is a variant of the same problem as the Expression one, and will also be fixed by the explicit-constraints change?

If not I agree we can look into it as follow-on.

---

_Comment by @dcreager on 2025-12-16 03:39_

> Maybe this is a variant of the same problem as the Expression one, and will also be fixed by the explicit-constraints change?

Ah that's a good catch. If we're inferring the bounds, then yes I think the explicit-constraints change will help.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-16 13:09_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-16 13:09_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-16 16:21_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-16 16:21_

---

_Comment by @dcreager on 2025-12-16 16:39_

Any idea where a 101 exit code comes from? Does that happen when we panic? When I run ty locally on spack (both dev and profiling profiles), I get a successful type check.

---

_Comment by @carljm on 2025-12-16 16:45_

Just in terms of numbers, ecosystem diff is definitely improved here! We were adding almost 1200 new diagnostics before, now its under 800.

---

_Comment by @carljm on 2025-12-16 16:45_

Where do you see the 101 return code? Primer and ecosystem analyzer jobs both completed successfully...

EDIT: oh never mind, I see it once I link to the full ecosystem analyzer output... but spack checked fine on the mypy-primer job. Running ecosystem analyzer again to see if it persists.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-16 16:46_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-16 16:46_

---

_Comment by @carljm on 2025-12-16 17:00_

Spack error is gone on the second run; I suspect an infra issue. (I guess I didn't check the logs for the ecosystem analyzer run to see if there was anything useful there.)

Spot-checked the first Expression failure -- looks like a combo of generic protocol inference, plus us using narrower inferred types. Not an issue for this PR. I'll look at a couple more quick?

---

_Comment by @dcreager on 2025-12-16 17:00_

> Spot-checked the first Expression failure -- looks like a combo of generic protocol inference, plus us using narrower inferred types. Not an issue for this PR. I'll look at a couple more quick?

Sounds good, thanks!

---

_Comment by @carljm on 2025-12-16 17:01_

Second one also looks like generic protocols.

---

_Comment by @carljm on 2025-12-16 17:13_

In bokeh I see an example that boils down to this:

```py
from typing import Callable, TypeVar

T = TypeVar("T")

def infer(func: Callable[[T | str], T]) -> T:
    return func("foo")

def convert_str_seq(value: list[str] | str) -> list[str]:
    if isinstance(value, str):
        return [value]
    return value

reveal_type(infer(convert_str_seq))  # expected `list[str]` but we say `list[str] | str`
```

Basically we don't unpack the union and match it item-by-item, so the `T | str` in the formal parameter doesn't eliminate `| str` from the union in the actual argument.

But pyrefly does the same as we do here -- I think this one is fine to look at as follow-up.

---

_@carljm approved on 2025-12-16 17:13_

Ship it!

---

_Merged by @carljm on 2025-12-16 17:16_

---

_Closed by @carljm on 2025-12-16 17:16_

---

_Branch deleted on 2025-12-16 17:16_

---
