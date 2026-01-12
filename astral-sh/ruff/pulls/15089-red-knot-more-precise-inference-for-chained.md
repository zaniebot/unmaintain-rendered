```yaml
number: 15089
title: "[red-knot] More precise inference for chained boolean expressions"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-chained-booleans
created_at: 2024-12-21T00:07:34Z
updated_at: 2024-12-22T19:02:34Z
url: https://github.com/astral-sh/ruff/pull/15089
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] More precise inference for chained boolean expressions

---

_@InSyncWithFoo_

## Summary

Resolves #13632.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-21 00:07_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-21 00:07_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-21 00:07_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-21 00:07_

---

_Comment by @github-actions[bot] on 2024-12-21 00:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-12-21 13:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:223 on 2024-12-22 16:01_

The term we use for this is "narrowing", not "filtering"
```suggestion
## Narrowing in chained boolean expressions
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:237 on 2024-12-22 16:04_

This is not part of the requirements in the linked task. For the scope of this task, we should simply intersect with `~AlwaysTruthy` or `~AlwaysFalsy` and let the resulting types shake out.

If we feel we need to simplify display of some types to avoid user confusion, we may do that in future, but we want to evaluate that holistically in the context of real-world code, not pre-emptively. And we will implement that in a general way (either in type display or in intersection building), not special-cased to a single narrowing scenario.
```suggestion
def _(x: str):
    reveal_type(x or A())  # revealed: str & ~AlwaysFalsy | A
    reveal_type(x and A())  # revealed: str & ~AlwaysTruthy | A
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:266 on 2024-12-22 16:07_

FWIW, in case you're interested, I don't think this should be a difficult TODO to address (and I think it will also fix some other TODOs in the tests.) It just requires handling `ClassLiteral` differently in `Type::bool`. (The TODO comment there says we should look up "`__bool__` and `__len__`" methods on the metaclass, but this is wrong, we've since realized we can't rely on `__len__` for anything, as explained below under the Instance case.)

In fact I think the simplest implementation might be to treat the `ClassLiteral` type as instead an `Instance` type of its metaclass, and just recursively defer to the `Instance` case.

Probably best to do this in a separate PR though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3598 on 2024-12-22 16:08_

We shouldn't be doing all this special-casing of specific types here. As described in the issue, all we need to do here is intersect with `~AlwaysFalsy` or `~AlwaysTruthy`; `IntersectionBuilder` and our type-relation methods should determine what happens from there.

---

_@carljm reviewed on 2024-12-22 16:09_

Thanks! A few comments.

---

_@carljm approved on 2024-12-22 18:01_

Looks great, thank you!

---

_Merged by @carljm on 2024-12-22 18:02_

---

_Closed by @carljm on 2024-12-22 18:02_

---

_Branch deleted on 2024-12-22 19:02_

---
