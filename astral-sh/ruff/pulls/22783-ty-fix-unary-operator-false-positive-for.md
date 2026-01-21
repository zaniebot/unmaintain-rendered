```yaml
number: 22783
title: "[ty] Fix unary operator false-positive for constrained TypeVars"
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
draft: true
base: charlie/con-2
head: charlie/con-3
created_at: 2026-01-21T03:18:11Z
updated_at: 2026-01-21T03:21:12Z
url: https://github.com/astral-sh/ruff/pull/22783
synced_at: 2026-01-21T03:58:15Z
```

# [ty] Fix unary operator false-positive for constrained TypeVars

---

_@charliermarsh_

## Summary

An extension of https://github.com/astral-sh/ruff/pull/22782.


---

_Label `bug` added by @charliermarsh on 2026-01-21 03:18_

---

_Label `ty` added by @charliermarsh on 2026-01-21 03:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 03:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-21 03:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`

sympy (https://github.com/sympy/sympy)
- sympy/polys/domains/gaussiandomains.py:73:25: error[invalid-argument-type] Argument to bound method `new` is incorrect: Argument type `MPZ | MPQ` does not satisfy constraints (`MPZ`, `MPQ`) of type variable `Tdom`
- sympy/polys/domains/gaussiandomains.py:73:25: error[unsupported-operator] Unary operator `-` is not supported for object of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:73:34: error[invalid-argument-type] Argument to bound method `new` is incorrect: Argument type `MPZ | MPQ` does not satisfy constraints (`MPZ`, `MPQ`) of type variable `Tdom`
- sympy/polys/domains/gaussiandomains.py:73:34: error[unsupported-operator] Unary operator `-` is not supported for object of type `Tdom@GaussianElement`
- Found 15643 diagnostics
+ Found 15639 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14463 diagnostics
+ Found 14464 diagnostics


```

</details>


No memory usage changes detected ✅



---
