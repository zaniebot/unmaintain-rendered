```yaml
number: 22010
title: "[ty] Improve rendering of default values for function args"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
assignees: []
merged: true
base: main
head: gankra/default-val
created_at: 2025-12-16T17:31:54Z
updated_at: 2025-12-16T18:39:21Z
url: https://github.com/astral-sh/ruff/pull/22010
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Improve rendering of default values for function args

---

_@Gankra_

## Summary

We're actually quite good at computing this but the main issue is just that we compute it at the type-level and so wrap it in `Literal[...]`. So just special-case the rendering of these to omit `Literal[...]` and fallback to `...` in cases where the thing we'll show is probably useless (i.e. `x: str = str`).

Fixes https://github.com/astral-sh/ty/issues/1882


---

_Label `ty` added by @Gankra on 2025-12-16 17:31_

---

_Comment by @Gankra on 2025-12-16 17:32_

Having now implemented the fallback I think I should probably just land only the fallback and rip out the explicit support for default_value. This would mostly only harm stuff like `=sys.maxsize`.

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 17:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-16 18:07:49.056442460 +0000
+++ new-output.txt	2025-12-16 18:07:52.556461989 +0000
@@ -137,7 +137,7 @@
 callables_kwargs.py:64:11: error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `str`, found `Literal[1]`
 callables_kwargs.py:64:14: error[parameter-already-assigned] Multiple values provided for parameter `v3` of function `func2`
 callables_kwargs.py:103:19: error[invalid-assignment] Object of type `def func1(**kwargs: @Todo(`Unpack[]` special form)) -> None` is not assignable to `TDProtocol5`
-callables_kwargs.py:134:19: error[invalid-assignment] Object of type `def func7(*, v1: int, v3: str, v2: str = Literal[""]) -> None` is not assignable to `TDProtocol6`
+callables_kwargs.py:134:19: error[invalid-assignment] Object of type `def func7(*, v1: int, v3: str, v2: str = "") -> None` is not assignable to `TDProtocol6`
 callables_protocol.py:35:7: error[invalid-assignment] Object of type `def cb1_bad1(*vals: bytes, *, max_items: int | None) -> list[bytes]` is not assignable to `Proto1`
 callables_protocol.py:36:7: error[invalid-assignment] Object of type `def cb1_bad2(*vals: bytes) -> list[bytes]` is not assignable to `Proto1`
 callables_protocol.py:37:7: error[invalid-assignment] Object of type `def cb1_bad3(*vals: bytes, *, max_len: str | None) -> list[bytes]` is not assignable to `Proto1`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-16 17:35_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
+ tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
- tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
+ tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
- typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
+ typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/vendor/typing_extensions.py:1119:5: error[unresolved-attribute] Unresolved attribute `__text_signature__` on type `def _typeddict_new(*args, *, total=Literal[True], **kwargs) -> Unknown`.
+ lib/spack/spack/vendor/typing_extensions.py:1119:5: error[unresolved-attribute] Unresolved attribute `__text_signature__` on type `def _typeddict_new(*args, *, total=True, **kwargs) -> Unknown`.

pip (https://github.com/pypa/pip)
- src/pip/_vendor/idna/codec.py:114:9: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `_Decoder`, found `bound method Codec.decode(data: bytes, errors: str = Literal["strict"]) -> tuple[str, int]`
+ src/pip/_vendor/idna/codec.py:114:9: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `_Decoder`, found `bound method Codec.decode(data: bytes, errors: str = "strict") -> tuple[str, int]`

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/tests/test_master.py:173:5: error[unresolved-attribute] Object of type `bound method Master.rpc(method_name: str, serial: int = Literal[0]) -> CoroutineType[Any, Any, Any]` has no attribute `assert_called_once_with`
+ src/bandersnatch/tests/test_master.py:173:5: error[unresolved-attribute] Object of type `bound method Master.rpc(method_name: str, serial: int = 0) -> CoroutineType[Any, Any, Any]` has no attribute `assert_called_once_with`
- src/bandersnatch/tests/test_master.py:239:5: error[unresolved-attribute] Object of type `bound method Master.rpc(method_name: str, serial: int = Literal[0]) -> CoroutineType[Any, Any, Any]` has no attribute `assert_called_once_with`
+ src/bandersnatch/tests/test_master.py:239:5: error[unresolved-attribute] Object of type `bound method Master.rpc(method_name: str, serial: int = 0) -> CoroutineType[Any, Any, Any]` has no attribute `assert_called_once_with`
- src/bandersnatch/tests/test_package.py:31:12: error[unresolved-attribute] Object of type `bound method Master.get_package_metadata(package_name: str, serial: int = Literal[0]) -> CoroutineType[Any, Any, Any]` has no attribute `await_count`
+ src/bandersnatch/tests/test_package.py:31:12: error[unresolved-attribute] Object of type `bound method Master.get_package_metadata(package_name: str, serial: int = 0) -> CoroutineType[Any, Any, Any]` has no attribute `await_count`
- src/bandersnatch/tests/test_package.py:58:12: error[unresolved-attribute] Object of type `bound method Master.get_package_metadata(package_name: str, serial: int = Literal[0]) -> CoroutineType[Any, Any, Any]` has no attribute `await_count`
+ src/bandersnatch/tests/test_package.py:58:12: error[unresolved-attribute] Object of type `bound method Master.get_package_metadata(package_name: str, serial: int = 0) -> CoroutineType[Any, Any, Any]` has no attribute `await_count`

asynq (https://github.com/quora/asynq)
- asynq/mock_.py:96:1: error[unresolved-attribute] Unresolved attribute `object` on type `def patch(target, new=Any, spec=None, create=Literal[False], mocksignature=Literal[False], spec_set=None, autospec=Literal[False], new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:96:1: error[unresolved-attribute] Unresolved attribute `object` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
- asynq/mock_.py:98:1: error[unresolved-attribute] Unresolved attribute `dict` on type `def patch(target, new=Any, spec=None, create=Literal[False], mocksignature=Literal[False], spec_set=None, autospec=Literal[False], new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:98:1: error[unresolved-attribute] Unresolved attribute `dict` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
- asynq/mock_.py:99:1: error[unresolved-attribute] Unresolved attribute `TEST_PREFIX` on type `def patch(target, new=Any, spec=None, create=Literal[False], mocksignature=Literal[False], spec_set=None, autospec=Literal[False], new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:99:1: error[unresolved-attribute] Unresolved attribute `TEST_PREFIX` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
- asynq/mock_.py:100:1: error[unresolved-attribute] Unresolved attribute `stopall` on type `def patch(target, new=Any, spec=None, create=Literal[False], mocksignature=Literal[False], spec_set=None, autospec=Literal[False], new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:100:1: error[unresolved-attribute] Unresolved attribute `stopall` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/path.py:759:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
+ src/_pytest/_py/path.py:759:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
- src/_pytest/_py/path.py:760:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
+ src/_pytest/_py/path.py:760:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
- src/_pytest/_py/path.py:763:41: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
+ src/_pytest/_py/path.py:763:41: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
- src/_pytest/_py/path.py:763:55: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
+ src/_pytest/_py/path.py:763:55: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`

rich (https://github.com/Textualize/rich)
- examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def beat(length: int = Literal[1]) -> None`
+ examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def beat(length: int = 1) -> None`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/test_utils.py:147:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def tmp_digits_dataset(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = Literal[False], seed_train: int = Literal[13], seed_dev: int = Literal[13], source_text_prefix_token: str = Literal[""], with_n_source_factors: int = Literal[0], with_n_target_factors: int = Literal[0]) -> dict[str, Any]`
+ sockeye/test_utils.py:147:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def tmp_digits_dataset(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = False, seed_train: int = 13, seed_dev: int = 13, source_text_prefix_token: str = "", with_n_source_factors: int = 0, with_n_target_factors: int = 0) -> dict[str, Any]`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job_utest.py:269:13: warning[possibly-missing-attribute] Attribute `assert_called_once` may be missing on object of type `Unknown | (bound method SmartContractModel.set_state(state: str | ContractState, msg: str = Literal[""]) -> None) | MagicMock`
+ dragonchain/job_processor/contract_job_utest.py:269:13: warning[possibly-missing-attribute] Attribute `assert_called_once` may be missing on object of type `Unknown | (bound method SmartContractModel.set_state(state: str | ContractState, msg: str = "") -> None) | MagicMock`
- dragonchain/lib/database/redis_utest.py:102:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:102:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:106:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:106:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:122:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].flushall(asynchronous: bool = Literal[False], **kwargs: Any) -> bool` has no attribute `assert_called_once`
+ dragonchain/lib/database/redis_utest.py:122:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].flushall(asynchronous: bool = False, **kwargs: Any) -> bool` has no attribute `assert_called_once`
- dragonchain/lib/database/redis_utest.py:146:9: error[unresolved-attribute] Object of type `Overload[(keys: Iterable[bytes | int | float | str] | int | float, timeout: Literal[0] | None = Literal[0]) -> tuple[Unknown, Unknown], (keys: Iterable[bytes | int | float | str] | int | float, timeout: int | float) -> tuple[Unknown, Unknown] | None]` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:146:9: error[unresolved-attribute] Object of type `Overload[(keys: Iterable[bytes | int | float | str] | int | float, timeout: Literal[0] | None = 0) -> tuple[Unknown, Unknown], (keys: Iterable[bytes | int | float | str] | int | float, timeout: int | float) -> tuple[Unknown, Unknown] | None]` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:150:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].brpoplpush(src, dst, timeout: int | None = Literal[0]) -> Unknown` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:150:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].brpoplpush(src, dst, timeout: int | None = 0) -> Unknown` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:162:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:162:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = False, xx: bool = False, keepttl: bool = False, get: bool = False, exat: Any | None = None, pxat: Any | None = None) -> bool | None` has no attribute `assert_called_once_with`
- dragonchain/lib/database/redis_utest.py:218:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].zadd(name: str | bytes, mapping: Mapping[str | bytes, bytes | int | float | str], nx: bool = Literal[False], xx: bool = Literal[False], ch: bool = Literal[False], incr: bool = Literal[False], gt: Any | None = Literal[False], lt: Any | None = Literal[False]) -> int` has no attribute `assert_called_once_with`
+ dragonchain/lib/database/redis_utest.py:218:9: error[unresolved-attribute] Object of type `bound method Redis[Unknown].zadd(name: str | bytes, mapping: Mapping[str | bytes, bytes | int | float | str], nx: bool = False, xx: bool = False, ch: bool = False, incr: bool = False, gt: Any | None = False, lt: Any | None = False) -> int` has no attribute `assert_called_once_with`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/schemas.py:687:25: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = NotSet, media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
+ src/schemathesis/schemas.py:687:25: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
- src/schemathesis/schemas.py:732:82: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = NotSet, media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
+ src/schemathesis/schemas.py:732:82: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
- src/schemathesis/schemas.py:763:15: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = NotSet, media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
+ src/schemathesis/schemas.py:763:15: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
- src/schemathesis/schemas.py:807:10: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = NotSet, media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression
+ src/schemathesis/schemas.py:807:10: error[invalid-type-form] Variable of type `def Case(self, *, method: str | None = None, path_parameters: dict[str, Any] | None = None, headers: dict[str, Any] | CaseInsensitiveDict[Unknown] | None = None, cookies: dict[str, Any] | None = None, query: dict[str, Any] | None = None, body: list[Unknown] | dict[str, Any] | str | ... omitted 4 union elements = ..., media_type: str | None = None, multipart_content_types: dict[str, str] | None = None, _meta: CaseMetadata | None = None) -> Unknown` is not allowed in a type expression

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = <special-form 'typing.Any'>) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = ...) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
- test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = <special-form 'typing.Any'>) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = ...) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression

