```yaml
number: 2879
title: Update Rust crate pyo3 to 0.21.0 - autoclosed
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/pyo3-0.x
created_at: 2024-04-08T03:04:37Z
updated_at: 2024-04-08T21:09:04Z
url: https://github.com/astral-sh/uv/pull/2879
synced_at: 2026-01-12T16:05:17Z
```

# Update Rust crate pyo3 to 0.21.0 - autoclosed

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [pyo3](https://togithub.com/pyo3/pyo3) | dependencies | minor | `0.20.0` -> `0.21.0` |
| [pyo3](https://togithub.com/pyo3/pyo3) | workspace.dependencies | minor | `0.20.3` -> `0.21.0` |

---

### Release Notes

<details>
<summary>pyo3/pyo3 (pyo3)</summary>

### [`v0.21.1`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0211---2024-04-01)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.21.0...v0.21.1)

##### Added

-   Implement `Send` and `Sync` for `PyBackedStr` and `PyBackedBytes`. [#&#8203;4007](https://togithub.com/PyO3/pyo3/pull/4007)
-   Implement `Clone`, `Debug`, `PartialEq`, `Eq`, `PartialOrd`, `Ord` and `Hash` implementation for `PyBackedBytes` and `PyBackedStr`, and `Display` for `PyBackedStr`. [#&#8203;4020](https://togithub.com/PyO3/pyo3/pull/4020)
-   Add `import_exception_bound!` macro to import exception types without generating GIL Ref functionality for them. [#&#8203;4027](https://togithub.com/PyO3/pyo3/pull/4027)

##### Changed

-   Emit deprecation warning for uses of GIL Refs as `#[setter]` function arguments. [#&#8203;3998](https://togithub.com/PyO3/pyo3/pull/3998)
-   Add `#[inline]` hints on many `Bound` and `Borrowed` methods. [#&#8203;4024](https://togithub.com/PyO3/pyo3/pull/4024)

##### Fixed

-   Handle `#[pyo3(from_py_with = "")]` in `#[setter]` methods [#&#8203;3995](https://togithub.com/PyO3/pyo3/pull/3995)
-   Allow extraction of `&Bound` in `#[setter]` methods. [#&#8203;3998](https://togithub.com/PyO3/pyo3/pull/3998)
-   Fix some uncovered code blocks emitted by `#[pymodule]`, `#[pyfunction]` and `#[pyclass]` macros. [#&#8203;4009](https://togithub.com/PyO3/pyo3/pull/4009)
-   Fix typo in the panic message when a class referenced in `pyo3::import_exception!` does not exist. [#&#8203;4012](https://togithub.com/PyO3/pyo3/pull/4012)
-   Fix compile error when using an async `#[pymethod]` with a receiver and additional arguments. [#&#8203;4015](https://togithub.com/PyO3/pyo3/pull/4015)

### [`v0.21.0`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0210---2024-03-25)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.20.3...v0.21.0)

##### Added

-   Add support for GraalPy (24.0 and up). [#&#8203;3247](https://togithub.com/PyO3/pyo3/pull/3247)
-   Add `PyMemoryView` type. [#&#8203;3514](https://togithub.com/PyO3/pyo3/pull/3514)
-   Allow `async fn` in for `#[pyfunction]` and `#[pymethods]`, with the `experimental-async` feature. [#&#8203;3540](https://togithub.com/PyO3/pyo3/pull/3540) [#&#8203;3588](https://togithub.com/PyO3/pyo3/pull/3588) [#&#8203;3599](https://togithub.com/PyO3/pyo3/pull/3599) [#&#8203;3931](https://togithub.com/PyO3/pyo3/pull/3931)
-   Implement `PyTypeInfo` for `PyEllipsis`, `PyNone` and `PyNotImplemented`. [#&#8203;3577](https://togithub.com/PyO3/pyo3/pull/3577)
-   Support `#[pyclass]` on enums that have non-unit variants. [#&#8203;3582](https://togithub.com/PyO3/pyo3/pull/3582)
-   Support `chrono` feature with `abi3` feature. [#&#8203;3664](https://togithub.com/PyO3/pyo3/pull/3664)
-   `FromPyObject`, `IntoPy<PyObject>` and `ToPyObject` are implemented on `std::duration::Duration` [#&#8203;3670](https://togithub.com/PyO3/pyo3/pull/3670)
-   Add `PyString::to_cow`. Add `Py<PyString>::to_str`, `Py<PyString>::to_cow`, and `Py<PyString>::to_string_lossy`, as ways to access Python string data safely beyond the GIL lifetime. [#&#8203;3677](https://togithub.com/PyO3/pyo3/pull/3677)
-   Add `Bound<T>` and `Borrowed<T>` smart pointers as a new API for accessing Python objects. [#&#8203;3686](https://togithub.com/PyO3/pyo3/pull/3686)
-   Add `PyNativeType::as_borrowed` to convert "GIL refs" to the new `Bound` smart pointer. [#&#8203;3692](https://togithub.com/PyO3/pyo3/pull/3692)
-   Add `FromPyObject::extract_bound` method, to migrate `FromPyObject` implementations to the Bound API. [#&#8203;3706](https://togithub.com/PyO3/pyo3/pull/3706)
-   Add `gil-refs` feature to allow continued use of the deprecated GIL Refs APIs. [#&#8203;3707](https://togithub.com/PyO3/pyo3/pull/3707)
-   Add methods to `PyAnyMethods` for binary operators (`add`, `sub`, etc.) [#&#8203;3712](https://togithub.com/PyO3/pyo3/pull/3712)
-   Add `chrono-tz` feature allowing conversion between `chrono_tz::Tz` and `zoneinfo.ZoneInfo` [#&#8203;3730](https://togithub.com/PyO3/pyo3/pull/3730)
-   Add FFI definition `PyType_GetModuleByDef`. [#&#8203;3734](https://togithub.com/PyO3/pyo3/pull/3734)
-   Conversion between `std::time::SystemTime` and `datetime.datetime` [#&#8203;3736](https://togithub.com/PyO3/pyo3/pull/3736)
-   Add `Py::as_any` and `Py::into_any`. [#&#8203;3785](https://togithub.com/PyO3/pyo3/pull/3785)
-   Add `PyStringMethods::encode_utf8`. [#&#8203;3801](https://togithub.com/PyO3/pyo3/pull/3801)
-   Add `PyBackedStr` and `PyBackedBytes`, as alternatives to `&str` and `&bytes` where a Python object owns the data. [#&#8203;3802](https://togithub.com/PyO3/pyo3/pull/3802) [#&#8203;3991](https://togithub.com/PyO3/pyo3/pull/3991)
-   Allow `#[pymodule]` macro on Rust `mod` blocks, with the `experimental-declarative-modules` feature. [#&#8203;3815](https://togithub.com/PyO3/pyo3/pull/3815)
-   Implement `ExactSizeIterator` for `set` and `frozenset` iterators on `abi3` feature. [#&#8203;3849](https://togithub.com/PyO3/pyo3/pull/3849)
-   Add `Py::drop_ref` to explicitly drop a \`Py\`\` and immediately decrease the Python reference count if the GIL is already held. [#&#8203;3871](https://togithub.com/PyO3/pyo3/pull/3871)
-   Allow `#[pymodule]` macro on single argument functions that take `&Bound<'_, PyModule>`. [#&#8203;3905](https://togithub.com/PyO3/pyo3/pull/3905)
-   Implement `FromPyObject` for `Cow<str>`. [#&#8203;3928](https://togithub.com/PyO3/pyo3/pull/3928)
-   Implement `Default` for `GILOnceCell`. [#&#8203;3971](https://togithub.com/PyO3/pyo3/pull/3971)
-   Add `PyDictMethods::into_mapping`, `PyListMethods::into_sequence` and `PyTupleMethods::into_sequence`. [#&#8203;3982](https://togithub.com/PyO3/pyo3/pull/3982)

##### Changed

-   `PyDict::from_sequence` now takes a single argument of type `&PyAny` (previously took two arguments `Python` and `PyObject`). [#&#8203;3532](https://togithub.com/PyO3/pyo3/pull/3532)
-   Deprecate `Py::is_ellipsis` and `PyAny::is_ellipsis` in favour of `any.is(py.Ellipsis())`. [#&#8203;3577](https://togithub.com/PyO3/pyo3/pull/3577)
-   Split some `PyTypeInfo` functionality into new traits `HasPyGilRef` and `PyTypeCheck`. [#&#8203;3600](https://togithub.com/PyO3/pyo3/pull/3600)
-   Deprecate `PyTryFrom` and `PyTryInto` traits in favor of `any.downcast()` via the `PyTypeCheck` and `PyTypeInfo` traits. [#&#8203;3601](https://togithub.com/PyO3/pyo3/pull/3601)
-   Allow async methods to accept `&self`/`&mut self` [#&#8203;3609](https://togithub.com/PyO3/pyo3/pull/3609)
-   `FromPyObject` for set types now also accept `frozenset` objects as input. [#&#8203;3632](https://togithub.com/PyO3/pyo3/pull/3632)
-   `FromPyObject` for `bool` now also accepts NumPy's `bool_` as input. [#&#8203;3638](https://togithub.com/PyO3/pyo3/pull/3638)
-   Add `AsRefSource` associated type to `PyNativeType`. [#&#8203;3653](https://togithub.com/PyO3/pyo3/pull/3653)
-   Rename `.is_true` to `.is_truthy` on `PyAny` and `Py<PyAny>` to clarify that the test is not based on identity with or equality to the True singleton. [#&#8203;3657](https://togithub.com/PyO3/pyo3/pull/3657)
-   `PyType::name` is now `PyType::qualname` whereas `PyType::name` efficiently accesses the full name which includes the module name. [#&#8203;3660](https://togithub.com/PyO3/pyo3/pull/3660)
-   The `Iter(A)NextOutput` types are now deprecated and `__(a)next__` can directly return anything which can be converted into Python objects, i.e. awaitables do not need to be wrapped into `IterANextOutput` or `Option` any more. `Option` can still be used as well and returning `None` will trigger the fast path for `__next__`, stopping iteration without having to raise a `StopIteration` exception. [#&#8203;3661](https://togithub.com/PyO3/pyo3/pull/3661)
-   Implement `FromPyObject` on `chrono::DateTime<Tz>` for all `Tz`, not just `FixedOffset` and `Utc`. [#&#8203;3663](https://togithub.com/PyO3/pyo3/pull/3663)
-   Add lifetime parameter to `PyTzInfoAccess` trait. For the deprecated gil-ref API, the trait is now implemented for `&'py PyTime` and `&'py PyDateTime` instead of `PyTime` and `PyDate`. [#&#8203;3679](https://togithub.com/PyO3/pyo3/pull/3679)
-   Calls to `__traverse__` become no-ops for unsendable pyclasses if on the wrong thread, thereby avoiding hard aborts at the cost of potential leakage. [#&#8203;3689](https://togithub.com/PyO3/pyo3/pull/3689)
-   Include `PyNativeType` in `pyo3::prelude`. [#&#8203;3692](https://togithub.com/PyO3/pyo3/pull/3692)
-   Improve performance of `extract::<i64>` (and other integer types) by avoiding call to `__index__()` converting the value to an integer for 3.10+. Gives performance improvement of around 30% for successful extraction. [#&#8203;3742](https://togithub.com/PyO3/pyo3/pull/3742)
-   Relax bound of `FromPyObject` for `Py<T>` to just `T: PyTypeCheck`. [#&#8203;3776](https://togithub.com/PyO3/pyo3/pull/3776)
-   `PySet` and `PyFrozenSet` iterators now always iterate the equivalent of `iter(set)`. (A "fast path" with no noticeable performance benefit was removed.) [#&#8203;3849](https://togithub.com/PyO3/pyo3/pull/3849)
-   Move implementations of `FromPyObject` for `&str`, `Cow<str>`, `&[u8]` and `Cow<[u8]>` onto a temporary trait `FromPyObjectBound` when `gil-refs` feature is deactivated. [#&#8203;3928](https://togithub.com/PyO3/pyo3/pull/3928)
-   Deprecate `GILPool`, `Python::with_pool`, and `Python::new_pool`. [#&#8203;3947](https://togithub.com/PyO3/pyo3/pull/3947)

##### Removed

-   Remove all functionality deprecated in PyO3 0.19. [#&#8203;3603](https://togithub.com/PyO3/pyo3/pull/3603)

##### Fixed

-   Match PyPy 7.3.14 in removing PyPy-only symbol `Py_MAX_NDIMS` in favour of `PyBUF_MAX_NDIM`. [#&#8203;3757](https://togithub.com/PyO3/pyo3/pull/3757)
-   Fix segmentation fault using `datetime` types when an invalid `datetime` module is on sys.path. [#&#8203;3818](https://togithub.com/PyO3/pyo3/pull/3818)
-   Fix `non_local_definitions` lint warning triggered by many PyO3 macros. [#&#8203;3901](https://togithub.com/PyO3/pyo3/pull/3901)
-   Disable `PyCode` and `PyCode_Type` on PyPy: `PyCode_Type` is not exposed by PyPy. [#&#8203;3934](https://togithub.com/PyO3/pyo3/pull/3934)

### [`v0.20.3`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0203---2024-02-23)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.20.2...v0.20.3)

##### Packaging

-   Add `portable-atomic` dependency. [#&#8203;3619](https://togithub.com/PyO3/pyo3/pull/3619)
-   Check maximum version of Python at build time and for versions not yet supported require opt-in to the `abi3` stable ABI by the environment variable `PYO3_USE_ABI3_FORWARD_COMPATIBILITY=1`. [#&#8203;3821](https://togithub.com/PyO3/pyo3/pull/3821)

##### Fixed

-   Use `portable-atomic` to support platforms without 64-bit atomics. [#&#8203;3619](https://togithub.com/PyO3/pyo3/pull/3619)
-   Fix compilation failure with `either` feature enabled without `experimental-inspect` enabled. [#&#8203;3834](https://togithub.com/PyO3/pyo3/pull/3834)

### [`v0.20.2`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0202---2024-01-04)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.20.1...v0.20.2)

##### Packaging

-   Pin `pyo3` and `pyo3-ffi` dependencies on `pyo3-build-config` to require the same patch version, i.e. `pyo3` 0.20.2 requires *exactly* `pyo3-build-config` 0.20.2. [#&#8203;3721](https://togithub.com/PyO3/pyo3/pull/3721)

##### Fixed

-   Fix compile failure when building `pyo3` 0.20.0 with latest `pyo3-build-config` 0.20.X. [#&#8203;3724](https://togithub.com/PyO3/pyo3/pull/3724)
-   Fix docs.rs build. [#&#8203;3722](https://togithub.com/PyO3/pyo3/pull/3722)

### [`v0.20.1`](https://togithub.com/pyo3/pyo3/blob/HEAD/CHANGELOG.md#0201---2023-12-30)

[Compare Source](https://togithub.com/pyo3/pyo3/compare/v0.20.0...v0.20.1)

##### Added

-   Add optional `either` feature to add conversions for `either::Either<L, R>` sum type. [#&#8203;3456](https://togithub.com/PyO3/pyo3/pull/3456)
-   Add optional `smallvec` feature to add conversions for `smallvec::SmallVec`. [#&#8203;3507](https://togithub.com/PyO3/pyo3/pull/3507)
-   Add `take` and `into_inner` methods to `GILOnceCell` [#&#8203;3556](https://togithub.com/PyO3/pyo3/pull/3556)
-   `#[classmethod]` methods can now also receive `Py<PyType>` as their first argument. [#&#8203;3587](https://togithub.com/PyO3/pyo3/pull/3587)
-   `#[pyfunction(pass_module)]` can now also receive `Py<PyModule>` as their first argument. [#&#8203;3587](https://togithub.com/PyO3/pyo3/pull/3587)
-   Add `traverse` method to `GILProtected`. [#&#8203;3616](https://togithub.com/PyO3/pyo3/pull/3616)
-   Added `abi3-py312` feature [#&#8203;3687](https://togithub.com/PyO3/pyo3/pull/3687)

##### Fixed

-   Fix minimum version specification for optional `chrono` dependency. [#&#8203;3512](https://togithub.com/PyO3/pyo3/pull/3512)
-   Silenced new `clippy::unnecessary_fallible_conversions` warning when using a `Py<Self>` `self` receiver. [#&#8203;3564](https://togithub.com/PyO3/pyo3/pull/3564)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Comment by @renovate[bot] on 2024-04-08 03:04_

### ⚠ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --workspace
    Updating crates.io index
error: failed to select a version for `pyo3-ffi`.
    ... required by package `pyo3 v0.16.2`
    ... which satisfies dependency `pyo3 = ">=0.15, <0.21"` of package `pyo3-log v0.9.0`
    ... which satisfies dependency `pyo3-log = "^0.9.0"` of package `pep508_rs v0.4.2 (/tmp/renovate/repos/github/astral-sh/uv/crates/pep508-rs)`
    ... which satisfies path dependency `pep508_rs` (locked to 0.4.2) of package `uv v0.1.29 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv)`
versions that meet the requirements `=0.16.2` are: 0.16.2

the package `pyo3-ffi` links to the native library `python`, but it conflicts with a previous package which links to `python` as well:
package `pyo3-ffi v0.21.0`
    ... which satisfies dependency `pyo3-ffi = "=0.21.0"` of package `pyo3 v0.21.0`
    ... which satisfies dependency `pyo3 = "^0.21.0"` of package `pep440_rs v0.5.0 (/tmp/renovate/repos/github/astral-sh/uv/crates/pep440-rs)`
    ... which satisfies path dependency `pep440_rs` (locked to 0.5.0) of package `uv-dev v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv-dev)`
Only one package in the dependency graph may specify the same links value. This helps ensure that only one copy of a native library is linked in the final binary. Try to adjust your dependencies so that only one package uses the `links = "python"` value. For more information, see https://doc.rust-lang.org/cargo/reference/resolver.html#links.

failed to select a version for `pyo3-ffi` which could resolve this conflict

```



---

_Label `internal` added by @renovate[bot] on 2024-04-08 03:04_

---

_Renamed from "Update Rust crate pyo3 to 0.21.0" to "Update Rust crate pyo3 to 0.21.0 - autoclosed" by @renovate[bot] on 2024-04-08 21:09_

---

_Closed by @renovate[bot] on 2024-04-08 21:09_

---

_Branch deleted on 2024-04-08 21:09_

---
