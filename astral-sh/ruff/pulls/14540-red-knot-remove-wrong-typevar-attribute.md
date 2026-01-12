```yaml
number: 14540
title: "[red-knot] remove wrong typevar attribute implementations"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/remove
created_at: 2024-11-22T20:30:32Z
updated_at: 2024-11-22T21:17:18Z
url: https://github.com/astral-sh/ruff/pull/14540
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] remove wrong typevar attribute implementations

---

_@carljm_

## Summary

In #14182 I added "precise" per-TypeVar type inference for attributes of a typevar (`__bound__`, `__constraints__`, `__default__`). This wasn't motivated so much by the real-world need for this precise inference, as it was by wanting to test the correctness of our internal representation of the typevar, via an mdtest.

The implementation I added was unfortunately just wrong. The problem is that our internal representation cares about the bound/constraints/default inferred as a type expression, while the runtime attributes of the `TypeVar` object will return it as a value expression.

In the simplest case, this means that for a typevar `[T: A]`, while we might store the bound as the Instance type `A`, the value of the `__bound__` attribute should be typed as `type[A]`.

Beguiled by the simple case, I slapped on a `.to_meta_type()` and called it a day. But this is just wrong in more complex cases; the meta-type is not always the same thing as the "type form" type. For example, the meta-type of `A | B` is `type[A] | type[B]`, but the type-form type in this case would be an instance of `types.UnionType`.

In a future world with PEP 747 (`TypeForm`), where we have a typevar with a bound of `A | B` it would be correct for us to infer its `__bound__` attribute as `TypeForm[A | B]`, offering a nicer solution to this problem.

In the meantime, I think we should just follow the lead of other type checkers in falling back to the general typeshed types for these attributes, and I should bite the bullet and write a Rust test instead of an mdtest when I want to test internal representations of types that don't (yet, since we haven't implemented generics) have a visible effect in the type system.

## Test Plan

Re-wrote an mdtest as a Rust test.

---

_Review requested from @MichaReiser by @carljm on 2024-11-22 20:30_

---

_Review requested from @AlexWaygood by @carljm on 2024-11-22 20:30_

---

_Review requested from @sharkdp by @carljm on 2024-11-22 20:30_

---

_Label `red-knot` added by @carljm on 2024-11-22 20:30_

---

_Comment by @github-actions[bot] on 2024-11-22 20:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-11-22 21:15_

---

_Merged by @carljm on 2024-11-22 21:17_

---

_Closed by @carljm on 2024-11-22 21:17_

---

_Branch deleted on 2024-11-22 21:17_

---