urllib3 (https://github.com/urllib3/urllib3)
- test/test_http2_connection.py:133:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:133:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:150:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:150:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:173:9: warning[possibly-missing-attribute] Attribute `assert_has_calls` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:173:9: warning[possibly-missing-attribute] Attribute `assert_has_calls` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:205:17: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:205:17: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:229:13: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:229:13: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:259:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:259:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:292:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:292:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:325:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:325:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`
- test/test_http2_connection.py:347:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = Literal[0], /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)`
+ test/test_http2_connection.py:347:9: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `(bound method socket.sendall(data: Buffer, flags: int = 0, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = 0) -> None)`

build (https://github.com/pypa/build)
- tests/test_projectbuilder.py:439:5: error[invalid-assignment] Object of type `Literal["dist-1.0.dist-info"]` is not assignable to attribute `return_value` on type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = Literal[True]) -> str)`
+ tests/test_projectbuilder.py:439:5: error[invalid-assignment] Object of type `Literal["dist-1.0.dist-info"]` is not assignable to attribute `return_value` on type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = True) -> str)`
- tests/test_projectbuilder.py:442:5: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = Literal[True]) -> str)`
+ tests/test_projectbuilder.py:442:5: warning[possibly-missing-attribute] Attribute `assert_called_with` may be missing on object of type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = True) -> str)`
- tests/test_projectbuilder.py:450:5: error[invalid-assignment] Object of type `HookMissing` is not assignable to attribute `side_effect` on type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = Literal[True]) -> str)`
+ tests/test_projectbuilder.py:450:5: error[invalid-assignment] Object of type `HookMissing` is not assignable to attribute `side_effect` on type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = True) -> str)`
- tests/test_projectbuilder.py:459:5: error[invalid-assignment] Object of type `<class 'Exception'>` is not assignable to attribute `side_effect` on type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = Literal[True]) -> str)`
+ tests/test_projectbuilder.py:459:5: error[invalid-assignment] Object of type `<class 'Exception'>` is not assignable to attribute `side_effect` on type `Unknown | (bound method BuildBackendHookCaller.prepare_metadata_for_build_wheel(metadata_directory: str, config_settings: Mapping[str, Any] | None = None, _allow_fallback: bool = True) -> str)`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `

