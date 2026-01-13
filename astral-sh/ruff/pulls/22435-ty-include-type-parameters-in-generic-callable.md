```yaml
number: 22435
title: "[ty] Include type parameters in generic callable display"
type: pull_request
state: merged
author: bxff
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: main
created_at: 2026-01-07T12:02:47Z
updated_at: 2026-01-13T17:29:08Z
url: https://github.com/astral-sh/ruff/pull/22435
synced_at: 2026-01-13T18:48:34Z
```

# [ty] Include type parameters in generic callable display

---

_@bxff_

Fixes astral-sh/ty#1207

## Summary

Modified `DisplaySignature` to show type parameter brackets (e.g., `[T]`) when displaying generic function and method signatures. Added a `generic_context` field and updated `fmt_detailed()` to prepend type parameters, preventing duplication when `DisplayFunctionType` already renders them.

## Test Plan

Updated expected output in 6 test files. All tests pass: `test result: ok. 307 passed; 0 failed; 0 ignored`

## Example

```
Before: list[Unknown | ((t: T@f) -> T@f)]
After:  list[Unknown | ([T](t: T@f) -> T@f)]
```

---

_Review requested from @carljm by @bxff on 2026-01-07 12:02_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-07 12:02_

---

_Review requested from @sharkdp by @bxff on 2026-01-07 12:02_

---

_Review requested from @dcreager by @bxff on 2026-01-07 12:02_

---

_Label `ty` added by @AlexWaygood on 2026-01-07 12:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1170 on 2026-01-07 12:18_

Because `Wrap` has been specialized with `int` here, the `__init__` method is arguably no longer generic in this case, so I find the `[T]` prefix confusing here.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1183 on 2026-01-07 12:18_

Same here: `WrappedIntAndExtraData` has already been specialized with `bytes`, so the `__init__` signature is no longer generic

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 12:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-13 17:26:33.731951055 +0000
+++ new-output.txt	2026-01-13 17:26:34.088951937 +0000
@@ -468,7 +468,7 @@
 generics_defaults.py:131:1: error[type-assertion-failure] Type `Any` does not match asserted type `int`
 generics_defaults.py:155:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
 generics_defaults.py:156:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
-generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth(self, /) -> Self@meth`
+generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth[Self](self, /) -> Self`
 generics_defaults_referential.py:23:1: error[type-assertion-failure] Type `type[slice[int, int, int | None]]` does not match asserted type `<class 'slice'>`
 generics_defaults_referential.py:36:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`

```

</details>




---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1211 on 2026-01-07 12:20_

For clarity, I think this should either be

```suggestion
# revealed: [T](self: ParentDataclass[T], value: T) -> None
```

or

```suggestion
# revealed: [T@ChildOfParentDataclass](self: ParentDataclass[T@ChildOfParentDataclass], value: T@ChildOfParentDataclass) -> None
```

(I prefer the first version, because it's more concise, and I think once you add the generic parameter to the beginning of the display the `@<function_name>` suffix for the type parameter in the rest of the display becomes unnecessary -- the scoping of the type parameter is now fully explicit through other means)

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 12:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, (base: _SupportsPow2[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3NoneOnly[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3[_E_contra@pow, _M_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: _M_contra@pow) -> _T_co@pow, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
+ more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, [_E_contra, _T_co](base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _T_co](base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _M_contra, _T_co](base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/fsm_cmd.py:44:5: error[invalid-assignment] Object of type `def symlink_aware_copyfile(...) -> Unknown` is not assignable to attribute `copyfile` of type `def copyfile[_StrOrBytesPathT](src: str | bytes | PathLike[str] | PathLike[bytes], dst: _StrOrBytesPathT@copyfile, *, follow_symlinks: bool = True) -> _StrOrBytesPathT@copyfile`
+ paasta_tools/cli/fsm_cmd.py:44:5: error[invalid-assignment] Object of type `def symlink_aware_copyfile(...) -> Unknown` is not assignable to attribute `copyfile` of type `def copyfile[_StrOrBytesPathT](src: str | bytes | PathLike[str] | PathLike[bytes], dst: _StrOrBytesPathT, *, follow_symlinks: bool = True) -> _StrOrBytesPathT`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/decorators.py:40:16: error[invalid-assignment] Object of type `(func: (**_P@deprecated) -> _T@deprecated) -> _T@deprecated` is not assignable to `def deco[**_P](func: (**_P@deprecated) -> _T@deprecated) -> (**_P@deprecated) -> _T@deprecated`
+ scrapy/utils/decorators.py:40:16: error[invalid-assignment] Object of type `(func: (**_P@deprecated) -> _T@deprecated) -> _T@deprecated` is not assignable to `def deco[**_P](func: (**_P@deprecated) -> _T) -> (**_P@deprecated) -> _T`

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/multiprocessing/managers.pyi:328:39: error[invalid-type-form] Invalid subscript of object of type `Overload[(self, iterable: Iterable[_T@list], /) -> ListProxy[_T@list], (self) -> ListProxy[Any]]` in type expression
+ mypy/typeshed/stdlib/multiprocessing/managers.pyi:328:39: error[invalid-type-form] Invalid subscript of object of type `Overload[[_T](self, iterable: Iterable[_T], /) -> ListProxy[_T], (self) -> ListProxy[Any]]` in type expression
- mypy/typeshed/stdlib/multiprocessing/managers.pyi:330:39: error[invalid-type-form] Invalid subscript of object of type `Overload[(self, iterable: Iterable[_T@list], /) -> ListProxy[_T@list], (self) -> ListProxy[Any]]` in type expression
+ mypy/typeshed/stdlib/multiprocessing/managers.pyi:330:39: error[invalid-type-form] Invalid subscript of object of type `Overload[[_T](self, iterable: Iterable[_T], /) -> ListProxy[_T], (self) -> ListProxy[Any]]` in type expression

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = ...) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T] = ...) -> T | _Placeholder[T]` in type expression
- test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = ...) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T] = ...) -> T | _Placeholder[T]` in type expression

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_typeinfo.py:300:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: str | int) -> TypeInfo, (key: tuple[type[T@__getitem__], int]) -> T@__getitem__]` cannot be called with key of type `str | int | tuple[type, int]` on object of type `Self@get`
+ psycopg/psycopg/_typeinfo.py:300:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: str | int) -> TypeInfo, [T](key: tuple[type[T], int]) -> T]` cannot be called with key of type `str | int | tuple[type, int]` on object of type `Self@get`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

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

