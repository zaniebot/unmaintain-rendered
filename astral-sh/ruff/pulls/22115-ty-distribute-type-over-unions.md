```yaml
number: 22115
title: "[ty] Distribute `type[]` over unions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/unions
created_at: 2025-12-20T16:27:31Z
updated_at: 2025-12-21T23:45:31Z
url: https://github.com/astral-sh/ruff/pull/22115
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Distribute `type[]` over unions

---

_Pull request opened by @charliermarsh on 2025-12-20 16:27_

## Summary

Closes https://github.com/astral-sh/ty/issues/2121.


---

_Label `ty` added by @charliermarsh on 2025-12-20 16:27_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 16:29_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 16:30_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/type_hints.py:172:46: error[invalid-argument-type] Argument to function `lenient_issubclass` is incorrect: Argument type `object` does not satisfy upper bound `type | tuple[type, ...]` of type variable `T`
- Found 149 diagnostics
+ Found 150 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/types.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `type[int] | type[float]`, found `<special-form 'typing.Annotated[int | float, <metadata>]'>`
+ pydantic/v1/types.py:318:12: error[invalid-return-type] Return type does not match returned value: expected `type[int] | type[float]`, found `type`
- Found 6716 diagnostics
+ Found 6718 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:82:20: error[unsupported-operator] Operator `>` is not supported between objects of type `MinMaxT@Min` and `Unknown | MinMaxT@Min`
+ koda_validate/generic.py:82:20: error[unsupported-operator] Operator `>` is not supported between two objects of type `MinMaxT@Min`
- koda_validate/generic.py:84:20: error[unsupported-operator] Operator `>=` is not supported between objects of type `MinMaxT@Min` and `Unknown | MinMaxT@Min`
+ koda_validate/generic.py:84:20: error[unsupported-operator] Operator `>=` is not supported between two objects of type `MinMaxT@Min`
- koda_validate/generic.py:94:20: error[unsupported-operator] Operator `<` is not supported between objects of type `MinMaxT@Max` and `Unknown | MinMaxT@Max`
+ koda_validate/generic.py:94:20: error[unsupported-operator] Operator `<` is not supported between two objects of type `MinMaxT@Max`
- koda_validate/generic.py:96:20: error[unsupported-operator] Operator `<=` is not supported between objects of type `MinMaxT@Max` and `Unknown | MinMaxT@Max`
+ koda_validate/generic.py:96:20: error[unsupported-operator] Operator `<=` is not supported between two objects of type `MinMaxT@Max`
- koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between objects of type `Num@MultipleOf` and `Unknown | Num@MultipleOf`
+ koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between two objects of type `Num@MultipleOf`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/context.py:472:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/context.py:620:39: error[invalid-argument-type] Argument is incorrect: Expected `Cog`, found `(Any & ~str & ~None) | (Cog & ~AlwaysFalsy) | Command[None, (...), Any]`
+ discord/ext/commands/core.py:680:52: warning[possibly-missing-attribute] Attribute `cog_command_error` may be missing on object of type `CogT@Command & ~None`
+ discord/ext/commands/core.py:683:35: error[invalid-argument-type] Argument is incorrect: Expected `Context[BotT@cog_command_error]`, found `Context[BotT@dispatch_error]`
+ discord/ext/commands/core.py:919:47: warning[possibly-missing-attribute] Attribute `cog_before_invoke` may be missing on object of type `CogT@Command & ~None`
+ discord/ext/commands/core.py:921:23: error[invalid-argument-type] Argument to bound method `cog_before_invoke` is incorrect: Expected `Cog`, found `CogT@Command`
+ discord/ext/commands/core.py:939:47: warning[possibly-missing-attribute] Attribute `cog_after_invoke` may be missing on object of type `CogT@Command & ~None`
+ discord/ext/commands/core.py:941:23: error[invalid-argument-type] Argument to bound method `cog_after_invoke` is incorrect: Expected `Cog`, found `CogT@Command`
+ discord/ext/commands/core.py:1312:58: warning[possibly-missing-attribute] Attribute `cog_check` may be missing on object of type `CogT@Command & ~None`
+ discord/ext/commands/core.py:1314:63: error[invalid-argument-type] Argument to function `maybe_coroutine` is incorrect: Expected `Context[BotT@cog_check]`, found `Context[BotT@can_run]`
- discord/ext/commands/help.py:1001:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 558 diagnostics
+ Found 565 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/structs.py:215:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[type[c_char], Type].__getitem__(key: type[c_char], /) -> Type` cannot be called with key of type `(type[_SimpleCData[Any]] & ~<Protocol with members '_length_'>) | (type[_Pointer[Any]] & ~<Protocol with members '_length_'>) | (type[CFuncPtr] & ~<Protocol with members '_length_'>) | (type[Union] & ~<Protocol with members '_length_'>) | (type[Structure] & ~<Protocol with members '_length_'>)` on object of type `dict[type[c_char], Type]`
- Found 2516 diagnostics
+ Found 2517 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/plot/facetgrid.py:172:43: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~None` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:173:43: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~None` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:184:24: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:185:24: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:200:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:231:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:232:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:236:44: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:760:48: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:760:62: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:761:17: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:861:38: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Any` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/tests/test_variable.py:364:40: error[unsupported-operator] Operator `<<` is not supported between objects of type `ndarray[tuple[int], dtype[float64 | Any]]` and `Literal[3]`
- xarray/tests/test_variable.py:365:40: error[unsupported-operator] Operator `>>` is not supported between objects of type `ndarray[tuple[int], dtype[float64 | Any]]` and `Literal[2]`
- xarray/tests/test_variable.py:372:40: error[unsupported-operator] Operator `<<` is not supported between two objects of type `ndarray[tuple[int], dtype[float64 | Any]]`
- xarray/tests/test_variable.py:373:40: error[unsupported-operator] Operator `>>` is not supported between two objects of type `ndarray[tuple[int], dtype[float64 | Any]]`
- Found 1770 diagnostics
+ Found 1778 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/constants/test_constants.pyi:44:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:44:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:45:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:45:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:46:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:46:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:47:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:47:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:48:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:48:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:49:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:49:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:50:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:50:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:51:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:51:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:52:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:52:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:53:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:53:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:54:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:54:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:55:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:55:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:56:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:56:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:57:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:57:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:58:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:58:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:59:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:59:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:60:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:60:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:61:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:61:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:62:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:62:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:63:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:63:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:64:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:64:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:65:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:65:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:66:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:66:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:67:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:67:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:68:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:68:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:69:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:69:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:70:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:70:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:71:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:71:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:72:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:72:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:73:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:73:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:74:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:74:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:75:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:75:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:76:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:76:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:77:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:77:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:78:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:78:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:79:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:79:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:80:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:80:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:81:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:81:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:82:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:82:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:83:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:83:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:84:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:84:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:85:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:85:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:86:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:86:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:87:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:87:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:88:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:88:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:89:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:89:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:90:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:90:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:91:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:91:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:92:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:92:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:93:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:93:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:94:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:94:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:96:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:96:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:97:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:97:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:98:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:98:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:99:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:99:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:100:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:100:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:101:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:101:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:102:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:102:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:103:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:103:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:104:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:104:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:105:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:105:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:106:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:106:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:107:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:107:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:109:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:109:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:110:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:110:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:111:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:111:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:112:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:112:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:113:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:113:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:114:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:114:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:115:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:115:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:116:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:116:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:117:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:117:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:118:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:118:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:119:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:119:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:120:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:120:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:121:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:121:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:122:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:122:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:123:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:123:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:124:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:124:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:126:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:126:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:127:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:127:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:128:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:128:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:129:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:129:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:130:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:130:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:131:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:131:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:132:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:132:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:133:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:133:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:134:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:134:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:135:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:135:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:136:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:136:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:137:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:137:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:138:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:138:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:139:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:139:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:140:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:140:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:142:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:142:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:143:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:143:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:144:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:144:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:145:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:145:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:146:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:146:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:147:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:147:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:148:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:148:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:149:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:149:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:150:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:150:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:151:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:151:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:152:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:152:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:153:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:153:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:154:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:154:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:155:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:155:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:156:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:156:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:157:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:157:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:158:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:158:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:160:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:160:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:161:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:161:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:162:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:162:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:163:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:163:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:164:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:164:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:165:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:165:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:166:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:166:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:167:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:167:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:168:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:168:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:169:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:169:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:170:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:170:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:171:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:171:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:172:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:172:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:173:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:173:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:174:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:174:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:175:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:175:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:176:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:176:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:177:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:177:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:178:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:178:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:179:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:179:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:180:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:180:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:181:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:181:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:183:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:183:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:184:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:184:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:185:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:185:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:186:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:186:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:187:1: error[type-assertion-failure] Type `@Todo` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:187:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:188:1: error[type-as

... (truncated 96 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~20MB
+     struct metadata = ~21MB


```

