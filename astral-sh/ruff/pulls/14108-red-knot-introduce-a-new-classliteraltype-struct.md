```yaml
number: 14108
title: "[red-knot] Introduce a new `ClassLiteralType` struct"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/class-literal-type
created_at: 2024-11-05T13:43:09Z
updated_at: 2024-11-05T22:25:48Z
url: https://github.com/astral-sh/ruff/pull/14108
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Introduce a new `ClassLiteralType` struct

---

_@AlexWaygood_

## Summary

The struct on `main` called `ClassType` does not actually represent a type. It represents inner data for a type -- but exactly how the data should be interpreted depends on what kind of type is wrapping the inner data. `Type::ClassLiteral()` and `Type::Instance()` both wrap the struct-currently-known-as-ClassType, but they're very different kinds of types: the first is a singleton type representing a single class object at runtime, whereas the second represents a type of unknown size representing all (possible) instances of a single class object at runtime.

To clarify the situation here, this PR renames the class-currently-known-as-ClassType to `Class`. This reflects better the fact that it represents data representing a class object at runtime, but does not in itself represent a type. In the refactor that this PR proposes, we now have two `*Type` structs that wrap `Class`: `InstanceType` and `ClassLiteralType`. The `Type::ClassLiteral` enum variant wraps `ClassLiteralType` instead of wrapping `Class` directly.

This refactor allows us to pass around objects that unambiguously represent specific types without having to wrap the objects in variants the `Type` enum. In particular, the signatures of `perform_rich_comparison` and `perform_membership_test_comparison` become much more expressive and type-safe.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-05 13:43_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-05 13:43_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-05 13:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-05 13:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2098 on 2024-11-05 13:46_

I'm glad I tried to writeup some docs here, as it made me realise that `InstanceType` types can now represent two pretty different things, depending on whether the `known` field is `Some()` or `None`. I'm not sure that's really desirable; we might consider having a separate `Type::KnownInstance` variant on `Type`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3779 on 2024-11-05 13:48_

I had to split up `infer_type_expression` into two methods, `infer_type_expression` and `infer_type_expression_impl`, or `cargo fmt` insisted on increasing the indentation of the huge `match` statement in `infer_type_expression` with my changes here. I didn't want a huge diff in this PR, and I also didn't think the extra indentation was desirable in and of itself, so I split the method into two.

---

_@AlexWaygood reviewed on 2024-11-05 13:48_

---

_@AlexWaygood reviewed on 2024-11-05 13:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:312 on 2024-11-05 13:50_

I moved this down a little bit so that the three dynamic variants (`Any`, `Unknown` and `Todo`) were together at the top.

---

_@AlexWaygood reviewed on 2024-11-05 13:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3779 on 2024-11-05 13:55_

Oh, and now I see @sharkdp does a very similar thing in https://github.com/astral-sh/ruff/pull/14106/files#r1829286623. Let's merge his PR first; I can rebase this on top.

---

_Comment by @github-actions[bot] on 2024-11-05 13:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-11-05 14:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:365 on 2024-11-05 14:01_

Maybe name this consistently (similar for module/function) to match the names of `expect_class_literal` and `is_class_literal`?
```suggestion
    pub const fn into_class_literal(self) -> Option<ClassLiteralType<'db>> {
```
Or the other way around (add `_type` suffix consistently)?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2121 on 2024-11-05 14:07_

Minor: the name `to_…` might suggest that instance types are more or less equivalent to class literal types, since they can be converted so easily(?). Maybe something like
```suggestion
    pub fn corresponding_class_literal_type(self) -> ClassLiteralType<'db> {
```

---

_@sharkdp reviewed on 2024-11-05 14:07_

---

_@AlexWaygood reviewed on 2024-11-05 14:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:365 on 2024-11-05 14:10_

Removed all `*_type` suffixes in https://github.com/astral-sh/ruff/pull/14108/commits/a97d183e7cb5a458f288e7ce53bd4b54e8ebb329 👍

---

_@sharkdp approved on 2024-11-05 14:16_

Thank you, this looks good to me. It's slightly more verbose now in some places, but more consistent, safe and less verbose in places where it matters.

---

_@AlexWaygood reviewed on 2024-11-05 14:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2121 on 2024-11-05 14:17_

I was very unsure whether this method and `ClassLiteralType::to_instance_type` were more helpful than they were confusing. Your comment here convinces me that they're on the "more confusing" side of the line 😆

I removed them in https://github.com/astral-sh/ruff/pull/14108/commits/22d7a3b0802f31f591fff52e31f7b9f44a3b2841, in favour of a `Type::anonymous_instance` helper function.

---

_@MichaReiser approved on 2024-11-05 16:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1272 on 2024-11-05 17:18_

Just occurred to me that we should probably add a TODO comment here, because this is actually not quite right (predating this PR).

The correct meta type of an Instance type would be a `type[]` type that includes subclasses, not a class literal type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2090 on 2024-11-05 17:22_

nit: I don't think "compare and hash" are really the behaviors that matter most here? I would write this in terms of type semantics.

```suggestion
/// Some specific instances of some types need to be treated specially by the type system; for example, various special forms are instances of `typing._SpecialForm`, but need to be handled differently in annotations.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2098 on 2024-11-05 17:24_

I agree. I also realized that we are now handling known instances wrongly at least in `Type::is_equivalent_to` and likely in other `Type` methods as well.

Although it was convenient to be able to ignore the `known` field when unpacking a `Type::Instance`, I think it makes it too easy to do the wrong thing.

---

_@carljm approved on 2024-11-05 17:27_

Looks great, thank you!!

---

_@AlexWaygood reviewed on 2024-11-05 17:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2090 on 2024-11-05 17:32_

Yes, thank you! It was thinking through the "oh wait... two `InstanceType`s with the same class now compare and hash differently if one is a known instance and the other is not?" question that made me realise how uncomfortable I was with the new idiom. But that's a facet of our implementation rather than an aspect of what the type _represents_, of course.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1272 on 2024-11-05 17:41_

And worth noting that our choice to invest in improved naming of types is what brought this subtle issue to my attention!

---

_@carljm reviewed on 2024-11-05 17:41_

---

_@carljm reviewed on 2024-11-05 17:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2098 on 2024-11-05 17:51_

https://github.com/astral-sh/ruff/issues/14114

---

_Merged by @AlexWaygood on 2024-11-05 22:16_

---

_Closed by @AlexWaygood on 2024-11-05 22:16_

---
