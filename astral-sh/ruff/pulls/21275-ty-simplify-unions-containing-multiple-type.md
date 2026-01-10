```yaml
number: 21275
title: "[ty] Simplify unions containing multiple type variables during inference"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/typevar-unions
created_at: 2025-11-04T17:21:25Z
updated_at: 2025-11-05T15:03:21Z
url: https://github.com/astral-sh/ruff/pull/21275
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Simplify unions containing multiple type variables during inference

---

_Pull request opened by @ibraheemdev on 2025-11-04 17:21_

## Summary

Splitting this one out from https://github.com/astral-sh/ruff/pull/21210. This is also something that should be made obselete by the new constraint solver, but is easy enough to fix now.

---

_Review requested from @carljm by @ibraheemdev on 2025-11-04 17:21_

---

_Label `ty` added by @ibraheemdev on 2025-11-04 17:21_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-04 17:21_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-04 17:21_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-04 17:21_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-04 17:21_

---

_Comment by @github-actions[bot] on 2025-11-04 17:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-04 17:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo, flags: Unknown = Literal[0]) -> Match[bytes] | None]`
- tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo, flags: Unknown = Literal[0]) -> Match[bytes] | None]`
- tests/test_validators.py:228:32: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `() -> Unknown`
+ tests/test_validators.py:228:32: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str, str, int, /) -> Match[str] | None) | None`, found `() -> Unknown`
- typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((Literal["foo"], Literal["foo"], int, /) -> Match[Literal["foo"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo, flags: Unknown = Literal[0]) -> Match[bytes] | None]`
- Found 587 diagnostics
+ Found 584 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_http_request.py:1428:21: error[invalid-argument-type] Argument to function `parse_qs` is incorrect: Argument type `bytes | str` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 1699 diagnostics
+ Found 1700 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/specs/openapi/checks.py:644:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 254 diagnostics
+ Found 253 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/httputil_test.py:91:30: error[invalid-argument-type] Argument to function `qs_to_qsl` is incorrect: Expected `dict[str, list[str]]`, found `dict[Literal["a=1&b=2&a=3"], list[Literal["a=1&b=2&a=3"]]]`
- Found 332 diagnostics
+ Found 331 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/_gp/search_space.py:74:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `tuple[int | float, int | float]`, found `tuple[Unknown | ndarray[Unknown, <class 'float'>], Unknown | ndarray[Unknown, <class 'float'>]]`
- optuna/_gp/search_space.py:75:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
- optuna/_gp/search_space.py:104:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `tuple[int | float, int | float]`, found `tuple[Unknown | ndarray[Unknown, <class 'float'>], Unknown | ndarray[Unknown, <class 'float'>]]`
- optuna/_gp/search_space.py:105:21: error[invalid-argument-type] Argument to function `_normalize_one_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
- optuna/_gp/search_space.py:181:53: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `tuple[int | float, int | float]`, found `tuple[Unknown | ndarray[Unknown, <class 'float'>], Unknown | ndarray[Unknown, <class 'float'>]]`
- optuna/_gp/search_space.py:181:83: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
- tests/gp_tests/test_search_space.py:109:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `tuple[int | float, int | float]`, found `Unknown | ndarray[Unknown, <class 'float'>]`
- tests/gp_tests/test_search_space.py:110:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
- Found 583 diagnostics
+ Found 575 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/coding/strings.py:180:13: error[invalid-argument-type] Argument to bound method `map_blocks` is incorrect: Argument type `Literal["S1"]` does not satisfy upper bound `dtype[Any]` of type variable `_DType_co`
- Found 1747 diagnostics
+ Found 1748 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/type_checking.py:307:1: error[invalid-assignment] Object of type `KwargInfo[dict[Unknown, Unknown]]` is not assignable to `KwargInfo[str | list[str] | dict[str, @Todo]]`
- unittests/internaltests.py:1446:88: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
- unittests/internaltests.py:1446:122: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
- unittests/internaltests.py:1447:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[dict[Unknown, Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str | tuple[str, str]]`
- unittests/internaltests.py:1447:143: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[dict[Unknown, Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str | tuple[str, str]]`
- Found 1680 diagnostics
+ Found 1675 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/preprocessing/tests/test_encoders.py:846:63: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None | ndarray[tuple[int], <class 'object'>]`
+ sklearn/preprocessing/tests/test_encoders.py:846:63: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
- sklearn/preprocessing/tests/test_encoders.py:889:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None | ndarray[tuple[int], <class 'object'>]`
+ sklearn/preprocessing/tests/test_encoders.py:889:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/internal/ci_visibility/git_client.py:99:38: error[invalid-argument-type] Argument to function `urljoin` is incorrect: Expected `Literal["/evp_proxy/v2/api/v2/git"]`, found `Unknown | DerivedVariable[Unknown]`
+ ddtrace/internal/ci_visibility/git_client.py:99:38: error[invalid-argument-type] Argument to function `urljoin` is incorrect: Expected `str`, found `Unknown | DerivedVariable[Unknown]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/test_indexes.py:1577:11: error[type-assertion-failure] Argument does not have asserted type `Index[Any]`
- Found 5415 diagnostics
+ Found 5414 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/boolean.py:259:24: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | None`
+ pandas/core/arrays/boolean.py:259:24: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `@Todo | None`
- pandas/core/arrays/boolean.py:262:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, Unknown], ndarray[@Todo, Unknown]]`, found `tuple[@Todo | ndarray[tuple[int], <class 'bool'>], Unknown | None]`
+ pandas/core/arrays/boolean.py:262:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, Unknown], ndarray[@Todo, Unknown]]`, found `tuple[@Todo, @Todo | None]`
- pandas/core/internals/construction.py:254:29: error[invalid-argument-type] Argument to function `_ensure_2d` is incorrect: Expected `ndarray[@Todo, Unknown]`, found `Unknown | ExtensionArray | ndarray[@Todo, Unknown]`
- pandas/core/internals/managers.py:1165:21: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `<class 'object'> | Unknown`
- pandas/core/internals/managers.py:1175:24: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `<class 'object'> | Unknown`
- pandas/tests/scalar/timedelta/test_arithmetic.py:183:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `Timedelta` and `timedelta64[str]`
- pandas/tests/scalar/timedelta/test_arithmetic.py:186:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `timedelta64[str]` and `Timedelta`
- pandas/tests/scalar/timedelta/test_arithmetic.py:551:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `timedelta64[str]` and `Timedelta`
+ pandas/tests/scalar/timedelta/test_arithmetic.py:551:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `timedelta64[timedelta | int | None]` and `Timedelta`
- Found 3378 diagnostics
+ Found 3373 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/polys/tests/test_polyclasses.py:110:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `DMP[MPZ]` and `Literal[5]`
+ sympy/polys/tests/test_polyclasses.py:118:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `DMP[MPZ]` and `Literal[5]`
+ sympy/polys/tests/test_polyclasses.py:126:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `DMP[MPZ]` and `Literal[5]`
- Found 14459 diagnostics
+ Found 14462 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/linalg/tests/test_basic.py:1427:29: error[no-matching-overload] No overload of function `tri` matches arguments
+ scipy/linalg/tests/test_basic.py:1439:29: error[no-matching-overload] No overload of function `tri` matches arguments
- Found 9074 diagnostics
+ Found 9076 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:477 on 2025-11-04 17:50_

```suggestion
If the type variable is present multiple times in the union, we choose the correct union element to
```

---

_@carljm approved on 2025-11-04 17:51_

---

_Merged by @ibraheemdev on 2025-11-05 15:03_

---

_Closed by @ibraheemdev on 2025-11-05 15:03_

---

_Branch deleted on 2025-11-05 15:03_

---
