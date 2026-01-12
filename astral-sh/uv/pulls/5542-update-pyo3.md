```yaml
number: 5542
title: Update pyo3
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/pyo3
created_at: 2024-07-29T00:10:10Z
updated_at: 2024-07-29T00:31:50Z
url: https://github.com/astral-sh/uv/pull/5542
synced_at: 2026-01-12T16:06:53Z
```

# Update pyo3

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [pyo3](https://togithub.com/pyo3/pyo3) | workspace.dependencies | minor | `0.21.0` -> `0.22.0` |
| [pyo3-log](https://togithub.com/vorner/pyo3-log) | workspace.dependencies | minor | `0.10.0` -> `0.11.0` |

---

### Release Notes

<details>
<summary>pyo3/pyo3 (pyo3)</summary>

### [`v0.22.2`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0222---2024-07-17)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.22.1...v0.22.2)

##### Packaging

-   Require opt-in to freethreaded Python using the `UNSAFE_PYO3_BUILD_FREE_THREADED=1` environment variable (it is not yet supported by PyO3). [#&#8203;4327](https://togithub.com/PyO3/pyo3/pull/4327)

##### Changed

-   Use FFI function calls for reference counting on all abi3 versions. [#&#8203;4324](https://togithub.com/PyO3/pyo3/pull/4324)
-   `#[pymodule(...)]` now directly accepts all relevant `#[pyo3(...)]` options. [#&#8203;4330](https://togithub.com/PyO3/pyo3/pull/4330)

##### Fixed

-   Fix compile failure in declarative `#[pymodule]` under presence of `#![no_implicit_prelude]`. [#&#8203;4328](https://togithub.com/PyO3/pyo3/pull/4328)
-   Fix compile failure due to c-string literals on Rust < 1.79. [#&#8203;4353](https://togithub.com/PyO3/pyo3/pull/4353)

### [`v0.22.1`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0221---2024-07-06)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.22.0...v0.22.1)

##### Added

-   Add `#[pyo3(submodule)]` option for declarative `#[pymodule]`s. [#&#8203;4301](https://togithub.com/PyO3/pyo3/pull/4301)
-   Implement `PartialEq<bool>` for `Bound<'py, PyBool>`. [#&#8203;4305](https://togithub.com/PyO3/pyo3/pull/4305)

##### Fixed

-   Return `NotImplemented` instead of raising `TypeError` from generated equality method when comparing different types. [#&#8203;4287](https://togithub.com/PyO3/pyo3/pull/4287)
-   Handle full-path `#[pyo3::prelude::pymodule]` and similar for `#[pyclass]` and `#[pyfunction]` in declarative modules.[#&#8203;4288](https://togithub.com/PyO3/pyo3/pull/4288)
-   Fix 128-bit int regression on big-endian platforms with Python <3.13. [#&#8203;4291](https://togithub.com/PyO3/pyo3/pull/4291)
-   Stop generating code that will never be covered with declarative modules. [#&#8203;4297](https://togithub.com/PyO3/pyo3/pull/4297)
-   Fix invalid deprecation warning for trailing optional on `#[setter]` function. [#&#8203;4304](https://togithub.com/PyO3/pyo3/pull/4304)

### [`v0.22.0`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0220---2024-06-24)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.21.2...v0.22.0)

##### Packaging

-   Update `heck` dependency to 0.5. [#&#8203;3966](https://togithub.com/PyO3/pyo3/pull/3966)
-   Extend range of supported versions of `chrono-tz` optional dependency to include version 0.10. [#&#8203;4061](https://togithub.com/PyO3/pyo3/pull/4061)
-   Update MSRV to 1.63. [#&#8203;4129](https://togithub.com/PyO3/pyo3/pull/4129)
-   Add optional `num-rational` feature to add conversions with Python's `fractions.Fraction`. [#&#8203;4148](https://togithub.com/PyO3/pyo3/pull/4148)
-   Support Python 3.13. [#&#8203;4184](https://togithub.com/PyO3/pyo3/pull/4184)

##### Added

-   Add `PyWeakref`, `PyWeakrefReference` and `PyWeakrefProxy`. [#&#8203;3835](https://togithub.com/PyO3/pyo3/pull/3835)
-   Support `#[pyclass]` on enums that have tuple variants. [#&#8203;4072](https://togithub.com/PyO3/pyo3/pull/4072)
-   Add support for scientific notation in `Decimal` conversion. [#&#8203;4079](https://togithub.com/PyO3/pyo3/pull/4079)
-   Add `pyo3_disable_reference_pool` conditional compilation flag to avoid the overhead of the global reference pool at the cost of known limitations as explained in the performance section of the guide. [#&#8203;4095](https://togithub.com/PyO3/pyo3/pull/4095)
-   Add `#[pyo3(constructor = (...))]` to customize the generated constructors for complex enum variants. [#&#8203;4158](https://togithub.com/PyO3/pyo3/pull/4158)
-   Add `PyType::module`, which always matches Python `__module__`. [#&#8203;4196](https://togithub.com/PyO3/pyo3/pull/4196)
-   Add `PyType::fully_qualified_name` which matches the "fully qualified name" defined in [PEP 737](https://peps.python.org/pep-0737). [#&#8203;4196](https://togithub.com/PyO3/pyo3/pull/4196)
-   Add `PyTypeMethods::mro` and `PyTypeMethods::bases`. [#&#8203;4197](https://togithub.com/PyO3/pyo3/pull/4197)
-   Add `#[pyclass(ord)]` to implement ordering based on `PartialOrd`. [#&#8203;4202](https://togithub.com/PyO3/pyo3/pull/4202)
-   Implement `ToPyObject` and `IntoPy<PyObject>` for `PyBackedStr` and `PyBackedBytes`. [#&#8203;4205](https://togithub.com/PyO3/pyo3/pull/4205)
-   Add `#[pyclass(hash)]` option to implement `__hash__` in terms of the `Hash` implementation [#&#8203;4206](https://togithub.com/PyO3/pyo3/pull/4206)
-   Add `#[pyclass(eq)]` option to generate `__eq__` based on `PartialEq`, and `#[pyclass(eq_int)]` for simple enums to implement equality based on their discriminants. [#&#8203;4210](https://togithub.com/PyO3/pyo3/pull/4210)
-   Implement `From<Bound<'py, T>>` for `PyClassInitializer<T>`. [#&#8203;4214](https://togithub.com/PyO3/pyo3/pull/4214)
-   Add `as_super` methods to `PyRef` and `PyRefMut` for accesing the base class by reference. [#&#8203;4219](https://togithub.com/PyO3/pyo3/pull/4219)
-   Implement `PartialEq<str>` for `Bound<'py, PyString>`. [#&#8203;4245](https://togithub.com/PyO3/pyo3/pull/4245)
-   Implement `PyModuleMethods::filename` on PyPy. [#&#8203;4249](https://togithub.com/PyO3/pyo3/pull/4249)
-   Implement `PartialEq<[u8]>` for `Bound<'py, PyBytes>`. [#&#8203;4250](https://togithub.com/PyO3/pyo3/pull/4250)
-   Add `pyo3_ffi::c_str` macro to create `&'static CStr` on Rust versions which don't have 1.77's `c""` literals. [#&#8203;4255](https://togithub.com/PyO3/pyo3/pull/4255)
-   Support `bool` conversion with `numpy` 2.0's `numpy.bool` type [#&#8203;4258](https://togithub.com/PyO3/pyo3/pull/4258)
-   Add `PyAnyMethods::{bitnot, matmul, floor_div, rem, divmod}`. [#&#8203;4264](https://togithub.com/PyO3/pyo3/pull/4264)

##### Changed

-   Change the type of `PySliceIndices::slicelength` and the `length` parameter of `PySlice::indices()`. [#&#8203;3761](https://togithub.com/PyO3/pyo3/pull/3761)
-   Deprecate implicit default for trailing optional arguments [#&#8203;4078](https://togithub.com/PyO3/pyo3/pull/4078)
-   `Clone`ing pointers into the Python heap has been moved behind the `py-clone` feature, as it must panic without the GIL being held as a soundness fix. [#&#8203;4095](https://togithub.com/PyO3/pyo3/pull/4095)
-   Add `#[track_caller]` to all `Py<T>`, `Bound<'py, T>` and `Borrowed<'a, 'py, T>` methods which can panic. [#&#8203;4098](https://togithub.com/PyO3/pyo3/pull/4098)
-   Change `PyAnyMethods::dir` to be fallible and return `PyResult<Bound<'py, PyList>>` (and similar for `PyAny::dir`). [#&#8203;4100](https://togithub.com/PyO3/pyo3/pull/4100)
-   The global reference pool (to track pending reference count decrements) is now initialized lazily to avoid the overhead of taking a mutex upon function entry when the functionality is not actually used. [#&#8203;4178](https://togithub.com/PyO3/pyo3/pull/4178)
-   Emit error messages when using `weakref` or `dict` when compiling for `abi3` for Python older than 3.9. [#&#8203;4194](https://togithub.com/PyO3/pyo3/pull/4194)
-   Change `PyType::name` to always match Python `__name__`. [#&#8203;4196](https://togithub.com/PyO3/pyo3/pull/4196)
-   Remove CPython internal ffi call for complex number including: add, sub, mul, div, neg, abs, pow. Added PyAnyMethods::{abs, pos, neg} [#&#8203;4201](https://togithub.com/PyO3/pyo3/pull/4201)
-   Deprecate implicit integer comparision for simple enums in favor of `#[pyclass(eq_int)]`. [#&#8203;4210](https://togithub.com/PyO3/pyo3/pull/4210)
-   Set the `module=` attribute of declarative modules' child `#[pymodule]`s and `#[pyclass]`es. [#&#8203;4213](https://togithub.com/PyO3/pyo3/pull/4213)
-   Set the `module` option for complex enum variants from the value set on the complex enum `module`. [#&#8203;4228](https://togithub.com/PyO3/pyo3/pull/4228)
-   Respect the Python "limited API" when building for the `abi3` feature on PyPy or GraalPy. [#&#8203;4237](https://togithub.com/PyO3/pyo3/pull/4237)
-   Optimize code generated by `#[pyo3(get)]` on `#[pyclass]` fields. [#&#8203;4254](https://togithub.com/PyO3/pyo3/pull/4254)
-   `PyCFunction::new`, `PyCFunction::new_with_keywords` and `PyCFunction::new_closure` now take `&'static CStr` name and doc arguments (previously was `&'static str`). [#&#8203;4255](https://togithub.com/PyO3/pyo3/pull/4255)
-   The `experimental-declarative-modules` feature is now stabilized and available by default. [#&#8203;4257](https://togithub.com/PyO3/pyo3/pull/4257)

##### Fixed

-   Fix panic when `PYO3_CROSS_LIB_DIR` is set to a missing path. [#&#8203;4043](https://togithub.com/PyO3/pyo3/pull/4043)
-   Fix a compile error when exporting an exception created with `create_exception!` living in a different Rust module using the `declarative-module` feature. [#&#8203;4086](https://togithub.com/PyO3/pyo3/pull/4086)
-   Fix FFI definitions of `PY_VECTORCALL_ARGUMENTS_OFFSET` and `PyVectorcall_NARGS` to fix a false-positive assertion. [#&#8203;4104](https://togithub.com/PyO3/pyo3/pull/4104)
-   Disable `PyUnicode_DATA` on PyPy: not exposed by PyPy. [#&#8203;4116](https://togithub.com/PyO3/pyo3/pull/4116)
-   Correctly handle `#[pyo3(from_py_with = ...)]` attribute on dunder (`__magic__`) method arguments instead of silently ignoring it. [#&#8203;4117](https://togithub.com/PyO3/pyo3/pull/4117)
-   Fix a compile error when declaring a standalone function or class method with a Python name that is a Rust keyword. [#&#8203;4226](https://togithub.com/PyO3/pyo3/pull/4226)
-   Fix declarative modules discarding doc comments on the `mod` node. [#&#8203;4236](https://togithub.com/PyO3/pyo3/pull/4236)
-   Fix `__dict__` attribute missing for `#[pyclass(dict)]` instances when building for `abi3` on Python 3.9. [#&#8203;4251](https://togithub.com/PyO3/pyo3/pull/4251)

</details>

<details>
<summary>vorner/pyo3-log (pyo3-log)</summary>

### [`v0.11.0`](https://togithub.com/vorner/pyo3-log/blob/HEAD/CHANGELOG.md#0110)

-   Update to pyo3 0.22.
-   `Logger` not longer implements `Clone`

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://togithub.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy40NDAuNyIsInVwZGF0ZWRJblZlciI6IjM3LjQ0MC43IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-07-29 00:10_

---

_Closed by @charliermarsh on 2024-07-29 00:31_

---
