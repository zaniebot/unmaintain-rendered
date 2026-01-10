```yaml
number: 21609
title: Fix cargo shear in CI
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/cargo-shear
created_at: 2025-11-24T05:30:56Z
updated_at: 2025-11-24T05:35:35Z
url: https://github.com/astral-sh/ruff/pull/21609
synced_at: 2026-01-10T16:48:02Z
```

# Fix cargo shear in CI

---

_Pull request opened by @dhruvmanila on 2025-11-24 05:30_

Tested using `cargo build` and `cargo build --release`.

---

_Review requested from @carljm by @dhruvmanila on 2025-11-24 05:30_

---

_Label `internal` added by @dhruvmanila on 2025-11-24 05:30_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-11-24 05:30_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-11-24 05:30_

---

_Review requested from @dcreager by @dhruvmanila on 2025-11-24 05:30_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 05:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review request for @dcreager removed by @dhruvmanila on 2025-11-24 05:33_

---

_Review request for @carljm removed by @dhruvmanila on 2025-11-24 05:33_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2025-11-24 05:33_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-11-24 05:33_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 05:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @dhruvmanila on 2025-11-24 05:35_

---

_Closed by @dhruvmanila on 2025-11-24 05:35_

---

_Branch deleted on 2025-11-24 05:35_

---
