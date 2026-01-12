```yaml
number: 21936
title: Use datatest for parser tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: micha/format-datatest
head: micha/parser-datatest
created_at: 2025-12-12T08:07:38Z
updated_at: 2025-12-12T17:21:48Z
url: https://github.com/astral-sh/ruff/pull/21936
synced_at: 2026-01-12T15:57:37Z
```

# Use datatest for parser tests

---

_@MichaReiser_

## Summary

Same as https://github.com/astral-sh/ruff/pull/21933 but for our parser tests.

[`datatest-stable`](https://github.com/nextest-rs/datatest-stable) uses a custom test-harness that mimics `cargo test` and supports `cargo nextest` to create a separate test for every file found in a given directory.
Unlike other test-macros, `datatest-stable` doesn't require re-compilation after adding or removing a test file (because it uses a custom test harness).

The main advantage of using it for the parser fixture tests is that each spec file now becomes its own test so that:

* Tests can run in parallel, resulting in faster wall-time
* You can run all tests even if there are a few failing tests
* You can selectively run tests




---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-12 08:07_

---

_Label `testing` added by @MichaReiser on 2025-12-12 08:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5136 diagnostics
+ Found 5137 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:26_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/fixtures.rs`:16 on 2025-12-12 14:22_

Did you want to move these down here?

---

_@ntBre approved on 2025-12-12 14:22_

---

_Merged by @MichaReiser on 2025-12-12 17:21_

---

_Closed by @MichaReiser on 2025-12-12 17:21_

---

_Branch deleted on 2025-12-12 17:21_

---
