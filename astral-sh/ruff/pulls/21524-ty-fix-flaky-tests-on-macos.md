```yaml
number: 21524
title: "[ty] Fix flaky tests on macos"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/flaky-constraints
created_at: 2025-11-19T14:00:04Z
updated_at: 2025-11-19T14:44:34Z
url: https://github.com/astral-sh/ruff/pull/21524
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix flaky tests on macos

---

_Pull request opened by @dcreager on 2025-11-19 14:00_

We're seeing flaky test failures on macos, which seems to be caused by different Salsa ID orderings on the different platforms. Constraint set BDDs order their internal nodes based on the Salsa IDs of the interned typevar structs, and we had some code that depended on variable ordering in an unexpected way.

This patch definitely fixes the macos test failure on #21414, and hopefully fixes it on #21436, too.

---

_Review requested from @carljm by @dcreager on 2025-11-19 14:00_

---

_Label `internal` added by @dcreager on 2025-11-19 14:00_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-19 14:00_

---

_Review requested from @sharkdp by @dcreager on 2025-11-19 14:00_

---

_Label `ty` added by @dcreager on 2025-11-19 14:00_

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 14:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-19 14:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @MichaReiser on 2025-11-19 14:28_

I got really excited when I saw your PR title because I thought you fixed the flaky file watching tests... This is still great :)

---

_@MichaReiser approved on 2025-11-19 14:29_

---

_Merged by @dcreager on 2025-11-19 14:44_

---

_Closed by @dcreager on 2025-11-19 14:44_

---

_Branch deleted on 2025-11-19 14:44_

---
