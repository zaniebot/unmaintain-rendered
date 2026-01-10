```yaml
number: 20340
title: "[ty] Add dedicated variant for `NominalInstance(object)`"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/object-variant
created_at: 2025-09-10T21:19:26Z
updated_at: 2025-09-11T17:03:00Z
url: https://github.com/astral-sh/ruff/pull/20340
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Add dedicated variant for `NominalInstance(object)`

---

_Pull request opened by @dcreager on 2025-09-10 21:19_

Previously, `Type::object` would find the definition of the `object` class in typeshed, load that in (to produce a `ClassLiteral` and `ClassType`), and then create a `NominalInstance` of that class.

It's possible that we are using a typeshed that doesn't define `object`. We will not be able to do much useful work with that kind of typeshed, but it's still a possibility that we have to support at least without panicking. Previously, we would handle this situation by falling back on `Unknown`.

In most cases, that's a perfectly fine fallback! But `object` is also our top type — the type of all values. `Unknown` is _not_ an acceptable stand-in for the top type.

This PR adds a new `NominalInstance` variant for "instances of `object`". Unlike other nominal instances, we do not need to load in `object`'s `ClassType` to instantiate this variant. We will use this new variant even when the current typeshed does not define an `object` class, ensuring that we have a fully static representation of our top type at all times.

There are several operations that need access to a nominal instance's class, and for this new `object` variant we load it lazily only when it's needed. That means this operation is now fallible, since this is where the "typeshed doesn't define `object`" failure shows up.

This new approach also has the benefit of avoiding some salsa cycles that were cropping up while I was debugging #20093, since the new constraint set representation was trying to instantiate `Type::object` while in the middle of processing its definition in typeshed. Cycle handling was kicking in correctly and returning the `Unknown` fallback mentioned above. But the constraint set implementation depends on `Type::object` being a distinct and fully static type, highlighting that this is a correctness fix, not just an optimization fix.

---

_Label `internal` added by @dcreager on 2025-09-10 21:19_

---

_Label `ty` added by @dcreager on 2025-09-10 21:19_

---

_Comment by @github-actions[bot] on 2025-09-10 22:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 22:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~23MB
+     memo metadata = ~22MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~40MB
+     memo metadata = ~38MB

```
</details>


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-09-10 23:27_

---

_Comment by @github-actions[bot] on 2025-09-10 23:31_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://dcreager-object-variant.ecosystem-663.pages.dev/diff)**


---

_Marked ready for review by @dcreager on 2025-09-10 23:40_

---

_Review requested from @carljm by @dcreager on 2025-09-10 23:40_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-10 23:40_

---

_Review requested from @sharkdp by @dcreager on 2025-09-10 23:40_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:212 on 2025-09-11 00:35_

