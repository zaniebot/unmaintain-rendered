```yaml
number: 11970
title: "[red-knot] Add more tests asserting that the VendoredFileSystem and the `VERSIONS` parser work with the vendored typeshed stubs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshed-validation
created_at: 2024-06-21T14:48:15Z
updated_at: 2024-06-21T17:08:56Z
url: https://github.com/astral-sh/ruff/pull/11970
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Add more tests asserting that the VendoredFileSystem and the `VERSIONS` parser work with the vendored typeshed stubs

---

_Pull request opened by @AlexWaygood on 2024-06-21 14:48_

- Assert that all modules found in the `vendor/typeshed` directory can be retrieved from the zipfile using the `VendoredFileSystem` struct.
- Assert that all top-level modules found in `vendor/typeshed` are included in `VERSIONS`


---

_Label `red-knot` added by @AlexWaygood on 2024-06-21 14:48_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-21 14:48_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-21 14:48_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:40 on 2024-06-21 15:41_

We could now consider implementing `Default` where the default implementation uses the "vendored" files

---

_@MichaReiser reviewed on 2024-06-21 15:41_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:86 on 2024-06-21 15:42_

Nit: I think `Path` implements `PartialEq<str>`
```suggestion
            if relative_path == "VERSIONS" {
```

---

_@AlexWaygood reviewed on 2024-06-21 15:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed.rs`:40 on 2024-06-21 15:44_

I think that would mean that `ruff_db` would have to depend on this crate, and I'd rather have it the other way around

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:68 on 2024-06-21 15:46_

I don't mind the integration tests, but they are a bit hard to understand. For example, I'm still not a 100% if I understand the second test and I would feel terrified if the test starts failing. 

What's nice about the test is that it is exhaustive, but the exhaustiveness adds a lot of complexity (and file iteration isn't guaranteed to always run in the same order). 

I would probably take a shorter route and simply assert that a few selected files exist. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:73 on 2024-06-21 15:46_

Nit: I would consider moving the tests closer to the implementation they're testing. 

---

_@MichaReiser approved on 2024-06-21 15:46_

LGTM: I would consider simplifying the tests. I don't think we need full exhaustiveness tests. Testing a few samples should be sufficient.

---

_@carljm approved on 2024-06-21 15:47_

---

_@AlexWaygood reviewed on 2024-06-21 15:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed.rs`:86 on 2024-06-21 15:51_

Compiler says no:

```
error[E0277]: can't compare `std::path::Path` with `str`
  --> crates/red_knot_module_resolver/src/typeshed.rs:86:30
   |
86 |             if relative_path == "VERSIONS" {
   |                              ^^ no implementation for `std::path::Path == str`
   |
   = help: the trait `std::cmp::PartialEq<str>` is not implemented for `std::path::Path`, which is required by `&std::path::Path: std::cmp::PartialEq<&str>`
   = help: the following other types implement trait `std::cmp::PartialEq<Rhs>`:
             <&'a std::path::Path as std::cmp::PartialEq<camino::Utf8Path>>
             <&'a std::path::Path as std::cmp::PartialEq<camino::Utf8PathBuf>>
             <&'a std::path::Path as std::cmp::PartialEq<std::borrow::Cow<'b, std::ffi::OsStr>>>
             <&'a std::path::Path as std::cmp::PartialEq<std::ffi::OsStr>>
             <&'a std::path::Path as std::cmp::PartialEq<std::ffi::OsString>>
             <&'a std::path::Path as std::cmp::PartialEq<std::path::PathBuf>>
             <&'b std::path::Path as std::cmp::PartialEq<std::borrow::Cow<'a, std::path::Path>>>
             <std::path::Path as std::cmp::PartialEq<&'a camino::Utf8Path>>
           and 9 others
   = note: required for `&std::path::Path` to implement `std::cmp::PartialEq<&str>`
```

---

_Comment by @github-actions[bot] on 2024-06-21 16:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2024-06-21 16:13_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:40 on 2024-06-21 16:13_

ah right that's true. 

---

_@AlexWaygood reviewed on 2024-06-21 16:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed.rs`:68 on 2024-06-21 16:31_

Thanks. I would much rather be exhaustive here, I think. I've gone through and added custom error messages to each `unwrap()` and `assert()` call, so that it's clear exactly which invariant is being asserted at each stage, and so that it should be less terrifying to debug a test if one of the assertions starts failing.

---

_Merged by @AlexWaygood on 2024-06-21 16:53_

---

_Closed by @AlexWaygood on 2024-06-21 16:53_

---

_Branch deleted on 2024-06-21 16:53_

---

_Comment by @codspeed-hq[bot] on 2024-06-21 16:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeshed-validation)

### Merging #11970 will **improve performances by 5.07%**

<sub>Comparing <code>typeshed-validation</code> (613a9bb) with <code>typeshed-validation</code> (8b3f05f)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `typeshed-validation` | `typeshed-validation` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +5.07% |


---
