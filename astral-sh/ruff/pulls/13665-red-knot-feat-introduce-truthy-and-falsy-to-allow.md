```yaml
number: 13665
title: "[red-knot] feat: Introduce `Truthy` and `Falsy` to allow more precise typing"
type: pull_request
state: closed
author: Slyces
labels:
  - ty
assignees: []
base: main
head: feat/truthy-falsy-type-narrowing
created_at: 2024-10-07T15:56:05Z
updated_at: 2024-10-28T10:06:21Z
url: https://github.com/astral-sh/ruff/pull/13665
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] feat: Introduce `Truthy` and `Falsy` to allow more precise typing

---

_Pull request opened by @Slyces on 2024-10-07 15:56_

## Summary

This is a PR trying to address astral-sh/ruff#13632.

Reminder of the problem: when evaluating boolean operations, we could improve precision of types
```python
reveal_type(str_instance() and 8)  # Current: `str | Literal[8]` -- Expected `Literal["", 8]`
``` 
The idea is that in some contexts, we can narrow a type to the subset of instances that are `truthy` (`boo(instance) == True`) or the subset of instances that are `falsy`(`boo(instance) == False`).

As @carljm suggested, a very generic way to express this would be to implement two new types, `Truthy` and `Falsy`, and express "the subset of `A` instances that evaluate to `True`" as `A & Truthy` (and conversely for `False` and `Falsy`).

### Interface

On a high level, this should be fairly straightforward to use when required

- Introduce two new types `Truthy` and `Falsy`
- Convenience method to create `A & Truthy` (and falsy): `IntersectionBuilder::build_truthy(a_ty)`

### Interesting Cases

Here's a list of interesting cases that might be unlocked by this more precise type knowledge. Some of them are more or less complicated to handle and maybe shouldn't be supported.

#### Behaviour of Falsy and Truthy outside of intersections

While they only make sense in intersections and probably should only exist in that context, we do need to implement the behaviour of `Falsy` and `Truthy` outside of intersections, as they're a `Type` variant.
Suggestions are welcome if I didn't handle that part properly, I don't have specific opinions or feedback on this part of the implementation.

#### Consistency of intersections

As it's been raised in [this comment](https://github.com/astral-sh/ruff/issues/13632#issuecomment-2395193418) by @AlexWaygood, the existence of `Truthy` and `Falsy` coupled with having both `positive` (must be) and negative (must not be) elements in intersection leads to a question:
- Do we need both `truthy` and `falsy`
- `X & Falsy` = `X & ~Truthy`

Currently, as this PR explores a first attempt with `Truthy` and `Falsy`, we need to make sure that only one of the above two representations is used.

#### Defined falsy and truthy subsets

Some types have a known subset of instances that are `falsy`:
- `bool → False`
- `int → 0`
- `str → ""`
- `bytes → b''`
- `tuple → (,)`
- `list → []`, `dict → {}`, `set → set()` - we can't really represent that at the moment

Conversely for truthy (most types in Python are truthy, so this is rarely defined)
- `bool -> True`

This could be used in a few ways (that I can think of)
- Simplify `int & Falsy` to `Literal[0]` (we most probably want that)
- Simplify `int & Truthy | Literal[0]` to `int`
  ```python
  # Case 1
  reveal_type(int_instance() or 0)  # Should be `int != 0 | 0` → `int`
  ```
  - Note: that's a more advanced case of `X & Truthy | X & Falsy` → `X` (which we should support)
- Unify `int & ~0` (where `~0` is `Literal[0]` in the negatives) to `int & Truthy`
  - This is not really a "simplification" as both say the same thing, but we need consistency
- `x1 | x2` where `x1` is `X & Truthy` and `x2` is `X & Falsy`
  - This is what we're doing with `BooleanLiteral(true)`, `BooleanLiteral(false)` and `bool`
  - Not coded as I can't think of any other type that has both a known set of ` truthy` and `falsy` values
  
So far, all subsets I can think of have been singletons (no `UnionType`). This means that my code might maybe either be too simple (can't handle non-singleton) or too complicated (tries to handle non singleton) at times.

I guess that if we want to include this knowledge in the feature, knowing if non-singleton cases should be supported (or if they even exist) would help.
  
#### Simplifying `X & Falsy` / `X & Truthy`

I think that's a fairly straightforward feature. Some types (e.g. `FunctionType`) have a constant `Truthiness` (for `FunctionType.bool() → Truthiness::AlwaysTrue`), so associating them with `Truthy` or `Falsy` can be simplified.
- `X & Truthy` → `X` if `x.bool() == Truthiness::AlwaysTrue`
- `X & Falsy` → `Never` if `x.bool() == Truthiness::AlwaysTrue`
- ... (same for `AlwaysFalse`)

Overall, we should probably guarantee at build time that you can't have both `AlwaysTrue` and `AlwaysFalse` elements in an intersection, because elements of this intersection would need to have both properties, and that's not possible.

## Test Plan

- Added unit tests in the `builder.rs` for intersection/union cases
  - Overall, all features written about above should have one or more dedicated tests
- Added some "integration tests" in `infer.rs` through boolean chained operations (currently the only way to create `X & Truthy` or `X & Falsy` intersections)
- Edited existing tests with a more precise type inference
- Solved a random test's `// TODO`
  - `Literal[0] | None | Literal[1] & None` became `Literal[0] | None` with this PR which was the expected behaviour
  - Note that this should have been solved by other features eventually

