```yaml
number: 19193
title: "[ty] Return `CallableType` from `BoundMethodType.into_callable_type`"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: rename-bound-method-into-callable-type
created_at: 2025-07-07T23:55:59Z
updated_at: 2025-07-16T21:13:32Z
url: https://github.com/astral-sh/ruff/pull/19193
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Return `CallableType` from `BoundMethodType.into_callable_type`

---

_Pull request opened by @MatthewMckee4 on 2025-07-07 23:55_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Now that `FunctionType.into_callable_type` returns a CallableType, it seems like we should follow a convention that `into_callable` returns a `Type` (or `Option<Type>` or similar) and `into_callable_type` returns `CallableType`

Also updated `FunctionType.into_callable_type` docstring


---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-07 23:56_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-07 23:56_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-07 23:56_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-07 23:56_

---

_Renamed from "Rename BoundMethodType.into_callable_type to into_callable" to "[ty] Rename BoundMethodType.into_callable_type to into_callable" by @MatthewMckee4 on 2025-07-07 23:56_

---

_Comment by @github-actions[bot] on 2025-07-07 23:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dhruvmanila reviewed on 2025-07-08 04:10_

Would it be more beneficial to return a `CallableType` instead of `Type`? The caller can then either use `CallableType` however it wants or just wrap it in `Type` if that's what's required. Sorry, I haven't been following around this refactor too closely so feel free to ignore it not applicable.

---

_Label `internal` added by @dhruvmanila on 2025-07-08 04:10_

---

_Label `ty` added by @dhruvmanila on 2025-07-08 04:10_

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-08 08:51_

---

_Comment by @MatthewMckee4 on 2025-07-08 12:48_

This makes sense, will update later tonight.

---

_Renamed from "[ty] Rename BoundMethodType.into_callable_type to into_callable" to "[ty] Return `CallableType` from `BoundMethodType.into_callable_type`" by @AlexWaygood on 2025-07-08 19:33_

---

_@AlexWaygood approved on 2025-07-08 19:33_

---

_Merged by @AlexWaygood on 2025-07-08 19:33_

---

_Closed by @AlexWaygood on 2025-07-08 19:33_

---

_Branch deleted on 2025-07-16 21:13_

---
