```yaml
number: 15129
title: "[red-knot] Report classes inheriting from bases with incompatible `__slots__`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-slots
created_at: 2024-12-24T00:28:01Z
updated_at: 2024-12-27T14:01:53Z
url: https://github.com/astral-sh/ruff/pull/15129
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Report classes inheriting from bases with incompatible `__slots__`

---

_Pull request opened by @InSyncWithFoo on 2024-12-24 00:28_

## Summary

Resolves #14931.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-24 00:28_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-24 00:28_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-24 00:28_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-24 00:28_

---

_Comment by @github-actions[bot] on 2024-12-24 00:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-12-24 01:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:160 on 2024-12-24 17:34_

```suggestion
    /// Classes with no or empty `__slots__` are always compatible:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:173 on 2024-12-24 17:43_

This isn't strictly true. A class with non-empty `__slots__` can "participate in multiple inheritance" if all of the multiply-inherited bases go back to the same slots-defining class, e.g. in a diamond pattern like:

```py
class A:
    __slots__ = ("a", "b")

class B(A): ...
class C(A): ...

class D(A, B, C): ...  # fine
```

I think we need to add a test for this case, and fix it, because the current PR will emit a false positive diagnostic here, and I think it's important that we not emit false positives in this check. I think supporting this case will mean we can't just look at each immediate base and see if it has non-empty `__slots__` (including those inherited); we need to know which superclass the `__slots__` were explicitly defined on (Python runtime calls this the "solid base"), and the question is not "do we have 2+ immediate bases with `__slots__`" but rather "do we have multiple immediate bases with different 'solid base'?".

I would also re-word the phrasing here to instead say something like "Multiple inheritance from more than one different class defining non-empty `__slots__` is not allowed:"

---

_@carljm reviewed on 2024-12-24 19:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:335 on 2024-12-26 19:32_

```suggestion
    /// `__slots__` is not found in the class.
```

---

_@carljm approved on 2024-12-26 19:33_

LGTM (modulo comment) but will leave for @AlexWaygood review.

---

_@AlexWaygood approved on 2024-12-27 11:40_

Thanks! Nicely done

---

_Merged by @AlexWaygood on 2024-12-27 11:43_

---

_Closed by @AlexWaygood on 2024-12-27 11:43_

---

_Branch deleted on 2024-12-27 14:01_

---
