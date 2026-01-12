```yaml
number: 19793
title: "[ty] Repurpose `FunctionType.into_bound_method_type` to return `BoundMethodType`"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: into-callable-option
created_at: 2025-08-06T20:48:42Z
updated_at: 2025-11-06T11:48:13Z
url: https://github.com/astral-sh/ruff/pull/19793
synced_at: 2026-01-12T15:56:47Z
```

# [ty] Repurpose `FunctionType.into_bound_method_type` to return `BoundMethodType`

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

As per our naming scheme (at least for callable types) this should return a `BoundMethodType`, or be renamed, but it makes more sense to change the return type.

I also ensure `ClassType.into_callable` returns a `Type::Callable` in the changed branch.

Ideally we could return a `CallableType` from these `into_callable` functions (and rename to `into_callable_type` but because of unions we cannot do this.



---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-06 20:48_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-06 20:48_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-06 20:48_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-06 20:48_

---

_Comment by @github-actions[bot] on 2025-08-06 20:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-06 20:52_

---

_Comment by @github-actions[bot] on 2025-08-06 20:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-08-06 20:54_

Oh i didn't think that would have any effect

---

_Closed by @MatthewMckee4 on 2025-08-06 20:54_

---

_Reopened by @MatthewMckee4 on 2025-08-06 20:58_

---

_Renamed from "Remove `FunctionType.into_bound_method_type`" to "[ty] Repurpose `FunctionType.into_bound_method_type` to return `BoundMethodType`" by @MatthewMckee4 on 2025-08-06 20:59_

---

_@carljm approved on 2025-08-06 22:24_

---

_Merged by @carljm on 2025-08-06 22:24_

---

_Closed by @carljm on 2025-08-06 22:24_

---

_Branch deleted on 2025-11-06 11:48_

---
