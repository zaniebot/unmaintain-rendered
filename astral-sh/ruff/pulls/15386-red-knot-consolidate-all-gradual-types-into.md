```yaml
number: 15386
title: "[red-knot] Consolidate all gradual types into single Type variant"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/consolidate-any
created_at: 2025-01-09T20:25:00Z
updated_at: 2025-01-10T02:32:22Z
url: https://github.com/astral-sh/ruff/pull/15386
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Consolidate all gradual types into single Type variant

---

_Pull request opened by @dcreager on 2025-01-09 20:25_

Prompted by https://github.com/astral-sh/ty/issues/222:

> One nit: I think we need to consider `Any` and `Unknown` and `Todo` as all (gradually) equivalent to each other, and thus `type & Any` and `type & Unknown` and `type & Todo` as also equivalent. The distinction between `Any` vs `Unknown` vs `Todo` is entirely about provenance/debugging, there is no type level distinction. (And I've been wondering if the `Any` vs `Unknown` distinction is really worth it.)

The thought here is that _most_ places want to treat `Any`, `Unknown`, and `Todo` identically.  So this PR simplifies things by having a single `Type::Any` variant, and moves the provenance part into a new `AnyType` type.  If you need to treat e.g. `Todo` differently, you still can by pattern-matching into the `AnyType`.  But if you don't, you can just use `Type::Any(_)`.

(This would also allow us to (more easily) distinguish "unknown via an unannotated value" from "unknown because of a typing error" should we want to do that in the future)

---

_Review requested from @carljm by @dcreager on 2025-01-09 20:25_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-09 20:25_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-09 20:25_

---

_Review requested from @sharkdp by @dcreager on 2025-01-09 20:25_

---

_Renamed from "Consolidate all gradual types into single Type variant" to "[red-knot] Consolidate all gradual types into single Type variant" by @dcreager on 2025-01-09 20:25_

---

_Label `red-knot` added by @dcreager on 2025-01-09 20:25_

---

_@sharkdp approved on 2025-01-09 20:33_

I like it! (haven't reviewed each and every change, but I guess it's more or less mechanical)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:505 on 2025-01-09 20:34_

I'm on my phone so I don't have have an easy way to verify it myself and I don't remember if we have a check for this. Does this increase my the overall size of Type in release builds or does it remain unchanged?

---

_@MichaReiser reviewed on 2025-01-09 20:34_

---

_Comment by @github-actions[bot] on 2025-01-09 20:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:505 on 2025-01-09 20:37_

The static assertion is unmodified:

https://github.com/astral-sh/ruff/blob/b0905c4b043189879af78afb757270c4e0397e25/crates/red_knot_python_semantic/src/types.rs#L4001-L4004

---

_@sharkdp reviewed on 2025-01-09 20:37_

---

_@sharkdp reviewed on 2025-01-09 20:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:505 on 2025-01-09 20:43_

The debug size is also the same (32 byte). Which was surprising to me because the `Todo` variant was the largest a while ago (24 byte). But now it has been replaced by `Type::SubclassOf`, which contains `SubclassOfType` of size 24 byte because it can also contain a heavy `Todo`.

---

_Comment by @AlexWaygood on 2025-01-09 21:05_

Brilliant idea! Is it possibly worth calling the variant and struct `Type::Gradual` and `GradualType` rather than `Type::Any` and `AnyType`? `Type::Any(AnyType::Unknown)` reads a bit strangely to me

---

_Comment by @AlexWaygood on 2025-01-09 21:12_

Also: [all roads lead to Rome](https://github.com/python/mypy/blob/ccf05db67f6f99878c73eb902fc59a6f037b18a6/mypy/types.py#L182-L210)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:551 on 2025-01-09 21:12_

nit: I'd be inclined just to call this `Type::any()`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:21 on 2025-01-09 21:13_

(Same here)

---

_@AlexWaygood approved on 2025-01-09 21:17_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:551 on 2025-01-09 21:28_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class_base.rs`:21 on 2025-01-09 21:28_

Done

---

_@dcreager reviewed on 2025-01-09 21:29_

> Brilliant idea! Is it possibly worth calling the variant and struct `Type::Gradual` and `GradualType` rather than `Type::Any` and `AnyType`? `Type::Any(AnyType::Unknown)` reads a bit strangely to me

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4155 on 2025-01-09 21:30_

e.g. here

---

_@dcreager reviewed on 2025-01-09 21:31_

> but I guess it's more or less mechanical

That is a good callout.  I did try to make this a pure refactoring.  For instance, there are a couple of places where we were doing something only for a `Todo`, but not for `Any` or `Unknown`.  Instead of inspecting and possibly changing those in this PR, I've left them as-is so that this PR does not combine a refactoring with a logic change.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2334 on 2025-01-09 21:35_

```suggestion
    // An unannotated value, or a gradual type resulting from an error
```

---

_@AlexWaygood approved on 2025-01-09 21:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:505 on 2025-01-09 21:47_

So I feel bad suggesting a naming change after you just changed the name for another reviewer... but I would rather call this `Type::Dynamic` and `DynamicType`. "Dynamic" or "dyn" is frequently used for this type in the literature, and usually the term "gradual type" is used as a blanket term encompassing all types, including both fully-static ones and partially-dynamic and fully-dynamic ones. That's also how the Python typing spec uses the term "gradual type" (as a blanket term, not a description of the dynamic type specifically.) Whereas "dynamic" is the term we use for this type right here in the doc comment above.

---

_@carljm approved on 2025-01-09 21:48_

This is excellent!!

---

_@AlexWaygood reviewed on 2025-01-09 21:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:505 on 2025-01-09 21:49_

Yeah, this is on me -- I agree `Dynamic` is a better name, for all the reasons @carljm mentions. Sorry @dcreager!!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2334 on 2025-01-09 21:55_

```suggestion
    // An unannotated value, or a dynamic type resulting from an error
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2353 on 2025-01-09 21:56_

```suggestion
            // `DynamicType::Todo`'s display should be explicit that is not a valid display of
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:156 on 2025-01-09 21:58_

```suggestion
        match self {
            Self::Class(class) => Some(class),
            Self::Dynamic(_) => None,
```

---

_@AlexWaygood approved on 2025-01-09 21:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1082 on 2025-01-09 21:59_

Side note on seeing this method, wondering why we need it, and going to look: I wonder if maybe we shouldn't allow `Any/Unknown/Todo` to coexist in the same union/intersection, and instead should just define a hierarchy of preference, e.g. `Todo` takes top priority, `Any` second, and `Unknown` third?

Definitely not something for this PR, just musing.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2087 on 2025-01-09 22:00_

```suggestion
                ClassBase::Dynamic(dynamic) => Type::Dynamic(dynamic),
```

---

_@carljm approved on 2025-01-09 22:03_

This makes handling these in match statements so much nicer!

---

_Merged by @dcreager on 2025-01-10 02:32_

---

_Closed by @dcreager on 2025-01-10 02:32_

---

_Branch deleted on 2025-01-10 02:32_

---
