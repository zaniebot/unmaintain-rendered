```yaml
number: 19743
title: "[ty] fix lookup order of class variables before they are defined"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-875-class-var-lookup
created_at: 2025-08-04T12:32:04Z
updated_at: 2025-08-07T03:09:59Z
url: https://github.com/astral-sh/ruff/pull/19743
synced_at: 2026-01-10T17:52:17Z
```

# [ty] fix lookup order of class variables before they are defined

---

_Pull request opened by @mtshiba on 2025-08-04 12:32_

## Summary

This is a follow-up to #19321.

If we try to access a class variable before it is defined, the variable is looked up in the global scope, rather than in any enclosing scopes.

Closes https://github.com/astral-sh/ty/issues/875.

## Test Plan

New tests in `narrow/conditionals/nested.md`.


---

_Review requested from @carljm by @mtshiba on 2025-08-04 12:32_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-08-04 12:32_

---

_Review requested from @sharkdp by @mtshiba on 2025-08-04 12:32_

---

_Review requested from @dcreager by @mtshiba on 2025-08-04 12:32_

---

_Comment by @github-actions[bot] on 2025-08-04 12:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-04 12:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/pytester.py:1176:27: error[unresolved-reference] Name `ret` used when not defined
- Found 495 diagnostics
+ Found 496 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-04 13:08_

---

_@carljm approved on 2025-08-05 03:21_

Looks good, thank you!

---

_Comment by @carljm on 2025-08-05 03:21_

The ecosystem hit looks like a true positive to me.

---

_Merged by @carljm on 2025-08-05 03:21_

---

_Closed by @carljm on 2025-08-05 03:21_

---

_Branch deleted on 2025-08-07 03:09_

---
