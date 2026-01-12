```yaml
number: 21987
title: Update MSRV to 1.90
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/rust-192
created_at: 2025-12-15T12:48:58Z
updated_at: 2025-12-15T13:29:14Z
url: https://github.com/astral-sh/ruff/pull/21987
synced_at: 2026-01-12T15:57:38Z
```

# Update MSRV to 1.90

---

_@MichaReiser_

## Summary

- Bump toolchain to 1.92
- Bump MSRV to Rust 1.90

## Test Plan

CI


---

_Review requested from @carljm by @MichaReiser on 2025-12-15 12:48_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-15 12:48_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-15 12:48_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-15 12:48_

---

_Label `release` added by @MichaReiser on 2025-12-15 12:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 12:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-15 12:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-15 13:13_


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

_@sharkdp approved on 2025-12-15 13:23_

---

_Merged by @MichaReiser on 2025-12-15 13:29_

---

_Closed by @MichaReiser on 2025-12-15 13:29_

---

_Branch deleted on 2025-12-15 13:29_

---