We could eventually add some additional integration tests that would be "easier to read" by adding support for the following snippet
```python
a = A()
reveal_type(a)  # type: `A`
if A():
   reveal_type(a)  # type: `A & Truthy`
```

## Concerns & Feedback on the current implementation

I think this feature is very exciting. It would allow us to represent types with more precision, which is always nice.

I do have some concerns with my implementation as it is now
- Taken individually, most cases are easy to follow
- But it feels like I "hardcorded" many corner cases in the code
  - Especially the `InnerIntersectionBuilder::simplify` method
- I think that I can't really picture the kind of intersections we'll work with in practice, so I might either not test enough or test things that can't happen
- Performance might not be optimal in some areas

As always feedback is welcome, don't hesitate to question the entire direction I took to achieve this, I'll be happy to refine this with whatever we can learn from this first iteration.


---

_Review requested from @carljm by @Slyces on 2024-10-07 15:56_

---

_Review requested from @MichaReiser by @Slyces on 2024-10-07 15:56_

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-07 15:56_

---

_Comment by @github-actions[bot] on 2024-10-07 16:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Feat: introduce `Truthy` and `Falsy` to allow more precise typing" to "[red-knot] feat: Introduce `Truthy` and `Falsy` to allow more precise typing" by @Slyces on 2024-10-07 16:51_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-07 18:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:422 on 2024-10-07 18:38_

Not sure if the compiler can see through this, but it seems like it might be more efficient to do a nested match here (single outer clause for `Self::Instance(class)` and then an inner match on `class.known`), so we only match once on `class.known` rather than separately for every `class.is_known` call in every `if` clause.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-07 18:44_

This method is really implementing "simplification of intersection with Falsy", which is logic I would expect to find in `IntersectionBuilder` rather than in a method on `Type`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:439 on 2024-10-07 18:48_

The doc comment on this method strongly suggests that it should always return a subtype of `self`, since it talks about the "set of values... in this type". (In fact, that might be a useful `debug_assert!`)? But these two cases don't return a subtype of `self`. `self` might be `Literal[True]` and this would return `Literal[False]`, requiring another intersection to resolve down to the correct return type of `Never`.

I think we should instead have separate cases here for `Literal[True]` (returns `Never`), `Literal[False]` (returns self), `Literal[0]` (returns self), and any other int literal (returns `Never`).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:446 on 2024-10-07 18:49_

The "can be expressed" formulation, here and above, has an implicit "without an explicit intersection type" caveat: all intersections with truthy/falsy "can be expressed" as an intersection. This is another reason why I think these methods should rather be internal implementation details of `IntersectionBuilder`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:453 on 2024-10-07 18:50_

Same comment as above; `Literal[False]` should return `Never` here, not `Literal[True]`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:563 on 2024-10-07 19:44_

These should be `Type::Todo` for now. The correct implementation here will be to return a bound method returning `Literal[True]` or `Literal[False]` if someone accesses `__bool__`, and otherwise fall back to member access on `object`. We shouldn't just fall back to `Unknown` silently; these types are not gradual forms.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:617 on 2024-10-07 19:47_

