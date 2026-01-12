```yaml
number: 15547
title: "[red-knot] `type[T]` is disjoint from `type[S]` if the metaclass of `T` is disjoint from the metaclass of `S`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/final-type-t
created_at: 2025-01-17T10:29:59Z
updated_at: 2025-01-17T18:32:11Z
url: https://github.com/astral-sh/ruff/pull/15547
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] `type[T]` is disjoint from `type[S]` if the metaclass of `T` is disjoint from the metaclass of `S`

---

_@AlexWaygood_

## Summary

This PR further extends the principles established in https://github.com/astral-sh/ruff/pull/15539 -- and allows us to get rid of even more special cases for `type[]` types in `Type::is_disjoint_from()`! A class definition `class A(B, C): ...` can only succeed if either the metaclass of `C` is a subclass of the metaclass of `B` or the metaclass of `B` is a subclass of the metaclass of `C`. As such, for two types `type[T]` and `type[S]`, we can conclude that the two types are disjoint if `T`'s metaclass and `S`'s metaclass are disjoint.

## Test Plan

- New mdtests added
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`

@sharkdp -- we don't have _great_ coverage for this kind of thing in our property tests, because I know of no standard-library classes that have `@final` metaclasses, so it's hard for me to think of how to get the property tests to use some `type[]` types that have disjoint metaclasses. Maybe that just means that this kind of thing is an edge case that we don't need to worry about too much, though 🤷

---

_Label `red-knot` added by @AlexWaygood on 2025-01-17 10:30_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-17 10:30_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-17 10:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-17 10:30_

---

_Merged by @AlexWaygood on 2025-01-17 10:41_

---

_Closed by @AlexWaygood on 2025-01-17 10:41_

---

_Branch deleted on 2025-01-17 10:41_

---

_Comment by @github-actions[bot] on 2025-01-17 10:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2025-01-17 11:06_

> @sharkdp -- we don't have _great_ coverage for this kind of thing in our property tests, because I know of no standard-library classes that have `@final` metaclasses, so it's hard for me to think of how to get the property tests to use some `type[]` types that have disjoint metaclasses. Maybe that just means that this kind of thing is an edge case that we don't need to worry about too much, though 🤷

Right. I think we might be able to write a custom file to the in-memory FS of the `TestDB` that would contain a couple of bespoke types that we could then import into the property tests? Might be a bit more difficult due to the `Ty` layer in between, though.

I've been thinking about changing the input to the property tests for another reason. We could add a layer like
```
pub enum PropertyTestType {
    Ty(Ty),
    CustomTypeWithFinalMetaclass,
    # …
}
```
and use that as the input to the property tests. That would have the added benefit that we could write a custom `Display` (or `Debug`?) implementation for this in order to get a better output when a property test fails. Instead of showing the `Ty` representation, we could potentially render the nicer `Type` `Display` output that would also contain initial simplifications in case of union types. So instead of showing `Ty::union([int,str, Literal[1]])`, we could show `int | str`.

---

_@carljm reviewed on 2025-01-17 17:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/subclass_of.rs`:75 on 2025-01-17 17:54_

I think this line is OK as-is, as far as this method is used in this PR, because our semantics of non-fully-static types in `is_disjoint_from` is that we treat them as equivalent to their widest possible type (that is, `Any` and `object` are treated the same by `is_disjoint_from`, in being disjoint from nothing.) But if this method were to be used in other contexts in future, we need to be a little careful about this line, it may not be the right thing; we should maybe return `Dynamic` instead. 

---

_@AlexWaygood reviewed on 2025-01-17 18:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/subclass_of.rs`:75 on 2025-01-17 18:21_

Hmm, good point. In my first version of this PR I used this method from multiple places (which was why I created at method)... but it looks like now it's only used in a single location... So maybe I should just inline it at that single location (like it was prior to this PR)?

---

_@carljm reviewed on 2025-01-17 18:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/subclass_of.rs`:75 on 2025-01-17 18:32_

Yeah, that might be better, in that it would reduce the risk of reusing this logic where this isn't the right handling of it.

---
