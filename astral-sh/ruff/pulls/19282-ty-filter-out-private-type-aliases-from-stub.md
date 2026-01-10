```yaml
number: 19282
title: "[ty] Filter out private type aliases from stub files when offering autocomplete suggestions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: alex/typealias-autocomplete
created_at: 2025-07-11T13:16:57Z
updated_at: 2025-07-11T13:20:40Z
url: https://github.com/astral-sh/ruff/pull/19282
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Filter out private type aliases from stub files when offering autocomplete suggestions

---

_Pull request opened by @AlexWaygood on 2025-07-11 13:16_

## Summary

A small followup to https://github.com/astral-sh/ruff/pull/19121, as promised in https://github.com/astral-sh/ruff/pull/19121#discussion_r2183052076. We now avoid offering most private type aliases from stub files as autocomplete suggestions. We filter out all explicit type aliases, and some implicit ones too.

## Test Plan

Tests added to `crates/ty_ide/src/completion.rs`. I also manually checked that `_PositiveInteger` (an alias in `builtins.pyi` from typeshed) is no longer included as an autocomplete suggestion when running the playground locally.


---

_Review requested from @carljm by @AlexWaygood on 2025-07-11 13:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-11 13:16_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-11 13:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-11 13:16_

---

_Label `server` added by @AlexWaygood on 2025-07-11 13:16_

---

_Label `ty` added by @AlexWaygood on 2025-07-11 13:16_

---

_Merged by @AlexWaygood on 2025-07-11 13:20_

---

_Closed by @AlexWaygood on 2025-07-11 13:20_

---

_Branch deleted on 2025-07-11 13:20_

---

_Comment by @github-actions[bot] on 2025-07-11 13:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
django-stubs (https://github.com/typeddjango/django-stubs)
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:74:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:74:1: Argument does not have asserted type `list[@Todo(unknown type subscript)]`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:75:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:75:1: Argument does not have asserted type `list[@Todo(unknown type subscript)]`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:76:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:76:1: Argument does not have asserted type `list[@Todo(unknown type subscript)]`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:77:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:77:1: Argument does not have asserted type `list[@Todo(unknown type subscript)]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- error[invalid-argument-type] sklearn/calibration.py:1062:66: Argument to function `len` is incorrect: Expected `Sized`, found `floating[Any] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/calibration.py:1062:66: Argument to function `len` is incorrect: Expected `Sized`, found `floating[Any] | @Todo(unknown type subscript)`
- error[invalid-argument-type] sklearn/calibration.py:1063:66: Argument to function `len` is incorrect: Expected `Sized`, found `floating[Any] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/calibration.py:1063:66: Argument to function `len` is incorrect: Expected `Sized`, found `floating[Any] | @Todo(unknown type subscript)`
- error[invalid-argument-type] sklearn/calibration.py:1064:51: Argument to function `len` is incorrect: Expected `Sized`, found `floating[Any] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/calibration.py:1064:51: Argument to function `len` is incorrect: Expected `Sized`, found `floating[Any] | @Todo(unknown type subscript)`
- error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1146:60: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Real) | (float & ~Real) | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1146:60: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Real) | (float & ~Real) | @Todo(unknown type subscript)`
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:406:12: Attribute `size` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:406:12: Attribute `size` on type `@Todo(unknown type subscript) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:409:12: Attribute `dtype` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:409:12: Attribute `dtype` on type `@Todo(unknown type subscript) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:412:30: Attribute `dtype` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:412:30: Attribute `dtype` on type `@Todo(unknown type subscript) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:415:12: Attribute `dtype` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:415:12: Attribute `dtype` on type `@Todo(unknown type subscript) | None` is possibly unbound
- error[not-iterable] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:416:42: Object of type `@Todo(Support for `typing.TypeAlias`) | None` may not be iterable
+ error[not-iterable] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:416:42: Object of type `@Todo(unknown type subscript) | None` may not be iterable
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:429:12: Attribute `dtype` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:429:12: Attribute `dtype` on type `@Todo(unknown type subscript) | None` is possibly unbound
- error[not-iterable] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:439:33: Object of type `@Todo(Support for `typing.TypeAlias`) | None` may not be iterable
+ error[not-iterable] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:439:33: Object of type `@Todo(unknown type subscript) | None` may not be iterable
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:448:14: Attribute `dtype` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:448:14: Attribute `dtype` on type `@Todo(unknown type subscript) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:461:16: Attribute `shape` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:461:16: Attribute `shape` on type `@Todo(unknown type subscript) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:465:24: Attribute `shape` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:465:24: Attribute `shape` on type `@Todo(unknown type subscript) | None` is possibly unbound
- error[invalid-argument-type] sklearn/linear_model/_logistic.py:452:27: Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Integral) | Literal[10] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/linear_model/_logistic.py:452:27: Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Integral) | Literal[10] | @Todo(unknown type subscript)`
- error[invalid-argument-type] sklearn/linear_model/_logistic.py:453:27: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Integral) | Literal[10] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/linear_model/_logistic.py:453:27: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Integral) | Literal[10] | @Todo(unknown type subscript)`

scipy (https://github.com/scipy/scipy)
- warning[possibly-unbound-attribute] scipy/_lib/_util.py:1143:33: Attribute `ndim` on type `None | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/_lib/_util.py:1143:33: Attribute `ndim` on type `None | @Todo(unknown type subscript)` is possibly unbound
- warning[possibly-unbound-attribute] scipy/_lib/pyprima/pyprima/src/pyprima/cobyla/cobyla.py:557:51: Attribute `shape` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/_lib/pyprima/pyprima/src/pyprima/cobyla/cobyla.py:557:51: Attribute `shape` on type `@Todo(unknown type subscript) | None` is possibly unbound
- error[invalid-argument-type] scipy/_lib/pyprima/pyprima/src/pyprima/cobyla/cobyla.py:557:70: Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(Support for `typing.TypeAlias`) | None`
+ error[invalid-argument-type] scipy/_lib/pyprima/pyprima/src/pyprima/cobyla/cobyla.py:557:70: Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(unknown type subscript) | None`
- warning[possibly-unbound-attribute] scipy/integrate/_quadrature.py:538:13: Attribute `reshape` on type `None | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/integrate/_quadrature.py:538:13: Attribute `reshape` on type `None | @Todo(unknown type subscript)` is possibly unbound
- warning[possibly-unbound-attribute] scipy/optimize/_lsq/least_squares.py:158:8: Attribute `ndim` on type `(Unknown & ~None & ~Literal["jac"]) | float | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/optimize/_lsq/least_squares.py:158:8: Attribute `ndim` on type `(Unknown & ~None & ~Literal["jac"]) | float | @Todo(unknown type subscript)` is possibly unbound
- warning[possibly-unbound-attribute] scipy/optimize/_lsq/least_squares.py:161:8: Attribute `shape` on type `(Unknown & ~None & ~Literal["jac"]) | float | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/optimize/_lsq/least_squares.py:161:8: Attribute `shape` on type `(Unknown & ~None & ~Literal["jac"]) | float | @Todo(unknown type subscript)` is possibly unbound
- error[invalid-argument-type] scipy/special/tests/test_basic.py:3767:13: Argument to function `assert_` is incorrect: Expected `str | (() -> str)`, found `tuple[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`), Any, Any]`
+ error[invalid-argument-type] scipy/special/tests/test_basic.py:3767:13: Argument to function `assert_` is incorrect: Expected `str | (() -> str)`, found `tuple[@Todo(unknown type subscript), @Todo(unknown type subscript), Any, Any]`
- error[invalid-return-type] scipy/stats/_multicomp.py:426:12: Return type does not match returned value: expected `tuple[ndarray[Unknown, Unknown], int | float, ndarray[Unknown, Unknown], ndarray[Unknown, Unknown]]`, found `tuple[Any, Any, floating[Any], @Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-return-type] scipy/stats/_multicomp.py:426:12: Return type does not match returned value: expected `tuple[ndarray[Unknown, Unknown], int | float, ndarray[Unknown, Unknown], ndarray[Unknown, Unknown]]`, found `tuple[Any, Any, floating[Any], @Todo(unknown type subscript)]`
- warning[possibly-unbound-attribute] scipy/stats/tests/test_multivariate.py:1605:24: Attribute `T` on type `None | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] scipy/stats/tests/test_multivariate.py:1605:24: Attribute `T` on type `None | @Todo(unknown type subscript)` is possibly unbound
- error[not-iterable] scipy/stats/tests/test_multivariate.py:1607:23: Object of type `None | @Todo(Support for `typing.TypeAlias`)` may not be iterable
+ error[not-iterable] scipy/stats/tests/test_multivariate.py:1607:23: Object of type `None | @Todo(unknown type subscript)` may not be iterable

```
</details>
No memory usage changes detected ✅


---