</details>




---

_Marked ready for review by @charliermarsh on 2025-12-20 16:40_

---

_Review requested from @carljm by @charliermarsh on 2025-12-20 16:40_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-20 16:40_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-20 16:40_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-20 16:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:90 on 2025-12-20 16:45_

I think you can use `UnionType::try_from_elements()` to avoid having to `.collect()` into a `Vec` before you union the types together

---

_@AlexWaygood reviewed on 2025-12-20 16:45_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-20 16:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 16:53_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 2 | 0 | 147 |
| `invalid-argument-type` | 51 | 2 | 2 |
| `invalid-return-type` | 2 | 1 | 7 |
| `unsupported-operator` | 0 | 4 | 5 |
| `unused-ignore-comment` | 0 | 5 | 0 |
| `possibly-missing-attribute` | 4 | 0 | 0 |
| **Total** | **59** | **12** | **161** |


**[Full report with detailed diff](https://76841239.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://76841239.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @AlexWaygood on 2025-12-20 17:00_

Hah, all the `type-assertion-failure` diagnostics on scipy-stubs looked very strange in the mypy_primer comment... but looking at the source code, they're all true positives.

`scipy-stubs` is doing stuff like this:

```py
X = 3.14

assert_type(X.__class__, type[float])
```

and we're complaining that our inferred type for `X.__class__` is `type[float]`, but the asserted type is `type[int | float]` (because of the special case where `float` in an annotation actually means `int | float`).

So I think this would be another use case for https://github.com/astral-sh/ty/issues/1762#issuecomment-3616910856, but nothing needs to be done in this PR about those diagnostics.

---

_Comment by @AlexWaygood on 2025-12-20 17:12_

Here's a minimal repro of the new artigraph diagnostic. It looks like a tricky missing case we don't yet handle when iterating over intersections! Seems like an unrelated 

```py
def f[T: tuple[int, ...] | int](x: T):
    if isinstance(x, tuple):
        reveal_type(x)
        for item in x:
            reveal_type(item)  # `object`, but should be `int`
```

`item` appears to be `Unknown` on `main` but `object` on this PR -- not totally sure why? But that's why the diagnostic appears on this branch but not on `main`

---

_Comment by @charliermarsh on 2025-12-20 17:13_

I can take a look at those artigraph diagnostics.

---

_Comment by @AlexWaygood on 2025-12-20 17:14_

All the new `colour` diagnostics appear to have `# pyright: ignore` comments next to them, so I wouldn't worry about those...

---

_Comment by @AlexWaygood on 2025-12-20 17:28_

Here's a minimal repro of the new hydypy diagnostic, which is a false positive:

```py
class Replace: ...
class Multiply: ...

def get_rule[TypeRule2: Replace | Multiply](type_: type[TypeRule2]):
    type_.__name__  # error: [unresolved-attribute] "Object of type `type[TypeRule2@get_rule]` has no attribute `__name__`"
```

It only repros if the TypeVar is bound to a union. But all `type[]` types should have a `__name__` attribute, because all `type[]` types are subtypes of `type`, and all instances of `type` have a `__name__` attribute.

I think the same thing is going on with the new `meson` diagnostic.

---

_Comment by @AlexWaygood on 2025-12-20 17:31_

The first pydantic diagnostic is a true positive that they already have a `# pyright: ignore` comment for. The second one is https://github.com/astral-sh/ty/issues/740, so you don't need to worry about that one either.

---

_Comment by @AlexWaygood on 2025-12-20 17:34_

Even if their root causes turn out to be pre-existing issues, we might need to fix the hydypy and artigraph issues before landing this -- seems like they cause a bunch of false positives otherwise :/

---

_Comment by @jorenham on 2025-12-20 18:54_

> Hah, all the `type-assertion-failure` diagnostics on scipy-stubs looked very strange in the mypy_primer comment... but looking at the source code, they're all true positives.
> 
> `scipy-stubs` is doing stuff like this:
> 
> ```python
> X = 3.14
> 
> assert_type(X.__class__, type[float])
> ```
> 
> and we're complaining that our inferred type for `X.__class__` is `type[float]`, but the asserted type is `type[int | float]` (because of the special case where `float` in an annotation actually means `int | float`).
> 
> So I think this would be another use case for [astral-sh/ty#1762 (comment)](https://github.com/astral-sh/ty/issues/1762#issuecomment-3616910856), but nothing needs to be done in this PR about those diagnostics.

I think this is a different issue from https://github.com/astral-sh/ty/issues/1762#issuecomment-3616910856.
Here, the right-hand-side of the `assert_type` (`type[float]`) isn't expanded into `type[int] | type[float]`, while the left-hand-side (`some_float.__class__`) *is*. And even if you don't consider `int | float` to be equivalent to `float`, then this is at the very least inconsistent IMO.
The error message is also different in this case, as there's no `<class 'float'>` or something like that here.

---

_Comment by @charliermarsh on 2025-12-20 18:55_

(Looking into these @AlexWaygood.)

---

_Comment by @AlexWaygood on 2025-12-20 19:05_

> I think this is a different issue from [astral-sh/ty#1762 (comment)](https://github.com/astral-sh/ty/issues/1762#issuecomment-3616910856). Here, the right-hand-side of the `assert_type` (`type[float]`) isn't expanded into `type[int] | type[float]`, while the left-hand-side (`some_float.__class__`) _is_. And even if you don't consider `int | float` to be equivalent to `float`, then this is at the very least inconsistent IMO.

@jorenham no, that's exactly backwards. We infer the type of `X` here as being precisely `float` (_not_ `float | int`), because it originates from a literal float expression. We therefore infer the type of `X.__class__` as being `type[float]` (_not_ `type[float | int]`). But because the second argument to `assert_type()` here is a type expression, we understand the second argument "type[float]" as meaning `type[float | int]` due to the `float`/`int` special case. `type[float]` and `type[float | int]` are not equivalent types, so the assertion fails. 

This seems very analogous to astral-sh/ty#1762, since `type[float]` is not a spellable type for us, given the way we've implemented the `int`/`float` special case. Whenever we see "float"  in a type expression, we implicitly transform it immediately to the union "int | float".

In order for the `assert_type` call to pass, theoretically you could use one of our ty-specific APIs... but it looks like we actually don't support `type[JustFloat]` or `type[TypeOf[3.14]]` yet, so these don't work yet unfortunately:

```py
from ty_extensions import JustFloat, TypeOf
from typing import assert_type

X = 3.14

assert_type(X.__class__, type[JustFloat])
assert_type(X.__class__, type[TypeOf[3.14]])
```

---

_Comment by @jorenham on 2025-12-20 19:13_

> @jorenham no, that's exactly backwards. We infer the type of `X` here as being precisely `float` (_not_ `float | int`), because it originates from a literal float expression. We therefore infer the type of `X.__class__` as being `type[float]` (_not_ `type[float | int]`). But because the second argument to `assert_type()` here is a type expression, we understand the second argument "type[float]" as meaning `type[float | int]` due to the `float`/`int` special case. `type[float]` and `type[float | int]` are not equivalent types, so the assertion fails.

Ah yea I see, that makes more sense that way :upside_down_face:. Sorry about the noise!

> In order for the `assert_type` call to pass, theoretically you could use one of our ty-specific APIs... but it looks like we actually don't support `type[JustFloat]` or `type[TypeOf[3.14]]` yet, so these don't work yet unfortunately:

Haha that sounds a lot like `optype.JustFloat` ([docs](https://github.com/jorenham/optype#just)), which clearly also wouldn't work here. I guess we "just" need a transparent invariant wrapper type, like `optype.Just[T]`, but less hacky.

---

_Comment by @charliermarsh on 2025-12-20 22:04_

The hydypy false-positives are fixed in https://github.com/astral-sh/ruff/pull/22116.

---

_@AlexWaygood approved on 2025-12-21 19:40_

It's a bit hard to analyze the ecosystem report because there's a bunch of false-positives being introduced (all the "object of type `type[T]` has no `__name__` attribute" ones) that you have a fix for, but the fix is in a PR stacked on top of this one (https://github.com/astral-sh/ruff/pull/22116). So I would be tempted to merge that PR into this one as a standalone commit, just so that we can double-check that it does really fix _all_ the false positives of that genre that this PR is currently introducing.

But having said that, this definitely makes sense to me as the correct way to fix this!

I haven't reviewed https://github.com/astral-sh/ruff/pull/22117 yet but I don't think it "blocks" this PR in the same way as #22116, because we only see one false positive of that genre in the ecosystem report here.

---

_Comment by @charliermarsh on 2025-12-21 20:03_

> So I would be tempted to merge that PR into this one as a standalone commit, just so that we can double-check that it does really fix all the false positives of that genre that this PR is currently introducing.

I can do that! Hopefully they both show up in the changelog, I can't remember how that works.


---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-21 23:29_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-21 23:29_

---

_Merged by @charliermarsh on 2025-12-21 23:45_

---

_Closed by @charliermarsh on 2025-12-21 23:45_

---

_Branch deleted on 2025-12-21 23:45_

---
