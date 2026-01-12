```yaml
number: 18534
title: "[ty] Fix panic when trying to pull types for subscript expressions inside `Callable` type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/concatenate-crash
created_at: 2025-06-07T13:27:24Z
updated_at: 2025-06-11T10:48:37Z
url: https://github.com/astral-sh/ruff/pull/18534
synced_at: 2026-01-12T15:56:20Z
```

# [ty] Fix panic when trying to pull types for subscript expressions inside `Callable` type expressions

---

_@AlexWaygood_

## Summary

We weren't storing types for subscript expressions inside the parameter list of `Callable` type expressions; this was the cause of the corpus test failures for two files in our vendored copy of typeshed.

## Test Plan

- Removed the remaining typeshed entries from the `KNOWN_FAILURES` corpus list
- Added a standalone corpus test so that we still have coverage for this in case typeshed changes in the future
- Added an mdtest to cover the regression surfaced by primer on the first version of this PR


---

_Label `ty` added by @AlexWaygood on 2025-06-07 13:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-07 13:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-07 13:27_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-07 13:27_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-07 13:27_

---

_Comment by @github-actions[bot] on 2025-06-07 13:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Converted to draft by @AlexWaygood on 2025-06-07 13:37_

---

_Marked ready for review by @AlexWaygood on 2025-06-07 14:39_

---

_Comment by @AlexWaygood on 2025-06-07 14:40_

The new primer hits are because for this snippet:

```py
from basedtyping import P

reveal_type(P)
```

We infer the type of `P` as `ParamSpec | Unknown` rather than `ParamSpec`, which means that we don't see it as a valid first argument to `Callable`. This is a pre-existing issue (and something of an edge case), we just see it in more cases now because we're now recursing inside `Concatenate` slices when inferring types.

---

_Comment by @AlexWaygood on 2025-06-07 15:37_

With this PR, we also succeed in pulling types for the entirety of typeshed's `stubs/` directory as well as its `stdlib/` directory! Except for any files where the final segment is `types.pyi`: we still overflow our stack on all of those.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:9362 on 2025-06-09 09:22_

nit: let's avoid using "ty" as variables :)

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:9465 on 2025-06-09 09:24_

This is only being done for tuple expressions because otherwise it would already be present via the `self.infer_type_expression(argument)` call?

---

_@dhruvmanila approved on 2025-06-09 09:25_

Looks good, thanks!

---

_@AlexWaygood reviewed on 2025-06-09 10:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:9465 on 2025-06-09 10:15_

Yup, exactly!

---

_Merged by @AlexWaygood on 2025-06-09 10:26_

---

_Closed by @AlexWaygood on 2025-06-09 10:26_

---

_Branch deleted on 2025-06-09 10:26_

---

_Label `bug` added by @dhruvmanila on 2025-06-11 10:48_

---