trio (https://github.com/python-trio/trio)
- src/trio/_core/_run.py:142:9: error[invalid-assignment] Object of type `def repr_callable[BaseExcT](fun: (BaseExcT@repr_callable, /) -> bool) -> str` is not assignable to attribute `repr_callable` of type `def repr_callable[BaseExcT_1](fun: (BaseExcT_1@repr_callable, /) -> bool) -> str`
+ src/trio/_core/_run.py:142:9: error[invalid-assignment] Object of type `def repr_callable[BaseExcT](fun: (BaseExcT, /) -> bool) -> str` is not assignable to attribute `repr_callable` of type `def repr_callable[BaseExcT_1](fun: (BaseExcT_1, /) -> bool) -> str`

pyodide (https://github.com/pyodide/pyodide)
- src/py/pyodide/webloop.py:980:5: error[invalid-assignment] Object of type `_Wrapped[(main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None), _T@run, (main, *, debug=None, loop_factory=None), Unknown]` is not assignable to attribute `run` of type `def run[_T](main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None) -> _T@run`
+ src/py/pyodide/webloop.py:980:5: error[invalid-assignment] Object of type `_Wrapped[(main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None), _T@run, (main, *, debug=None, loop_factory=None), Unknown]` is not assignable to attribute `run` of type `def run[_T](main: Coroutine[Any, Any, _T], *, debug: bool | None = None) -> _T`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/entryexitanalysis.py:58:57: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT@__getitem__), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements, (idx: ((DataFrame, /) -> ScalarT@__getitem__) | tuple[slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements, ScalarT@__getitem__ | None] | None) -> Series[Any], (idx: str | bytes | date | ... omitted 9 union elements) -> Series[Any] | DataFrame, (idx: tuple[str | bytes | date | ... omitted 9 union elements, slice[Any, Any, Any]]) -> Series[Any] | DataFrame, (key: slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements) -> DataFrame]` cannot be called with key of type `tuple[Hashable, Literal["enter_tag"]]` on object of type `_LocIndexerFrame[DataFrame]`
+ freqtrade/data/entryexitanalysis.py:58:57: error[invalid-argument-type] Method `__getitem__` of type `Overload[[ScalarT](idx: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements, [ScalarT, HashableT](idx: ((DataFrame, /) -> ScalarT) | tuple[slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements, ScalarT | None] | None) -> Series[Any], (idx: str | bytes | date | ... omitted 9 union elements) -> Series[Any] | DataFrame, (idx: tuple[str | bytes | date | ... omitted 9 union elements, slice[Any, Any, Any]]) -> Series[Any] | DataFrame, [HashableT](key: slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements) -> DataFrame]` cannot be called with key of type `tuple[Hashable, Literal["enter_tag"]]` on object of type `_LocIndexerFrame[DataFrame]`
- freqtrade/plot/plotting.py:187:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT@__getitem__), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements, (idx: ((DataFrame, /) -> ScalarT@__getitem__) | tuple[slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements, ScalarT@__getitem__ | None] | None) -> Series[Any], (idx: str | bytes | date | ... omitted 9 union elements) -> Series[Any] | DataFrame, (idx: tuple[str | bytes | date | ... omitted 9 union elements, slice[Any, Any, Any]]) -> Series[Any] | DataFrame, (key: slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements) -> DataFrame]` cannot be called with key of type `tuple[datetime, Literal["cum_profit"]]` on object of type `_LocIndexerFrame[DataFrame]`
+ freqtrade/plot/plotting.py:187:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[[ScalarT](idx: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements, [ScalarT, HashableT](idx: ((DataFrame, /) -> ScalarT) | tuple[slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements, ScalarT | None] | None) -> Series[Any], (idx: str | bytes | date | ... omitted 9 union elements) -> Series[Any] | DataFrame, (idx: tuple[str | bytes | date | ... omitted 9 union elements, slice[Any, Any, Any]]) -> Series[Any] | DataFrame, [HashableT](key: slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements) -> DataFrame]` cannot be called with key of type `tuple[datetime, Literal["cum_profit"]]` on object of type `_LocIndexerFrame[DataFrame]`
- freqtrade/plot/plotting.py:188:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT@__getitem__), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements, (idx: ((DataFrame, /) -> ScalarT@__getitem__) | tuple[slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements, ScalarT@__getitem__ | None] | None) -> Series[Any], (idx: str | bytes | date | ... omitted 9 union elements) -> Series[Any] | DataFrame, (idx: tuple[str | bytes | date | ... omitted 9 union elements, slice[Any, Any, Any]]) -> Series[Any] | DataFrame, (key: slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements) -> DataFrame]` cannot be called with key of type `tuple[datetime, Literal["cum_profit"]]` on object of type `_LocIndexerFrame[DataFrame]`
+ freqtrade/plot/plotting.py:188:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[[ScalarT](idx: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements, [ScalarT, HashableT](idx: ((DataFrame, /) -> ScalarT) | tuple[slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements, ScalarT | None] | None) -> Series[Any], (idx: str | bytes | date | ... omitted 9 union elements) -> Series[Any] | DataFrame, (idx: tuple[str | bytes | date | ... omitted 9 union elements, slice[Any, Any, Any]]) -> Series[Any] | DataFrame, [HashableT](key: slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]] | Index[Any] | ... omitted 8 union elements) -> DataFrame]` cannot be called with key of type `tuple[datetime, Literal["cum_profit"]]` on object of type `_LocIndexerFrame[DataFrame]`
- freqtrade/strategy/strategy_helper.py:102:42: error[invalid-argument-type] Method `__getitem__` of type `bound method _AtIndexerFrame.__getitem__[ScalarT](key: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT@__getitem__), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements` cannot be called with key of type `tuple[(str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | (date & ~AlwaysFalsy) | ... omitted 9 union elements, str]` on object of type `_AtIndexerFrame`
+ freqtrade/strategy/strategy_helper.py:102:42: error[invalid-argument-type] Method `__getitem__` of type `bound method _AtIndexerFrame.__getitem__[ScalarT](key: tuple[int | str | Timestamp | tuple[str | bytes | date | ... omitted 9 union elements, ...] | ((DataFrame, /) -> ScalarT), int | str | tuple[str | bytes | date | ... omitted 9 union elements, ...]]) -> str | bytes | date | ... omitted 9 union elements` cannot be called with key of type `tuple[(str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | (date & ~AlwaysFalsy) | ... omitted 9 union elements, str]` on object of type `_AtIndexerFrame`

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/early_patches.py:33:1: error[invalid-assignment] Object of type `def wrapped_lru_cache(...) -> Unknown` is not assignable to attribute `lru_cache` of type `Overload[(maxsize: int | None = 128, typed: bool = False) -> ((...) -> _T@lru_cache, /) -> _lru_cache_wrapper[_T@lru_cache], (maxsize: (...) -> _T@lru_cache, typed: bool = False) -> _lru_cache_wrapper[_T@lru_cache]]`
+ tests/unittests/early_patches.py:33:1: error[invalid-assignment] Object of type `def wrapped_lru_cache(...) -> Unknown` is not assignable to attribute `lru_cache` of type `Overload[[_T](maxsize: int | None = 128, typed: bool = False) -> ((...) -> _T, /) -> _lru_cache_wrapper[_T], [_T](maxsize: (...) -> _T, typed: bool = False) -> _lru_cache_wrapper[_T]]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/core.py:2358:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `(Context[object], /) -> bool | Coroutine[Never, object, bool]`, found `def predicate[BotT](ctx: Context[BotT@predicate]) -> bool`
+ discord/ext/commands/core.py:2358:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `(Context[object], /) -> bool | Coroutine[Never, object, bool]`, found `def predicate[BotT](ctx: Context[BotT]) -> bool`
- discord/ext/commands/core.py:2433:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `(Context[object], /) -> bool | Coroutine[Never, object, bool]`, found `def predicate[BotT](ctx: Context[BotT@predicate]) -> bool`
+ discord/ext/commands/core.py:2433:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `(Context[object], /) -> bool | Coroutine[Never, object, bool]`, found `def predicate[BotT](ctx: Context[BotT]) -> bool`
- discord/ext/commands/help.py:309:9: error[invalid-assignment] Object of type `def wrapped_get_commands(*, _original: () -> list[Command[Any, (...), Any]] = ...) -> list[Command[Any, (...), Any]]` is not assignable to attribute `get_commands` of type `def get_commands(self) -> list[Command[Self@get_commands, (...), Any]]`
+ discord/ext/commands/help.py:309:9: error[invalid-assignment] Object of type `def wrapped_get_commands(*, _original: () -> list[Command[Any, (...), Any]] = ...) -> list[Command[Any, (...), Any]]` is not assignable to attribute `get_commands` of type `def get_commands[Self](self) -> list[Command[Self, (...), Any]]`
- discord/ext/commands/help.py:310:9: error[invalid-assignment] Object of type `def wrapped_walk_commands(*, _original: () -> Generator[Command[Any, (...), Any], None, None] = ...) -> Unknown` is not assignable to attribute `walk_commands` of type `def walk_commands(self) -> Generator[Command[Self@walk_commands, (...), Any], None, None]`
+ discord/ext/commands/help.py:310:9: error[invalid-assignment] Object of type `def wrapped_walk_commands(*, _original: () -> Generator[Command[Any, (...), Any], None, None] = ...) -> Unknown` is not assignable to attribute `walk_commands` of type `def walk_commands[Self](self) -> Generator[Command[Self, (...), Any], None, None]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/utils/universal.py:1674:35: error[invalid-assignment] Object of type `Overload[(key: _T@extract_as_list, default: None = None, /) -> _U@extract_as_list | None, (key: _T@extract_as_list, default: _U@extract_as_list, /) -> _U@extract_as_list, (key: _T@extract_as_list, default: _T@get, /) -> _U@extract_as_list | _T@get]` is not assignable to `(_T@extract_as_list, /) -> _U@extract_as_list`
+ mesonbuild/utils/universal.py:1674:35: error[invalid-assignment] Object of type `Overload[(key: _T@extract_as_list, default: None = None, /) -> _U@extract_as_list | None, (key: _T@extract_as_list, default: _U@extract_as_list, /) -> _U@extract_as_list, [_T](key: _T@extract_as_list, default: _T, /) -> _U@extract_as_list | _T]` is not assignable to `(_T@extract_as_list, /) -> _U@extract_as_list`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/extension.py:123:37: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes, /) -> str | bytes`, found `Overload[(path: str) -> str, (path: bytes) -> bytes, (path: PathLike[AnyStr@fspath]) -> AnyStr@fspath]`
+ setuptools/_distutils/extension.py:123:37: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes, /) -> str | bytes`, found `Overload[(path: str) -> str, (path: bytes) -> bytes, [AnyStr](path: PathLike[AnyStr]) -> AnyStr]`
- setuptools/_distutils/tests/test_util.py:103:9: error[invalid-assignment] Object of type `def _splitdrive(path) -> Unknown` is not assignable to attribute `splitdrive` of type `Overload[(p: PathLike[AnyStr@splitdrive]) -> tuple[AnyStr@splitdrive, AnyStr@splitdrive], (p: AnyOrLiteralStr@splitdrive) -> tuple[AnyOrLiteralStr@splitdrive, AnyOrLiteralStr@splitdrive]]`
+ setuptools/_distutils/tests/test_util.py:103:9: error[invalid-assignment] Object of type `def _splitdrive(path) -> Unknown` is not assignable to attribute `splitdrive` of type `Overload[[AnyStr](p: PathLike[AnyStr]) -> tuple[AnyStr, AnyStr], [AnyOrLiteralStr](p: AnyOrLiteralStr) -> tuple[AnyOrLiteralStr, AnyOrLiteralStr]]`
- setuptools/_vendor/more_itertools/recipes.py:1005:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, (base: _SupportsPow2[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3NoneOnly[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3[_E_contra@pow, _M_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: _M_contra@pow) -> _T_co@pow, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
+ setuptools/_vendor/more_itertools/recipes.py:1005:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, [_E_contra, _T_co](base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _T_co](base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _M_contra, _T_co](base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
- setuptools/command/editable_wheel.py:420:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes, /) -> str | bytes`, found `Overload[(path: str) -> str, (path: bytes) -> bytes, (path: PathLike[AnyStr@fspath]) -> AnyStr@fspath]`
+ setuptools/command/editable_wheel.py:420:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes, /) -> str | bytes`, found `Overload[(path: str) -> str, (path: bytes) -> bytes, [AnyStr](path: PathLike[AnyStr]) -> AnyStr]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-docker/tests/test_worker.py:1459:13: error[invalid-assignment] Object of type `def tracking_set_flow_run_state(flow_run_id, state, **kwargs) -> CoroutineType[Any, Any, Unknown]` is not assignable to attribute `set_flow_run_state` of type `def set_flow_run_state[T](self, flow_run_id: UUID | str, state: State[T@set_flow_run_state], force: bool = False) -> CoroutineType[Any, Any, OrchestrationResult[T@set_flow_run_state]]`
+ src/integrations/prefect-docker/tests/test_worker.py:1459:13: error[invalid-assignment] Object of type `def tracking_set_flow_run_state(flow_run_id, state, **kwargs) -> CoroutineType[Any, Any, Unknown]` is not assignable to attribute `set_flow_run_state` of type `def set_flow_run_state[T](self, flow_run_id: UUID | str, state: State[T], force: bool = False) -> CoroutineType[Any, Any, OrchestrationResult[T]]`
- src/prefect/automations.py:235:5: error[invalid-argument-type] Argument is incorrect: Expected `Overload[(cls, id: UUID, name: str | None = ...) -> Self@aread | Self@aread, (cls, id: None = None, name: str = ...) -> Self@aread | Self@aread]`, found `Overload[(cls, id: UUID, name: str | None = ...) -> CoroutineType[Any, Any, Self@read], (cls, id: None = None, name: str = ...) -> CoroutineType[Any, Any, Self@read]]`
+ src/prefect/automations.py:235:5: error[invalid-argument-type] Argument is incorrect: Expected `Overload[(cls, id: UUID, name: str | None = ...) -> Self@aread | Self@aread, (cls, id: None = None, name: str = ...) -> Self@aread | Self@aread]`, found `Overload[[Self](cls, id: UUID, name: str | None = ...) -> CoroutineType[Any, Any, Self], [Self](cls, id: None = None, name: str = ...) -> CoroutineType[Any, Any, Self]]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/task_runners.py:398:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run[_T](main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None) -> _T@run`
+ src/prefect/task_runners.py:398:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run[_T](main: Coroutine[Any, Any, _T], *, debug: bool | None = None) -> _T`
- src/prefect/task_runners.py:404:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run_task_sync[**P, R](task: Task[P@run_task_sync, R@run_task_sync], task_run_id: UUID | None = None, task_run: TaskRun | None = None, parameters: dict[str, Any] | None = None, wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, return_type: Literal["state", "result"] = "result", dependencies: dict[str, set[RunInput]] | None = None, context: dict[str, Any] | None = None) -> R@run_task_sync | State[Any] | None`
+ src/prefect/task_runners.py:404:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run_task_sync[**P, R](task: Task[P, R], task_run_id: UUID | None = None, task_run: TaskRun | None = None, parameters: dict[str, Any] | None = None, wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, return_type: Literal["state", "result"] = "result", dependencies: dict[str, set[RunInput]] | None = None, context: dict[str, Any] | None = None) -> R | State[Any] | None`
- src/prefect/task_worker.py:370:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run_task_sync[**P, R](task: Task[P@run_task_sync, R@run_task_sync], task_run_id: UUID | None = None, task_run: TaskRun | None = None, parameters: dict[str, Any] | None = None, wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, return_type: Literal["state", "result"] = "result", dependencies: dict[str, set[RunInput]] | None = None, context: dict[str, Any] | None = None) -> R@run_task_sync | State[Any] | None`
+ src/prefect/task_worker.py:370:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run_task_sync[**P, R](task: Task[P, R], task_run_id: UUID | None = None, task_run: TaskRun | None = None, parameters: dict[str, Any] | None = None, wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, return_type: Literal["state", "result"] = "result", dependencies: dict[str, set[RunInput]] | None = None, context: dict[str, Any] | None = None) -> R | State[Any] | None`
- src/prefect/utilities/asyncutils.py:376:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `Overload[(value: None = None, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[None, None], (value: R@asyncnullcontext, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[R@asyncnullcontext, None]]`
+ src/prefect/utilities/asyncutils.py:376:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `Overload[(value: None = None, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[None, None], [R](value: R, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[R, None]]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5365 diagnostics
+ Found 5370 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/asyncio/patch.py:39:12: error[invalid-argument-type] Argument to function `unwrap` is incorrect: Expected `WrappedFunction`, found `def create_task[_T](self, coro: Generator[Any, None, _T@create_task] | Coroutine[Any, Any, _T@create_task], *, name: object = None) -> Task[_T@create_task]`
+ ddtrace/contrib/internal/asyncio/patch.py:39:12: error[invalid-argument-type] Argument to function `unwrap` is incorrect: Expected `WrappedFunction`, found `def create_task[_T](self, coro: Generator[Any, None, _T] | Coroutine[Any, Any, _T], *, name: object = None) -> Task[_T]`
- tests/contrib/integration_registry/registry_update_helpers/integration_registry_manager.py:164:9: error[invalid-assignment] Object of type `def _wrapped_getattr(obj, name, *default) -> Unknown` is not assignable to attribute `getattr` of type `Overload[(o: object, name: str, /) -> Any, (o: object, name: str, default: None, /) -> Any | None, (o: object, name: str, default: bool, /) -> Any | bool, (o: object, name: str, default: list[Any], /) -> Any | list[Any], (o: object, name: str, default: dict[Any, Any], /) -> Any | dict[Any, Any], (o: object, name: str, default: _T@getattr, /) -> Any | _T@getattr]`
+ tests/contrib/integration_registry/registry_update_helpers/integration_registry_manager.py:164:9: error[invalid-assignment] Object of type `def _wrapped_getattr(obj, name, *default) -> Unknown` is not assignable to attribute `getattr` of type `Overload[(o: object, name: str, /) -> Any, (o: object, name: str, default: None, /) -> Any | None, (o: object, name: str, default: bool, /) -> Any | bool, (o: object, name: str, default: list[Any], /) -> Any | list[Any], (o: object, name: str, default: dict[Any, Any], /) -> Any | dict[Any, Any], [_T](o: object, name: str, default: _T, /) -> Any | _T]`
- tests/debugging/mocking.py:192:9: error[invalid-assignment] Object of type `(_) -> Unknown` is not assignable to attribute `register` of type `def register[**_P, _T](func: (**_P@register) -> _T@register, /, *args: _P@register.args, **kwargs: _P@register.kwargs) -> (**_P@register) -> _T@register`
+ tests/debugging/mocking.py:192:9: error[invalid-assignment] Object of type `(_) -> Unknown` is not assignable to attribute `register` of type `def register[**_P, _T](func: (**_P@register) -> _T, /, *args: _P.args, **kwargs: _P.kwargs) -> (**_P@register) -> _T`

jax (https://github.com/google/jax)
- jax/_src/checkify.py:242:16: error[invalid-argument-type] Argument to function `safe_map` is incorrect: Expected `(_SupportsShape[Never] | int | float | ... omitted 9 union elements, /) -> tuple[Any, ...] | tuple[int] | tuple[int, int]`, found `Overload[(a: _SupportsShape[Never]) -> tuple[Any, ...], (a: _SupportsShape[_ShapeT@shape]) -> _ShapeT@shape, (a: int | float | complex | bytes | str) -> tuple[()], (a: list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]) -> tuple[int], (a: list[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]] | tuple[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...], ...]) -> tuple[int, int], (a: memoryview[int] | bytearray) -> tuple[int], (a: _Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements) -> tuple[Any, ...]]`
+ jax/_src/checkify.py:242:16: error[invalid-argument-type] Argument to function `safe_map` is incorrect: Expected `(_SupportsShape[Never] | int | float | ... omitted 9 union elements, /) -> tuple[Any, ...] | tuple[int] | tuple[int, int]`, found `Overload[(a: _SupportsShape[Never]) -> tuple[Any, ...], [_ShapeT](a: _SupportsShape[_ShapeT]) -> _ShapeT, (a: int | float | complex | bytes | str) -> tuple[()], (a: list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]) -> tuple[int], (a: list[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]] | tuple[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...], ...]) -> tuple[int, int], (a: memoryview[int] | bytearray) -> tuple[int], (a: _Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements) -> tuple[Any, ...]]`
- jax/_src/checkify.py:1309:14: error[invalid-argument-type] Argument to function `safe_map` is incorrect: Expected `(_SupportsShape[Never] | int | float | ... omitted 9 union elements, /) -> tuple[Any, ...] | tuple[int] | tuple[int, int]`, found `Overload[(a: _SupportsShape[Never]) -> tuple[Any, ...], (a: _SupportsShape[_ShapeT@shape]) -> _ShapeT@shape, (a: int | float | complex | bytes | str) -> tuple[()], (a: list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]) -> tuple[int], (a: list[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]] | tuple[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...], ...]) -> tuple[int, int], (a: memoryview[int] | bytearray) -> tuple[int], (a: _Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements) -> tuple[Any, ...]]`
+ jax/_src/checkify.py:1309:14: error[invalid-argument-type] Argument to function `safe_map` is incorrect: Expected `(_SupportsShape[Never] | int | float | ... omitted 9 union elements, /) -> tuple[Any, ...] | tuple[int] | tuple[int, int]`, found `Overload[(a: _SupportsShape[Never]) -> tuple[Any, ...], [_ShapeT](a: _SupportsShape[_ShapeT]) -> _ShapeT, (a: int | float | complex | bytes | str) -> tuple[()], (a: list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]) -> tuple[int], (a: list[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...]] | tuple[list[int | float | complex | bytes | str] | tuple[int | float | complex | bytes | str, ...], ...]) -> tuple[int, int], (a: memoryview[int] | bytearray) -> tuple[int], (a: _Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements) -> tuple[Any, ...]]`
- jax/_src/interpreters/partial_eval.py:2549:13: error[invalid-argument-type] Argument to function `foreach` is incorrect: Expected `(Var, None | DynamicJaxprTracer, /) -> Any`, found `Overload[(key: Var, default: None = None, /) -> _T@setdefault | None, (key: Var, default: DynamicJaxprTracer, /) -> DynamicJaxprTracer]`
+ jax/_src/interpreters/partial_eval.py:2549:13: error[invalid-argument-type] Argument to function `foreach` is incorrect:

... (truncated 73 lines) ...
```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood reviewed on 2026-01-07 12:20_

Nice, thanks! I think there's a few details here to work out, however:

---

_Comment by @bxff on 2026-01-07 14:09_

Hi @AlexWaygood, thanks for the detailed review! I've addressed all your feedback:

1. **Specialized generic classes**: Modified `TypeMapping::update_signature_generic_context` to filter out type variables that map to concrete types. Methods on specialized classes like `Wrap[int].__init__` now correctly display without the `[T]` prefix:

   ```python
   # Before: [T](self: Wrap[int], data: int) -> None
   # After:  (self: Wrap[int], data: int) -> None
   ```

2. **Scope suffix suppression**: Added `active_scopes` tracking to `DisplaySettings` and updated `DisplayBoundTypeVarIdentity` to suppress redundant `@{scope}` suffixes when the scope is already explicit in the signature prefix (your preferred option):

   ```python
   # Before: (self: ParentDataclass[T@ChildOfParentDataclass], value: T@ChildOfParentDataclass) -> None
   # After:  [T](self: ParentDataclass[T], value: T) -> None
   ```

Let me know if you have any other suggestions!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/methods.md`:625 on 2026-01-07 14:09_

Shouldn't these be

```suggestion
reveal_type(object.__new__)  # revealed: def __new__[Self](cls) -> Self
reveal_type(object().__new__)  # revealed: def __new__[Self](cls) -> Self
```

?

---

_@AlexWaygood reviewed on 2026-01-07 14:09_

Nice!

---

_Comment by @AlexWaygood on 2026-01-07 14:17_

Could you add this test somewhere? I like the new display, but it doesn't look like we have an explicit test for the display of a non-function callable that uses both a type parameter scoped to the callable itself and a type parameter scoped to an outer scope:

```py
from typing import Generic, TypeVar
from ty_extensions import into_callable

T = TypeVar("T")
S = TypeVar("S")


class Foo(Generic[T]):
    def bar(x: T, y: S) -> tuple[T, S]:
        raise NotImplementedError


def f(x: type[Foo[T]]) -> T:
    # revealed: [S](x: T@f, y: S) -> tuple[T@f, S]
    reveal_type(into_callable(x.bar))
    raise NotImplementedError
```

---

_@bxff reviewed on 2026-01-07 16:30_

---

_Review comment by @bxff on `crates/ty_python_semantic/resources/mdtest/call/methods.md`:625 on 2026-01-07 16:30_

I've investigated and it looks like there's a pre-existing filter that intentionally hides `Self` from the type parameter display to avoid noise (since it's implicitly added to most methods).

If I remove that filter, `object.__new__` would correctly show as `def __new__[Self](cls) -> Self`. However, this would also cause `Self` to appear in other places where it might not be helpful:

- For methods on specialized classes like `C[int].f`, we'd get `def f[Self](self, x: int) -> str` where `Self` only exists for the implicit `self` parameter and doesn't appear elsewhere in the signature
- For bound methods, we'd see `[Self]` even though the type has been fully resolved, which seems confusing

What's your preference here? Should I remove the filter and accept the broader display of `[Self]`, or would you prefer a more targeted approach that only shows `Self` when it appears meaningfully in the signature (like in the return type or explicit parameters)?

---

_@AlexWaygood reviewed on 2026-01-07 16:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/methods.md`:625 on 2026-01-07 16:40_

It sounds like the easiest thing to do here (for now) would be to just restore the display for `Self` typevars specifically to what they are on `main`? I think applying this patch to your PR would do the trick:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/call/methods.md b/crates/ty_python_semantic/resources/mdtest/call/methods.md
index d3b95d81cd..c15e14a615 100644
--- a/crates/ty_python_semantic/resources/mdtest/call/methods.md
+++ b/crates/ty_python_semantic/resources/mdtest/call/methods.md
@@ -621,17 +621,17 @@ argument:
 ```py
 from typing_extensions import Self
 
-reveal_type(object.__new__)  # revealed: def __new__(cls) -> Self
-reveal_type(object().__new__)  # revealed: def __new__(cls) -> Self
-# revealed: Overload[(cls, x: str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc = 0, /) -> Self, (cls, x: str | bytes | bytearray, /, base: SupportsIndex) -> Self]
+reveal_type(object.__new__)  # revealed: def __new__(cls) -> Self@__new__
+reveal_type(object().__new__)  # revealed: def __new__(cls) -> Self@__new__
+# revealed: Overload[(cls, x: str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc = 0, /) -> Self@__new__, (cls, x: str | bytes | bytearray, /, base: SupportsIndex) -> Self@__new__]
 reveal_type(int.__new__)
-# revealed: Overload[(cls, x: str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc = 0, /) -> Self, (cls, x: str | bytes | bytearray, /, base: SupportsIndex) -> Self]
+# revealed: Overload[(cls, x: str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc = 0, /) -> Self@__new__, (cls, x: str | bytes | bytearray, /, base: SupportsIndex) -> Self@__new__]
 reveal_type((42).__new__)
 
 class X:
     def __init__(self, val: int): ...
     def make_another(self) -> Self:
-        reveal_type(self.__new__)  # revealed: def __new__(cls) -> Self
+        reveal_type(self.__new__)  # revealed: def __new__(cls) -> Self@__new__
         return self.__new__(type(self))
 ```
 
diff --git a/crates/ty_python_semantic/src/types/display.rs b/crates/ty_python_semantic/src/types/display.rs
index 378e93c94e..ff0c4c90af 100644
--- a/crates/ty_python_semantic/src/types/display.rs
+++ b/crates/ty_python_semantic/src/types/display.rs
@@ -1092,13 +1092,19 @@ struct DisplayBoundTypeVarIdentity<'db> {
 impl Display for DisplayBoundTypeVarIdentity<'_> {
     fn fmt(&self, f: &mut Formatter<'_>) -> fmt::Result {
         f.write_str(self.bound_typevar_identity.identity.name(self.db))?;
-        if let Some(binding_context_name) = self.bound_typevar_identity.binding_context.name(self.db) {
-            let mut suppressed = false;
-            if let BindingContext::Definition(definition) = self.bound_typevar_identity.binding_context {
-                if self.settings.active_scopes.contains(&definition) {
-                    suppressed = true;
-                }
-            }
+        if let Some(binding_context_name) =
+            self.bound_typevar_identity.binding_context.name(self.db)
+        {
+            let suppressed = if let BindingContext::Definition(definition) =
+                self.bound_typevar_identity.binding_context
+                && self.settings.active_scopes.contains(&definition)
+                && !self.bound_typevar_identity.identity.kind(self.db).is_self()
+            {
+                true
+            } else {
+                false
+            };
+
             if !suppressed {
                 write!(f, "@{binding_context_name}")?;
             }
````

---

_Comment by @bxff on 2026-01-07 17:55_

@AlexWaygood thanks for the patch suggestion, it would certainly restore the old behavior with minimal churn. However, after investigating, I decided to fully remove the Self filter and properly fix the underlying display logic. Here's what that entailed:

**Bound Methods:** The key issue was that bound methods needed `[Self]` suppressed when `Self` is bound to a concrete type (the implicit self), but other method-scoped type parameters should still display. I refactored the logic to use `bind_self()` before collecting type parameters, which correctly handles this. For example:
- `C[int]().m` now shows `bound method C[int].m[U](x: int, y: U) -> tuple[int, U]` without `[Self]`
- `C[int].m` shows `def m[Self, U](self, x: int, y: U) -> tuple[int, U]` with `[Self]` preserved

**Specialized Classes:** For methods on specialized generic classes like `C[int].f`, `[Self]` now correctly appears in unbound method displays because `Self` remains distinct from the specialized class parameter `T`.

**Mixed-Scope Parameters:** The new test case confirms proper handling: `[T]` is suppressed when specialized, `[Self]` appears for unbound methods (and is removed for bound), while `[U]` always appears.

The result is modern, consistent `[Self]` syntax aligned with PEP 695 style, rather than the legacy `Self@__new__` suffix. While it required updating ~8 test files, the changes are accurate reflections of the type system's state. 

---

_Review requested from @AlexWaygood by @bxff on 2026-01-08 15:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/getattr_static.md`:147 on 2026-01-08 15:14_

`[Self]` is being prepended here even though it's not displayed as the explicit annotation for any parameter types, nor is it included in the return type. I think we should avoid that, and only prepend `[Self]` if it's displayed as a parameter annotation or it's displayed in the return type.

A parameter inferred as being of type `Self` only has that annotation "hidden" as part of the display if it's the first parameter of the method and this flag is set:

https://github.com/astral-sh/ruff/blob/701f5134ab7c1a860145dccc8abb3716a3f89fe7/crates/ty_python_semantic/src/types/signatures.rs#L2187-L2192

In no other situation is the type of a parameter hidden, so this should be fairly easy to fix

---

_@AlexWaygood reviewed on 2026-01-08 15:14_

---

_@bxff reviewed on 2026-01-08 15:49_

---

_Review comment by @bxff on `crates/ty_python_semantic/resources/mdtest/call/getattr_static.md`:147 on 2026-01-08 15:49_

@AlexWaygood Implemented your suggestion: `[Self]` now only appears in the prefix when it's used in an explicit parameter annotation or the return type.

---

_Review requested from @MichaReiser by @bxff on 2026-01-08 16:12_

---

_Review requested from @Gankra by @bxff on 2026-01-08 16:12_

---

_@AlexWaygood reviewed on 2026-01-08 16:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/getattr_static.md`:147 on 2026-01-08 16:14_

Thanks! the display of this method specifically still seems to include the `[Self]` prefix though?

---

_Review requested from @AlexWaygood by @bxff on 2026-01-08 16:15_

---

_@bxff reviewed on 2026-01-08 16:31_

---

_Review comment by @bxff on `crates/ty_python_semantic/resources/mdtest/call/getattr_static.md`:147 on 2026-01-08 16:31_

Oops, I missed the same fix in 3 other display paths, now all updated.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/scoping.md`:362 on 2026-01-08 16:33_

I'm interested in this test specifically, because we have tests elsewhere that already show that we have the type parameters prepended when displaying the signature of a bound method that combines type parameters scoped to the method with type parameters from the class. But we don't have a test that explicitly checks how we display the signature of the _`Callable` supertype_ of a bound method that combines type parameters scoped to the method with type parameters from the class.

```suggestion
from typing import Generic, TypeVar
from ty_extensions import into_callable

T = TypeVar("T")
S = TypeVar("S")


class Foo(Generic[T]):
    def bar(x: T, y: S) -> tuple[T, S]:
        raise NotImplementedError


def f(x: type[Foo[T]]) -> T:
    # revealed: [S](x: T@f, y: S) -> tuple[T@f, S]
    reveal_type(into_callable(x.bar))
    raise NotImplementedError
```

---

_@AlexWaygood reviewed on 2026-01-08 16:33_

---

_@AlexWaygood reviewed on 2026-01-08 16:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/getattr_static.md`:147 on 2026-01-08 16:36_

thanks!

---

_@bxff reviewed on 2026-01-08 16:47_

---

_Review comment by @bxff on `crates/ty_python_semantic/resources/mdtest/generics/scoping.md`:362 on 2026-01-08 16:47_

Added the test, output includes `self` as expected.

---

_Review requested from @AlexWaygood by @bxff on 2026-01-08 16:55_

---

_@AlexWaygood reviewed on 2026-01-08 19:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/scoping.md`:362 on 2026-01-08 19:55_

thank you!

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-08 20:22_

---

_Comment by @AlexWaygood on 2026-01-08 20:22_

The changes to functionality look great to me now. I'll try to review the code tomorrow!

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 20:27_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 0 | 33 |
| `invalid-assignment` | 0 | 0 | 16 |
| `invalid-return-type` | 0 | 2 | 3 |
| `invalid-type-form` | 0 | 0 | 4 |
| `possibly-missing-attribute` | 0 | 0 | 1 |
| `unsupported-operator` | 0 | 0 | 1 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **2** | **58** |


**[Full report with detailed diff](https://6829c456.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://6829c456.ty-ecosystem-ext.pages.dev/timing))



---

_@AlexWaygood reviewed on 2026-01-13 17:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6902 on 2026-01-13 17:25_

nice catch!

---

_@AlexWaygood approved on 2026-01-13 17:25_

Thanks!

---

_Merged by @AlexWaygood on 2026-01-13 17:29_

---

_Closed by @AlexWaygood on 2026-01-13 17:29_

---
