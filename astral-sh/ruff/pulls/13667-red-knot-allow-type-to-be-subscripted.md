```yaml
number: 13667
title: "[red-knot] Allow `type[]` to be subscripted"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-type-subscription
created_at: 2024-10-07T16:52:25Z
updated_at: 2024-10-07T18:43:49Z
url: https://github.com/astral-sh/ruff/pull/13667
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Allow `type[]` to be subscripted

---

_Pull request opened by @AlexWaygood on 2024-10-07 16:52_

Fixed a TODO by adding another TODO. It's the red-knot way!

## Summary

`builtins.type` can be subscripted at runtime on Python 3.9+, even though it has no `__class_getitem__` method and its metaclass (which is... itself) has no `__getitem__` method. The special case is [hardcoded directly into `PyObject_GetItem` in CPython](https://github.com/python/cpython/blob/744caa8ef42ab67c6aa20cd691e078721e72e22a/Objects/abstract.c#L181-L184). We just have to replicate the special case in our semantic model.

This will fail at runtime on Python <3.9. However, there's a bunch of outstanding questions (detailed in the TODO comment I added) regarding how we deal with subscriptions of other generic types on lower Python versions. Since we want to avoid too many false positives for now, I haven't tried to address this; I've just made `type` subscriptable on all Python versions.

## Test Plan

`cargo test -p red_knot_python_semantic --lib`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-07 16:52_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-07 16:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-07 16:52_

---

_Comment by @github-actions[bot] on 2024-10-07 17:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-10-07 18:35_

---

_Merged by @AlexWaygood on 2024-10-07 18:43_

---

_Closed by @AlexWaygood on 2024-10-07 18:43_

---

_Branch deleted on 2024-10-07 18:43_

---