... (truncated 381 lines) ...
```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood reviewed on 2025-12-16 17:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:919 on 2025-12-16 17:43_

do we even need to keep the `default_type` field if we now have a `default_value` field? I think we literally only use the `default_type` for rendering in display; I think it's completely irrelevant for type-checking purposes. (For type-checking purposes, all you need to know is whether a parameter _has_ a default; you don't need to know what the default _is_.)

---

_Renamed from "[ty] Render default values for function args" to "[ty] Improve rendering of default values for function args" by @Gankra on 2025-12-16 17:58_

---

_Marked ready for review by @Gankra on 2025-12-16 17:59_

---

_Review requested from @carljm by @Gankra on 2025-12-16 17:59_

---

_Review requested from @sharkdp by @Gankra on 2025-12-16 17:59_

---

_Review requested from @dcreager by @Gankra on 2025-12-16 17:59_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-16 17:59_

---

_Comment by @Gankra on 2025-12-16 17:59_

Ok yeah with the only the type-based fallback this PR is just a slamdunk strict improvement.

---

_Comment by @Gankra on 2025-12-16 18:00_

See https://github.com/astral-sh/ruff/pull/22010/commits/2e2a668b30ae000200296f5bd48b2af40fdef6a7 for what we give up by only using types and not exprs (it's not much).

---

_Comment by @AlexWaygood on 2025-12-16 18:04_

> See [2e2a668](https://github.com/astral-sh/ruff/commit/2e2a668b30ae000200296f5bd48b2af40fdef6a7) for what we give up by only using types and not exprs (it's not much).

yes, but do we need to be storing the types of these default values at all? Since we literally _only_ use the types of these default values for display rendering, we could probably rip that all out and _only_ store the default value instead. This PR doesn't, for instance, give us parity with Pylance, which supports rendering list/set/dict/tuple literals in parameter default values in its type display: 
<img width="972" height="224" alt="image" src="https://github.com/user-attachments/assets/b63382b6-0323-4e62-b270-fb8219d2957a" />

I'm okay with this as an incremental step since it's definitely a big improvement on the status quo but I think some bigger refactors and rethinks are in order here

---

_Comment by @Gankra on 2025-12-16 18:11_

> yes, but do we need to be storing the types of these default values at all? 

Yes we're actually really good at getting the default values of dataclasses but it only goes through the type machinery and comes out as `Literal[1]` or whatever. I added the type fallback path to cover that case and then realized, oh, actually, this covers most of the cases I'm catching with the expr anyway.

---

_@AlexWaygood approved on 2025-12-16 18:36_

I still think there's much more room for improvement here, but let's get this into the beta. It's a _big_ improvement on the status quo. Thank you!

---

_Merged by @Gankra on 2025-12-16 18:39_

---

_Closed by @Gankra on 2025-12-16 18:39_

---

_Branch deleted on 2025-12-16 18:39_

---