nit: the word "generic" here confused me, since I thought you meant generic types
```suggestion
            // Temporary special case for `None` until we handle all instances
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:630 on 2024-10-07 19:48_

Was this necessary to unblock something else in this PR? If so, that's fine. If not, why should we add a temporary special case for this? Maybe we just leave it out and wait for the general Type::Instance handling?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:249 on 2024-10-07 19:56_

I'd like for these doc comments to mostly stick to a factual description of what the type represents. Details of how it is used and examples of its use (especially in this case, as we figure out this feature over time) are likely to change, and too much detail here is just likely to go out of date.
```suggestion
    /// The type representing all objects whose `__bool__` returns `True`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:253 on 2024-10-07 19:57_

```suggestion
    /// The type representing all objects whose `__bool__` returns `False`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:813 on 2024-10-07 20:01_

This should also result in a diagnostic; we could group it under the below TODO comment for that, for now

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:818 on 2024-10-07 20:02_

I don't think the added comment here is necessary, nor was the pre-existing comment, now that we have `Type::Todo`:
```suggestion
```

---

_Comment by @carljm on 2024-10-08 05:17_

Thanks for the PR, this is very cool! I've started a review, but there's a lot to consider here, and I have some other pressing things to take care of, so it may be a couple days before I can complete the review.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:441 on 2024-10-08 11:08_

There's a few issues here:
1. An important correctness bug
2. Some missed opportunities where we can give some more precise answers
3. Some opportunities for simplification

The correctness bug is in your treatment of `IntLiteral` and `BooleanLiteral` branches. If a value has `Type::IntLiteral(_)`, it doesn't mean that it's an _arbitrary_ literal integer. We don't currently have a way of expressing such a type in our model (and possibly never will) -- if we did, it would probably be called `LiteralInt`, analogous to our `LiteralString` type. Rather, if a value has `Type::IntLiteral(_)`, it indicates that it is a _specific_ (known) literal integer: `Type::IntLiteral(2)` represents the specific value `2`, and `Type::IntLiteral(3)` represents the specific value `3`. As such, it's not true that `Type::IntLiteral(_).falsey_set(db) == KnownClass::Int.to_instance(db).falsey_set(db)`. If `self` is `Type::IntLiteral(3)`, then the set of possible values that `self` represents has size exactly 1, and the subset of those possible values that are falsey is the empty set (which is `Type::Never`).

The missed opportunity for more precise answers regards arbitrary instances that have `__bool__` methods annotated as returning `Literal[False]`. For these instances (which we haven't implemented the dunder-method lookup for yet in `Type::bool`), we know they are always falsey, so we know that the subset of possible falsey values is exactly equal to the original type. (For an always-falsy type `X`, `X & Falsy == X`; for an always-truthy type `Y`, `Y & Falsy == Never`.)

The opportunity for simplification is that I think it would be more readable (and possibly more efficient) if you used an inner match statement using `ClassType::known` rather than multiple branches using `ClassType::is_known`.

Putting all this together, I would make these changes:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -419,25 +419,24 @@ impl<'db> Type<'db> {
     pub fn falsy_set(&self, db: &'db dyn Db) -> Option<Type<'db>> {
         match self {
             // Some builtin types have known falsy values
-            Self::Instance(class) if class.is_known(db, KnownClass::Bool) => {
-                Some(Self::BooleanLiteral(false))
-            }
-            Self::Instance(class) if class.is_known(db, KnownClass::Int) => {
-                Some(Self::IntLiteral(0))
-            }
-            Self::Instance(class) if class.is_known(db, KnownClass::Str) => {
-                Some(Self::StringLiteral(StringLiteralType::new(db, "".into())))
-            }
-            Self::Instance(class) if class.is_known(db, KnownClass::Bytes) => {
-                Some(Self::BytesLiteral(BytesLiteralType::new(db, vec![].into())))
-            }
-            Self::Instance(class) if class.is_known(db, KnownClass::Tuple) => {
-                Some(Self::Tuple(TupleType::new(db, vec![].into())))
-            }
-            Type::LiteralString => KnownClass::Str.to_instance(db).falsy_set(db),
-            Type::IntLiteral(_) => KnownClass::Int.to_instance(db).falsy_set(db),
-            Type::BooleanLiteral(_) => KnownClass::Bool.to_instance(db).falsy_set(db),
-            _ => None,
+            Self::Instance(class) => match class.known(db) {
+                Some(KnownClass::Bool) => Some(Self::BooleanLiteral(false)),
+                Some(KnownClass::Int) => Some(Self::IntLiteral(0)),
+                Some(KnownClass::Bytes) => Some(Self::BytesLiteral(BytesLiteralType::empty(db))),
+                Some(KnownClass::Str) => Some(Self::StringLiteral(StringLiteralType::empty(db))),
+                Some(KnownClass::Tuple) => Some(Self::Tuple(TupleType::empty(db))),
+                _ => match self.bool(db) {
+                    Truthiness::AlwaysFalse => Some(*self),
+                    Truthiness::AlwaysTrue => Some(Type::Never),
+                    Truthiness::Ambiguous => None,
+                },
+            },
+            Type::LiteralString => Some(Self::StringLiteral(StringLiteralType::empty(db))),
+            _ => match self.bool(db) {
+                Truthiness::AlwaysFalse => Some(*self),
+                Truthiness::AlwaysTrue => Some(Type::Never),
+                Truthiness::Ambiguous => None,
+            },
         }
     }
 
@@ -1593,18 +1592,36 @@ pub struct StringLiteralType<'db> {
     value: Box<str>,
 }
 
+impl<'db> StringLiteralType<'db> {
+    pub fn empty(db: &'db dyn Db) -> Self {
+        Self::new(db, Box::default())
+    }
+}
+
 #[salsa::interned]
 pub struct BytesLiteralType<'db> {
     #[return_ref]
     value: Box<[u8]>,
 }
 
+impl<'db> BytesLiteralType<'db> {
+    pub fn empty(db: &'db dyn Db) -> Self {
+        Self::new(db, Box::default())
+    }
+}
+
 #[salsa::interned]
 pub struct TupleType<'db> {
     #[return_ref]
     elements: Box<[Type<'db>]>,
 }
 
+impl<'db> TupleType<'db> {
+    pub fn empty(db: &'db dyn Db) -> Self {
+        Self::new(db, Box::default())
+    }
+}
+
```

