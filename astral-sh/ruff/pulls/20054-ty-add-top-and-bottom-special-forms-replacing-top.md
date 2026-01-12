```yaml
number: 20054
title: "[ty] Add Top[] and Bottom[] special forms, replacing top_materialization_of() function"
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - ty
assignees: []
merged: true
base: main
head: top-special-form
created_at: 2025-08-23T01:06:00Z
updated_at: 2025-08-24T12:54:34Z
url: https://github.com/astral-sh/ruff/pull/20054
synced_at: 2026-01-12T15:56:53Z
```

# [ty] Add Top[] and Bottom[] special forms, replacing top_materialization_of() function

---

_@JelleZijlstra_

Part of astral-sh/ty#994

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add new special forms to `ty_extensions`, `Top[T]` and `Bottom[T]`. Remove `ty_extensions.top_materialization` and `ty_extensions.bottom_materialization`.

## Test Plan

Converted the existing `materialization.md` mdtest to the new syntax. Added some tests for invalid use of the new special form.

---

_Comment by @github-actions[bot] on 2025-08-23 01:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @JelleZijlstra on 2025-08-23 01:09_

---

_Review requested from @carljm by @JelleZijlstra on 2025-08-23 01:09_

---

_Review requested from @AlexWaygood by @JelleZijlstra on 2025-08-23 01:09_

---

_Review requested from @sharkdp by @JelleZijlstra on 2025-08-23 01:09_

---

_Review requested from @dcreager by @JelleZijlstra on 2025-08-23 01:09_

---

_Review requested from @MichaReiser by @JelleZijlstra on 2025-08-23 01:09_

---

_Comment by @github-actions[bot] on 2025-08-23 01:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-23 07:07_

---

_@carljm approved on 2025-08-23 18:20_

🚀 

---

_Merged by @carljm on 2025-08-23 18:20_

---

_Closed by @carljm on 2025-08-23 18:20_

---

_Branch deleted on 2025-08-24 12:54_

---
