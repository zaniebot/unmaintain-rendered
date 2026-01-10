```yaml
number: 20485
title: "[ty] Add support for inlay hints on attribute assignment"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: inlay-hint-attribute-type-hint
created_at: 2025-09-19T16:06:47Z
updated_at: 2025-11-06T11:48:25Z
url: https://github.com/astral-sh/ruff/pull/20485
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Add support for inlay hints on attribute assignment

---

_Pull request opened by @MatthewMckee4 on 2025-09-19 16:06_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

I think this is especially useful for inside class methods for example.

## Test Plan

Add test to show initial assignment of instance variable via `self` and assignment after instantiation of a class


---

_Comment by @github-actions[bot] on 2025-09-19 16:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @MatthewMckee4 on 2025-09-19 16:18_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-09-19 16:18_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-09-19 16:18_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-09-19 16:18_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-09-19 16:18_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-09-19 16:18_

---

_Comment by @github-actions[bot] on 2025-09-19 16:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-09-19 17:06_

---

_Label `ty` added by @AlexWaygood on 2025-09-19 17:06_

---

_Renamed from "Add support for inlay hints on attribute assignment" to "[ty] Add support for inlay hints on attribute assignment" by @AlexWaygood on 2025-09-19 17:06_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-09-19 17:06_

---

_Review request for @carljm removed by @carljm on 2025-09-22 22:20_

---

_@MichaReiser approved on 2025-09-23 11:14_

Thank you

---

_Merged by @MichaReiser on 2025-09-23 11:14_

---

_Closed by @MichaReiser on 2025-09-23 11:14_

---

_Branch deleted on 2025-11-06 11:48_

---
