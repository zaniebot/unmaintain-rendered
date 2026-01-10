```yaml
number: 20524
title: "[ty] implement `auto()` for `StrEnum`"
type: pull_request
state: merged
author: thejchap
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: thejchap/enum-auto
created_at: 2025-09-22T22:16:56Z
updated_at: 2025-09-23T10:22:59Z
url: https://github.com/astral-sh/ruff/pull/20524
synced_at: 2026-01-10T17:40:28Z
```

# [ty] implement `auto()` for `StrEnum`

---

_Pull request opened by @thejchap on 2025-09-22 22:16_

## Summary
see discussion here: https://github.com/astral-sh/ty/issues/876#issuecomment-3310130167

https://docs.python.org/3/library/enum.html#enum.StrEnum

> Note Using [auto](https://docs.python.org/3/library/enum.html#enum.auto) with [StrEnum](https://docs.python.org/3/library/enum.html#enum.StrEnum) results in the lower-cased member name as the value.

## Test Plan
- new mdtest
- also, added a test to assert the (already correct) behavior for `IntEnum`

---

_Comment by @github-actions[bot] on 2025-09-22 22:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-22 22:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @thejchap on 2025-09-23 00:13_

---

_Review requested from @carljm by @thejchap on 2025-09-23 00:13_

---

_Review requested from @AlexWaygood by @thejchap on 2025-09-23 00:13_

---

_Review requested from @sharkdp by @thejchap on 2025-09-23 00:13_

---

_Review requested from @dcreager by @thejchap on 2025-09-23 00:13_

---

_Label `ty` added by @MichaReiser on 2025-09-23 08:06_

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-09-23 08:06_

---

_@sharkdp approved on 2025-09-23 10:08_

This is great — thank you very much!

---

_Label `bug` added by @sharkdp on 2025-09-23 10:14_

---

_Merged by @sharkdp on 2025-09-23 10:22_

---

_Closed by @sharkdp on 2025-09-23 10:22_

---
