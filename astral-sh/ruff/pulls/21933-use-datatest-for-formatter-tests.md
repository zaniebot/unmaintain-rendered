```yaml
number: 21933
title: Use datatest for formatter tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/format-datatest
created_at: 2025-12-12T07:41:19Z
updated_at: 2025-12-13T08:02:24Z
url: https://github.com/astral-sh/ruff/pull/21933
synced_at: 2026-01-10T16:42:11Z
```

# Use datatest for formatter tests

---

_Pull request opened by @MichaReiser on 2025-12-12 07:41_

## Summary

[`datatest-stable`](https://github.com/nextest-rs/datatest-stable) is a custom test harness for data driven tests. 

It uses a custom test-harness that mimics `cargo test` and supports `cargo nextest` to create a separate test for every file found in a given directory.
Unlike other test-macros, `datatest-stable` doesn't require re-compilation after changing a test file (because it uses a custom test harness).

The main advantage of using it for formatter tests is that each formatter spec file is now its own test so that:

* Tests can run in parallel, resulting in faster wall-time
* You can run all tests even if there are a few failing tests
* You can selectively run tests

## Test Plan

I made changes to a test file and it picked up the changes without needing recompilation


---

_Label `testing` added by @MichaReiser on 2025-12-12 07:41_

---

_Marked ready for review by @MichaReiser on 2025-12-12 07:42_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 07:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 07:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5136 diagnostics
+ Found 5137 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:00_


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

_@ntBre approved on 2025-12-12 14:16_

Nice, this sounds awesome!

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-12 17:21_

---

_Merged by @MichaReiser on 2025-12-13 08:02_

---

_Closed by @MichaReiser on 2025-12-13 08:02_

---

_Branch deleted on 2025-12-13 08:02_

---