Technically we wouldn't need to load the class from typeshed here, for `Object`? We can just directly return `KnownClass::Object`. Unless you're concerned about inconsistency if `object` is missing from typeshed? (Frankly I'd be fine if we panicked in that scenario anyway.)

It just seems there could be some perf hiding here still, since checking whether a type is a certain known-class is a somewhat common operation.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:191 on 2025-09-11 00:38_

Personally I think it would be fine if we panicked if there is no `object` at all in typeshed (or alternatively, fell back to some empty `object` type defined in `ty_extensions`?) It would certainly be more convenient if this didn't have to become fallible.

---

_@carljm approved on 2025-09-11 00:41_

Looks good to me! I note also that this brings [across-the-board 1-3% perf wins on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fobject-variant), strongly suggesting that this is worth doing, even just for its own sake.

Wouldn't mind getting @AlexWaygood eyes on this before landing, he's done a lot of work on these class/instance representations.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:28 on 2025-09-11 10:11_

nit

```suggestion
    pub(crate) const fn object() -> Self {
        Type::NominalInstance(NominalInstanceType(NominalInstanceInner::Object))
    }

    pub(crate) const fn is_object(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:191 on 2025-09-11 10:17_

I'll co-sign this, and go further: I think I'd actually strongly prefer it if we panicked here when there was no `object` at all in typeshed. If it's a custom typeshed without `object`, I strongly suspect we'll panic in some way with a confusing error message -- and even if we don't, we won't have intuitive behaviour at all when checking any user code. Much better to panic early here with an intelligible error message than attempting to go further. Other type checkers such as mypy also crash quite early on if you omit certain fundamental types from typeshed, so we'd be far from alone in doing this. There's also an internal precedent for this in `TupleType::to_class_type()`: https://github.com/astral-sh/ruff/blob/59c8fda3f8f3bf2cc5c1ae34e7ca9dbea4d0278f/crates/ty_python_semantic/src/types/tuple.rs#L203-L207

As well as that, I just don't think the idea of a "nominal instance type with no class" makes much conceptual sense. The thing that makes an instance type nominal is the fact that the presence of a subtyping relation between two such instances is determined based on whether there is a subclass relations between their underlying classes. I think it's much easier to understand the concept of a nominal instance type -- and therefore much less error-prone for us to work with that concept -- if we maintain the invariant that a nominal instance type must always have a class backing it (even if that class is sometimes only lazily materialized).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:215 on 2025-09-11 10:18_

Could we rename this to:

```suggestion
    pub(super) fn has_known_class(&self, db: &'db dyn Db, known_class: KnownClass) -> bool {
```

?

An instance type might _have_ a known class, but that's a different question to asking whether it _is_ a known class

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:212 on 2025-09-11 10:22_

We could probably return `KnownClass::Tuple` here if the inner variant is `NominalInstanceInner::Tuple` too, without actually materializing the `ClassType`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:211 on 2025-09-11 10:23_

Similar to my comment regarding `is_known`, I'd prefer it if this could be named

```suggestion
    pub(super) fn known_class(&self, db: &'db dyn Db) -> Option<KnownClass> {
```

so that it's clear that it's the underlying class that is "known" rather than the _instance type_. (There could be multiple non-equivalent instance types with the same known class backing them, if that class is generic!)

---

_@AlexWaygood requested changes on 2025-09-11 10:36_

This is excellent! I've wondered whether we should do something like this for a while. It makes total sense to special-case `object` IMO (given its unique status as the top type in Python), and it's great that the recent changes to our `NominalInstanceType` representation make this so clean.

My main point of feedback is that I somewhat strongly feel that `NominalInstanceType::class()` should continue to return `ClassType` rather than `Option<ClassType>` (my reasoning is laid out in an inline comment).

We could possibly also make a similar change to the `ClassBase` enum: we probably don't need to eagerly materialize the `ClassType` here; we could probably add a dedicated `ClassBase` variant:

https://github.com/astral-sh/ruff/blob/59c8fda3f8f3bf2cc5c1ae34e7ca9dbea4d0278f/crates/ty_python_semantic/src/types/class_base.rs#L64-L70

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:212 on 2025-09-11 13:07_

Good ideas, done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:191 on 2025-09-11 13:36_

Works for me! Especially given the precedent with `tuple`

---

_@dcreager reviewed on 2025-09-11 13:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:216 on 2025-09-11 14:33_

nit: Add some short doc-comments for these APIs?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:262 on 2025-09-11 14:33_

```suggestion
    pub(super) const fn is_object(self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:368 on 2025-09-11 14:34_

we know that `object` is not generic; we can do this more cheaply:

```suggestion
            (NominalInstanceInner::Object, NominalInstanceInner::Object) => C::always_satisfiable(db),
            (NominalInstanceInner::NonTuple(class1), NominalInstanceInner::NonTuple(class2)) => {
                class1.is_equivalent_to_impl(db, class2, visitor)
            }
```

---

_@AlexWaygood approved on 2025-09-11 14:36_

Very cool -- thank you!

---

_Comment by @AlexWaygood on 2025-09-11 14:57_

> Memory usage changes were detected when running on open source projects
> 
> ```diff
> trio (https://github.com/python-trio/trio)
> -     memo metadata = ~23MB
> +     memo metadata = ~22MB
> 
> sphinx (https://github.com/sphinx-doc/sphinx)
> -     memo metadata = ~40MB
> +     memo metadata = ~38MB
> ```

This is pretty cool!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:368 on 2025-09-11 14:58_

I added a similar case up in `has_relation_to_impl` for `(_, Object)`, since we know that is always true without having to walk the MROs.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:216 on 2025-09-11 15:00_

Done

---

_@dcreager reviewed on 2025-09-11 15:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:347 on 2025-09-11 15:14_

you can add a similar short-circuit branch for `is_disjoint_from_impl`: if either `self` or `other` is `object` in `NominalInstanceType::is_disjoint_from_impl`, you can immediately return `C::unsatisfiable(db)`

---

_@AlexWaygood reviewed on 2025-09-11 15:15_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:347 on 2025-09-11 16:42_

Done

---

_@dcreager reviewed on 2025-09-11 16:42_

---

_@AlexWaygood approved on 2025-09-11 16:57_

🚀

---

_Merged by @dcreager on 2025-09-11 17:02_

---

_Closed by @dcreager on 2025-09-11 17:02_

---

_Branch deleted on 2025-09-11 17:03_

---
