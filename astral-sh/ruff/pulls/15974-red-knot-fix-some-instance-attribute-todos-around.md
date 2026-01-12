```yaml
number: 15974
title: "[red-knot] Fix some instance-attribute TODOs around `ModuleType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/instance-attrs-todos
created_at: 2025-02-05T15:29:23Z
updated_at: 2025-02-05T15:33:39Z
url: https://github.com/astral-sh/ruff/pull/15974
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Fix some instance-attribute TODOs around `ModuleType`

---

_@AlexWaygood_

## Summary

We understand (some) instance attributes now, thanks to @sharkdp's work, so these TODOs can now be done 🎉

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-05 15:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-05 15:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-05 15:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-05 15:29_

---

_@AlexWaygood reviewed on 2025-02-05 15:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:62 on 2025-02-05 15:30_

these look like regressions but they are actually improvements. accessing `typing.__init__` does not actually return the original `__init__` function in `types.ModuleType`'s class body; it returns a bound method. The difference is that you don't need to pass `self` in explicitly when calling the method.

---

_Merged by @AlexWaygood on 2025-02-05 15:33_

---

_Closed by @AlexWaygood on 2025-02-05 15:33_

---

_Branch deleted on 2025-02-05 15:33_

---