I think similar comments would apply to your `truthy_set` method below.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:620 on 2024-10-08 11:11_

We currently always treat `None` values as `Type::None` rather than as `Type::Instance(NoneType)` (although we [probably shouldn't](https://github.com/astral-sh/ruff/issues/13670) -- but that might be tricky to resolve until [a few other things](https://github.com/astral-sh/ruff/issues/12694) get done).

None of your tests fail if I remove this branch, so it seems at least to be uncovered:

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:650 on 2024-10-08 11:14_

I'd prefer to use "Python type" notation rather than maths notation in comments... but kudos for knowing the maths notation 😄

```suggestion
                // - `Ambiguous` & `Truthy` == `Truthy`
                // - `Ambiguous` & `Falsy` == `Falsy`
                // - `Truthy` & `Falsy` == `Never`  -- this should be impossible to build
```

---

_@AlexWaygood had review dismissed on 2024-10-08 11:18_

Thank you! I'll echo Carl that this is pretty cool!!

Carl's probably better placed to do a full review here, in particular for the logic you added in `builder.rs` -- but here's some things I noticed, in the meantime:

---

_@Slyces reviewed on 2024-10-08 11:48_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:620 on 2024-10-08 11:48_

No problem with removing this - I also started looking into #13670, and it's actually more work than we would think (`KnownClass::NoneType.to_instance(db)` is a `Type::Union` including two `None` and an `Unknown` ...)
So anyway - working with `Type::None` for now is probably the better choice.

---

_@AlexWaygood reviewed on 2024-10-08 11:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:620 on 2024-10-08 11:51_

Yup, we don't support `sys.version_info` branches yet either! Which will also be somewhat involved. I need to revive https://github.com/astral-sh/ruff/pull/13257 at some point -- it's on my TODO list for this week, but we need to do some internal wrangling about the exact design we're all happy with before we can procede with that :-)

---

_@Slyces reviewed on 2024-10-08 11:52_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:620 on 2024-10-08 11:52_

This comment about [sealed types](https://github.com/astral-sh/ruff/issues/12694) make me think there might be a connection to the logic I've been trying to cover in this PR

---

_@Slyces reviewed on 2024-10-08 12:34_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:441 on 2024-10-08 12:34_

No problem for the syntax improvements, and for the logic bug, good catch! I added some comments of my understanding of the case for people that will read the code.

---

_@AlexWaygood reviewed on 2024-10-08 12:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:462 on 2024-10-08 12:43_

Similar to the `falsy_set` method above, I think this should have a fallback branch for arbitrary types where `self.bool()` returns either `AlwaysTruthy` or `AlwaysFalsy`. And if you add such a fallback branch, you should be able to get rid of the `Type::None` branch, since `Type::None` is known to be an `AlwaysFalsy` type.

```suggestion
            Type::Instance(class) => match class.known(db) {
                Some(KnownClass::Bool) => Some(Type::BooleanLiteral(true)),
                _ => match self.bool(db) {
                    // When a `__bool__` signature is `AlwaysTrue`, it means that the truthy set is
                    // the whole type (that we had as input), and the falsy set is empty.
                    Truthiness::AlwaysTrue => Some(*self),
                    Truthiness::AlwaysFalse => Some(Type::Never),
                    Truthiness::Ambiguous => None,
                },
            },
            _ => match self.bool(db) {
                Truthiness::AlwaysFalse => Some(*self),
                Truthiness::AlwaysTrue => Some(Type::Never),
                Truthiness::Ambiguous => None,
            },
```

You could possibly also add branches here for some of the types you enumerated in the `always_falsy` method above. E.g. `str & Truthy` could be simplified to `str & ~Literal[""]`: the set of all truthy strings is exactly equal to the set of all strings that are not the empty string. But I'm not sure if that's a good idea or not. _Is_ it a simplification? It's actually a more complicated way to express the same type, even if it's easier for me to understand.

---

_@Slyces reviewed on 2024-10-08 12:50_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:462 on 2024-10-08 12:50_

> You could possibly also add branches here for some of the types you enumerated in the always_falsy method above. E.g. str & Truthy could be simplified to str & ~Literal[""]: the set of all truthy strings is exactly equal to the set of all strings that are not the empty string. But I'm not sure if that's a good idea or not. Is it a simplification? It's actually a more complicated way to express the same type, even if it's easier for me to understand.

I think we might want to keep this idea somewhere, but this could either become a critical way to improve the logic of simplification or on the opposite it could complicate things.

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-08 13:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:413 on 2024-10-09 23:40_

Why do we do this short-circuit through `self.falsy_set` here but not in `truthy` above?

As commented below, I think that the `truthy_set` and `falsy_set` methods should be internal to `IntersectionBuilder`, not methods on `Type`, and I don't think we should break the abstraction here: we should just build the right intersection, and `IntersectionBuilder` should be responsible to simplify that intersection as appropriate.

To be honest, I probably wouldn't include these helper methods at all, unless it's really clear that these will be very common operations and it's important that we make them as ergonomic as possible. I'd rather just go with a general `IntersectionType::from_elements` helper and use that where we need to build any intersection, including intersections with `Truthy` and `Falsy`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2480 on 2024-10-09 23:56_

```suggestion
                        // We only every return a type early in an `or` if it's a part of the
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2491 on 2024-10-09 23:58_

```suggestion
                            ty  // This type is  already always truthy/falsy
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7679 on 2024-10-10 00:15_

Now that the new test framework is merged, it would be great if we could add this as a Markdown-style test instead (see https://github.com/astral-sh/ruff/pull/13695 for documentation.) If you don't want to make that update, it's fine though -- this PR does predate the test framework landing, after all :) It'll just be one more test to port later.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:442 on 2024-10-10 19:25_

I think this duplication can be avoided by having a two-layer structure, where an inner method (or an initial match statement within one method) handles only the specific known cases, and if it returns `None`, an outer method (or second check) tries the "convert to bool" option.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:627 on 2024-10-10 19:27_

Using the word "generic" like this is ambiguous, sounds like it could mean generic types.
```suggestion
            // Temporary special case for `FunctionType` until we handle general `Type::Instance`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:666 on 2024-10-10 19:35_

This comment is more narrow than what the code actually does; `Truthy` and `Falsy` aren't the only types that have a non-ambiguous truthiness.

What this means is that this logic is only correct if we do eagerly check truthiness of every type added to an intersection and resolve to Never if we find both an always-truthy type and an always-falsy type in the intersection. I think it's _correct_ to do this, I'm a bit worried about the performance impact. But I think we should probably try doing it and see if it actually becomes a performance bottleneck.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:842 on 2024-10-10 19:41_

This should be `type[object]` - for now we can just make it a Todo with a comment that we don't have generic `type` yet (like some other todo comments in this method)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1595 on 2024-10-10 19:52_

I think if we generalize our approach in the intersection builder, we shouldn't need these methods; I think the approach using these methods is more special-cased than what we ideally want.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:85 on 2024-10-10 20:18_

`Truthy | Falsy` isn't `Never`, it's `object`. `Truthy & Falsy` is `Never`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:273 on 2024-10-10 20:59_

If we need better ergonomics for creating intersections, I'd rather have a more general `IntersectionType::from_elements(&db, impl IntoIterator<Item = Type>) -> Type`, parallel to `UnionType::from_elements`, rather than these dedicated methods for truthy and falsy.

`IntersectionType::from_elements(db, &[ty, Type::Truthy])` is not that much more verbose than `IntersectionBuilder::build_truthy(db, ty)`, and can be used in a lot more cases.

If you feel like these helpers are important to minimize verbosity in the builder tests, we could add them as test-only helper functions.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:613 on 2024-10-10 21:04_

`Truthy | Falsy` is `object`, not `Never`. Both of these unions should result in simply `object`. The type `Truthy` means "all truthy objects", the type `Falsy` means "all falsy objects", and all objects are either truthy or falsy, so the two types combined is "all objects."

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:665 on 2024-10-11 00:35_

```suggestion
                    // should simplify to Never at build time.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7660 on 2024-10-11 00:50_

We can't safely make either of these simplifications, as commented above. That means either we have less-simplified types in some of these tests (un-reduced intersections), and/or we switch to a type like `LiteralString` that can be simplified when intersected with `Falsy`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7665 on 2024-10-11 00:51_

this simplification isn't safe either, though it's only in this comment, not in the actual test

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7676 on 2024-10-11 00:52_

I think if we stop unsafely simplifying `int` intersection with Falsy, this corner case goes away?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7599 on 2024-10-11 00:54_

We can't make this simplification; at best this can be `str & Truthy | Literal[False]`.

But as described in another comment, I think it should probably stay just `str | Literal[False]`, because of the issue of mutability.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7572 on 2024-10-11 00:56_

As described in another comment, I think we probably need to eliminate `Truthy` and `Falsy` in intersections with mutable types (or types that might have mutable subclasses), which means this should stay just `str` as it was before.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4260 on 2024-10-11 00:57_

So in thinking through this PR, I've realized that in every case where we can't simplify an intersection with `Truthy` or `Falsy` to a known literal type, we should probably just simplify it out of the intersection altogether. In other words, if `X & Falsy` doesn't simplify via the logic in `falsy_set`, it should just immediately simplify to `X`, and same for `X & Truthy` and `truthy_set`. So I think this should stay just `A | B`.

To illustrate why, consider this code:

```py
def f(data: list[int]):
    if data:
        data.clear()
        reveal_type(data)  # if we say `list[int] & Truthy` here, we are wrong
```

The problem is mutability, which is generally a problem for narrowing, but more of a problem for some kinds of narrowing than for others. Narrowing a nominal type via `isinstance`, for example, is only invalidated by mutating the `__class__` attribute, which is rare and we can easily detect and error on it. Truthiness and falsiness are a property that is more easily flipped by mutation, and we can't feasibly detect all the cases where it can be flipped. This means to be safe I think we have to _only_ narrow based on truthiness in cases where we know the type in question is immutable and its truthiness can't change; and that's really only true for the cases described in `truthy_set` and `falsy_set`.

I think we still want the `Truthy` and `Falsy` types, because it allows us to describe all cases of type narrowing as intersections, which will be very convenient. But I think the implication of this is that `Truthy` and `Falsy` should always immediately disappear from any intersection they are added to.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:958 on 2024-10-11 01:01_

This test is correct and should pass, but not for the reason it was added; rather, but just because we should simplify `int & Truthy` to `int` and `str & Truthy` to `str`, and then unioning `int` or `str` with an `int` or `str` literal is still just `int` or `str` due to subtyping.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:917 on 2024-10-11 01:02_

```suggestion
        // `object & Falsy | object & Truthy` -> `object`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:933 on 2024-10-11 01:02_

This should not actually be a special case, because we can't simplify `int & False` to `Literal[0]` -- but it should still hold as part of the general pattern.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:911 on 2024-10-11 01:04_

I think this test will become irrelevant if we always simplify `Truthy` and `Falsy` out of intersections, as I suggest in another comment that we need to do. This logic becomes an internal implementation detail of intersection builder, not something externally visible in a built intersection type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:877 on 2024-10-11 01:06_

I don't think these types are actually equivalent (again because of the possibility of int subtypes), so we shouldn't do this translation in either direction.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:428 on 2024-10-11 01:09_

Unfortunately, because all four of these types (int, bytes, str, tuple) are subclassable, and the subclass is free to override `__bool__` to have whatever behavior the subclass wants, we can't safely make this simplification. E.g. `str & Falsy` could include an instance of a custom subclass of `str` with custom behavior that makes it falsy. So I think we need to remove these four cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:861 on 2024-10-11 01:19_

This is a bug that pre-exists your PR, but unfortunately even `Type::Tuple` should always have `Truthiness::Ambiguous`, again because of subclassing: even the "empty tuple" type can include an "empty" instance of a hypothetical tuple subclass that might have arbitrary `__bool__` behavior; the `__bool__` behavior isn't specified as part of the tuple type. So we should fix this in the `Type::bool` method so `Type::Tuple` is always `Truthiness::Ambiguous`, and rewrite this test to use a different type than tuple.

(This doesn't apply to e.g. BytesLiteral or StringLiteral or IntLiteral -- those types can only represent _literals_, so we know they are the built-in type, not a subclass.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:828 on 2024-10-11 01:22_

As mentioned below, we can't actually know that a non-empty `Type::Tuple` is truthy, so we should fix that in `Type::bool` and use a different truthy type here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:398 on 2024-10-11 06:45_

This ends up calling `.bool(db)` on every type in `self.positive` four times; it shouldn't be necessary to do it more than once.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:436 on 2024-10-11 06:50_

I think we should handle this more like union does, doing pairwise checks between each element (and probably better to do it as we add elements, rather than in a final `simplify` pass). And preferably the check would be more generic, like `simplify_intersection_of(db, a: Type, b: Type) -> Option<Type>` (returning `None` if no simplification is possible, or a Type if the intersection of `a` and `b` can simplify to a type) rather than so special-cased to `Truthy` and `Falsy`. There are a lot more cases where we can simplify intersections than just Truthy and Falsy (e.g. intersections between a lot of not-identical literal types just simplify to Never). Not saying we have to implement all of those in this PR, just that we should try to set up general infrastructure based on general set-theoretic principles, rather than a long list of special cases, which is going to be both harder to maintain and a lot less efficient.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:140 on 2024-10-11 06:54_

Not going to comment in detail on this since I've suggested semantic changes in other comments that I think will end up changing it a lot anyway. But let's keep in mind the general principles that we need to think pretty hard about doing things as efficiently as we can, and we should prefer abstracting to general relations between types whenever we can. For instance, we already have the subtype checks below and the bool checks above -- I think all of it, and a lot of the new stuff you're adding, maybe could abstract to a general `simplify_union_of(db, a: Type, b: Type) -> Option<Type>` which returns `None` if there is no simplification and `a` and `b` should both stay in the union, or a new `Type` to be added to the union instead of `a` and `b`. (Not 100% sure if that will cover all cases without actually working it all out in code myself, but hopefully that gives the flavor of the kind of abstractions we should be looking for to keep this code maintainable.)

---

_@carljm requested changes on 2024-10-11 06:58_

Thank you for tackling this! It's quite an ambitious project, and you definitely took it further than I'd anticipated for a first PR on the subject. Reviewing it was super useful in forcing me to think through a bunch of cases carefully, so regardless of what happens with it now, that was a super useful contribution.

My conclusion (recorded in the inline comments) is that I think we need to significantly scale back our aggressiveness in applying conclusions from truthiness and falsiness, which I think will probably simplify some of the handling. And in general I'd like to see the handling in UnionBuilder and IntersectionBuilder be a little more general and do less avoidable repeated work.

Thanks again, and sorry for the delayed review!

---

_Comment by @carljm on 2024-10-25 18:00_

As I've written up over at https://github.com/astral-sh/ruff/issues/13694#issuecomment-2438438759, I no longer think that Truthy and Falsy types are the right way for us to approach this issue, so I'm going to close this PR.

I'm sorry that means we won't land the work you put into this PR, but it doesn't mean this PR wasn't helpful; in fact reviewing it was critical in clarifying my thinking on this topic. Thank you so much for the PR!

---

_Closed by @carljm on 2024-10-25 18:00_

---

_Branch deleted on 2024-10-28 10:06_

---
