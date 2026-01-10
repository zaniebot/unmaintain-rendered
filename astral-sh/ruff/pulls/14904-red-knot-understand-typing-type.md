```yaml
number: 14904
title: "[red-knot] Understand `typing.Type`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-typing-type
created_at: 2024-12-11T02:18:06Z
updated_at: 2024-12-11T16:27:34Z
url: https://github.com/astral-sh/ruff/pull/14904
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Understand `typing.Type`

---

_Pull request opened by @InSyncWithFoo on 2024-12-11 02:18_

## Summary

Resolves #14891.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-11 02:18_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-11 02:18_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-11 02:18_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-11 02:18_

---

_Comment by @github-actions[bot] on 2024-12-11 02:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2024-12-11 02:25_

This has one small problem: Inheriting from `Type` results in an MRO with both `type` and `Generic`, but `Mro::of_class_impl()` expects only one returned value from `ClassBase::try_from_ty()`. Fixing this would require quite a few significant changes here and there; should I try to do so, or should it be left as a TODO for now?

---

_Label `red-knot` added by @MichaReiser on 2024-12-11 07:07_

---

_Comment by @AlexWaygood on 2024-12-11 10:40_

> This has one small problem: Inheriting from `Type` results in an MRO with both `type` and `Generic`, but `Mro::of_class_impl()` expects only one returned value from `ClassBase::try_from_ty()`. Fixing this would require quite a few significant changes here and there; should I try to do so, or should it be left as a TODO for now?

Fine to just TODO all that for now. We don't even understand that `typing.Generic` is a class at all right now!

---

_@AlexWaygood approved on 2024-12-11 10:57_

Thanks!

---

_Merged by @AlexWaygood on 2024-12-11 11:01_

---

_Closed by @AlexWaygood on 2024-12-11 11:01_

---

_Branch deleted on 2024-12-11 12:54_

---

_@carljm reviewed on 2024-12-11 16:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/typing_dot_Type.md`:12 on 2024-12-11 16:15_

Why does the `e` argument exist here?

---

_Comment by @carljm on 2024-12-11 16:17_

Nice, thank you!

---

_@InSyncWithFoo reviewed on 2024-12-11 16:22_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/type_of/typing_dot_Type.md`:12 on 2024-12-11 16:22_

It wasn't there [originally](https://github.com/astral-sh/ruff/pull/14904/commits/3dad4a87720e9e8369de71a04ef2b4c960196af1#diff-f5d0291e29eefe51ba224c6504cb8ad2879a2f20e961c0dcbc50e82bf4be7a78R12-R22). Probably a small oversight.

---

_@AlexWaygood reviewed on 2024-12-11 16:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_of/typing_dot_Type.md`:12 on 2024-12-11 16:27_

Sorry, I think I screwed that up when simplifying the mdtests a little :-)

---
