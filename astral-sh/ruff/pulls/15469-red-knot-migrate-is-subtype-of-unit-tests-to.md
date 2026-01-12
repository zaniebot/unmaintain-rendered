```yaml
number: 15469
title: "[red-knot] Migrate `is_subtype_of` unit tests to Markdown tests"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: rk-pt-is-subtype-of
created_at: 2025-01-14T09:34:40Z
updated_at: 2025-01-14T15:14:32Z
url: https://github.com/astral-sh/ruff/pull/15469
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Migrate `is_subtype_of` unit tests to Markdown tests

---

_@InSyncWithFoo_

## Summary

Part of #15397.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-14 09:34_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-14 09:34_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-14 09:34_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-14 09:34_

---

_Renamed from "Migrate `is_subtype_of` unit tests to Markdown tests" to "[red-knot] Migrate `is_subtype_of` unit tests to Markdown tests" by @InSyncWithFoo on 2025-01-14 09:34_

---

_Comment by @github-actions[bot] on 2025-01-14 09:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @AlexWaygood on 2025-01-14 09:41_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-14 09:41_

---

_@sharkdp reviewed on 2025-01-14 12:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:86 on 2025-01-14 12:34_

I guess you want to use a `Todo` type using `tuple[int, ...]` here? I think we should rather replace `tuple[int, ...]` with `Any` here.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:231 on 2025-01-14 12:37_

I would trust that `Intersection` works as expected here and go with just:

```suggestion
class A: ...
class B: ...

static_assert(not is_subtype_of(A, B))
static_assert(is_subtype_of(Intersection[A, B], A))
static_assert(is_subtype_of(Intersection[A, B], B))
```

In the Rust implementation, we used `a` and `b` to get access to the types `A` and `B` (and not just the class literal types `TypeOf[A]` and `TypeOf[B]`), but we don't need that here.

---

_@sharkdp reviewed on 2025-01-14 12:37_

---

_@sharkdp reviewed on 2025-01-14 12:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:142 on 2025-01-14 12:45_

This comment by @AlexWaygood probably also applies here: https://github.com/astral-sh/ruff/pull/15353#discussion_r1907236295. We could introduce something like
```
class Meta(type): ...
class HasCustomMetaClass(metaclass=Meta): ...
```
and then use those instead of `ABCMeta`, `ABC`.

---

_@sharkdp reviewed on 2025-01-14 12:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:107 on 2025-01-14 12:47_

`Intersection[Not[T]]` is equivalent to just `Not[T]`:

```suggestion
static_assert(is_subtype_of(Intersection[int, Not[Literal[2]]], Not[Literal[2]]))
```

The same simplification applies to a few other tests below as well.

---

_@sharkdp reviewed on 2025-01-14 12:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:185 on 2025-01-14 12:48_

Minor: If we use two files anyway, I think I'd prefer two subsections `### Unions` and `### Intersections`.

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-14 12:48_

---

_@sharkdp reviewed on 2025-01-14 12:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:196 on 2025-01-14 12:51_

Maybe we could use a type alias here to make this a bit easier to read?

```suggestion
type LiteralBase = TypeOf[Base]
type LiteralDerived = TypeOf[Derived]

assert_type(Base, LiteralBase)
assert_type(Derived, LiteralDerived)

static_assert(is_subtype_of(LiteralBase, type))
static_assert(is_subtype_of(LiteralDerived, object))

# and similar below …
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:211 on 2025-01-14 12:53_

Using the type aliases above, this could probably also be written without a `flag` helper?

```suggestion
static_assert(is_subtype_of(TypeOf[LiteralBase | LiteralUnrelated], type))
static_assert(is_subtype_of(TypeOf[LiteralBase | LiteralUnrelated], object))
```

---

_@sharkdp reviewed on 2025-01-14 12:53_

---

_@sharkdp reviewed on 2025-01-14 12:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:28 on 2025-01-14 12:54_

Maybe we can move those tests involving `Any` or `Unknown` into a new subsection titled "Non fully-static types do not participate in subtyping"?

---

_@sharkdp reviewed on 2025-01-14 12:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:46 on 2025-01-14 12:55_

Maybe

```suggestion
## `Never` is a subtype of every type
```

and then move other tests with `Never` on the LHS into this section?

---

_@sharkdp reviewed on 2025-01-14 12:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:46 on 2025-01-14 12:56_

Similarly, we could have a "`object` is a supertype of every type" section, and then move all tests with `object` on the RHS into that section?

---

_@sharkdp approved on 2025-01-14 12:59_

Thank you very much!

I think we can probably extend these tests and potentially organize them a little better now that it's easier to do so, but this does not (should not?) happen in this PR. I added a couple of review comments, but I'm also happy to merge this as a "pure refactoring" PR and incorporate these improvements myself as a follow up. Let me know what you think.

---

_@InSyncWithFoo reviewed on 2025-01-14 13:24_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:211 on 2025-01-14 13:24_

(I think you mean `is_subtype_of(LiteralBase | LiteralUnrelated, type)` without `TypeOf`.) Done.

---

_Comment by @InSyncWithFoo on 2025-01-14 13:25_

@sharkdp Please go right ahead! It would be rather unproductive for me to perform such organizational changes.

---

_@sharkdp approved on 2025-01-14 14:57_

Thank you for the updates. I made one minor change. I think this is an accurate representation of the tests we had before. If someone wants to make improvements to this test suite, we can always do them on top of this change after it's merged.

---

_Merged by @sharkdp on 2025-01-14 14:57_

---

_Closed by @sharkdp on 2025-01-14 14:57_

---

_Branch deleted on 2025-01-14 15:14_

---
