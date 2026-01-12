```yaml
number: 22532
title: "[ty] Apply `@runtime_checkable` across ancestors"
type: pull_request
state: closed
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/runtime-checkable
created_at: 2026-01-12T13:54:20Z
updated_at: 2026-01-12T14:51:40Z
url: https://github.com/astral-sh/ruff/pull/22532
synced_at: 2026-01-12T15:57:52Z
```

# [ty] Apply `@runtime_checkable` across ancestors

---

_@charliermarsh_

## Summary

I believe this is correct, though the initial motivation for it was lost. (It was in https://github.com/astral-sh/ruff/pull/22327, but reverting there doesn't cause any tests to fail.)

---

_Label `ty` added by @charliermarsh on 2026-01-12 13:54_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 13:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-12 13:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14503 diagnostics
+ Found 14504 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-12 14:18_

---

_Review requested from @carljm by @charliermarsh on 2026-01-12 14:18_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-12 14:18_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-12 14:18_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-12 14:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2483 on 2026-01-12 14:29_

Where does PEP 544 say this?

Also, PEP 544 is a historical document that records a decision taken several years ago and deliberately isn't kept up to date -- can we link to the living documentation (the spec) here?

---

_@AlexWaygood reviewed on 2026-01-12 14:31_

---

_@AlexWaygood reviewed on 2026-01-12 14:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2483 on 2026-01-12 14:33_

It's true that this is how things work at runtime currently, but that honestly seems a bit questionable, and there's an open issue asking if we should deprecate it: https://github.com/python/cpython/issues/132604. I wasn't aware of anywhere in a PEP or the spec that specified this behaviour.

---

_@charliermarsh reviewed on 2026-01-12 14:51_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2483 on 2026-01-12 14:51_

You're right, this was bad form -- the PEP doesn't say that, it's just an implementation detail in CPython AFAICT. I will close out.

---

_Closed by @charliermarsh on 2026-01-12 14:51_

---
