```yaml
number: 13411
title: "[red-knot] more efficient UnionBuilder::add"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/union-builder
created_at: 2024-09-19T19:43:39Z
updated_at: 2024-09-20T17:49:47Z
url: https://github.com/astral-sh/ruff/pull/13411
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] more efficient UnionBuilder::add

---

_Pull request opened by @carljm on 2024-09-19 19:43_

Avoid quadratic time in subsumed elements when adding a super-type of existing union elements.

Reserve space in advance when adding multiple elements (from another union) to a union.

Make union elements a `Box<[Type]>` instead of an `FxOrderSet`; the set doesn't buy much since the rules of union uniqueness are defined in terms of supertype/subtype, not in terms of simple type identity.

Move sealed-boolean handling out of a separate `UnionBuilder::simplify` method and into `UnionBuilder::add`; now that `add` is iterating existing elements anyway, this is more efficient.

Remove `UnionType::contains`, since it's now `O(n)` and we shouldn't really need it, generally we care about subtype/supertype, not type identity. (Right now it's used for `Type::Unbound`, which shouldn't even be a type.)

Add support for `is_subtype_of` for the `object` type.

Addresses comments on https://github.com/astral-sh/ruff/pull/13401

---

_Label `red-knot` added by @carljm on 2024-09-19 19:43_

---

_Review requested from @MichaReiser by @carljm on 2024-09-19 19:43_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-19 19:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-19 20:07_

Could we use `Box<[Type<'db>]>` for `UnionType::elements`, since the elements of a constructed `UnionType` should be immutable?

Should we even have a `pub fn contains` method on `UnionType` anymore, since it's now O(n) and (per https://github.com/astral-sh/ruff/pull/13401#issuecomment-2361238572) we don't expect to need it very often/it's probably better to special-case specific types like `Any` and `Unknown` when it comes to testing containment?

---

_@AlexWaygood reviewed on 2024-09-19 20:11_

---

_Comment by @github-actions[bot] on 2024-09-19 20:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-09-19 20:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-19 20:13_

Good call! Once again, I intended to switch to `Box<[Type]>` and then totally forgot about it once I had everything else working. Need a better short-term TODO stack, my brain is not up to the task 😆 

I'll check where we use `contains`...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-19 20:15_

Our only uses of `contains` are for checking if it contains Unbound, and I don't even want Unbound to be a type at all. I'll remove the method and make those two call sites do it manually instead, for now.

---

_@carljm reviewed on 2024-09-19 20:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-19 20:33_

Ok so actually removing it is kind of difficult because the `elements` field is not pub and one of the uses is in a lint rule over in a different crate. So I just deprecated it with a TODO note that it should go away when we remove `Type::Unbound`.

---

_@carljm reviewed on 2024-09-19 20:33_

---

_@carljm reviewed on 2024-09-19 20:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-19 20:46_

Actually never mind I can change that code to use `is_unbound` and `may_be_unbound`, and get rid of the method.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:95 on 2024-09-19 23:58_

You can simplify this a little bit -- get rid of the `.map()` call at the end:

```suggestion
                        let mut elements_iter = self.elements.iter();
                        'outer: for remove_index in remove_iter.by_ref() {
                            for (index, element) in elements_iter.by_ref().enumerate() {
                                if index == *remove_index {
                                    continue 'outer;
                                }
                                retain.push(*element);
                            }
                        }
                        retain.extend(elements_iter);
```

It is also quite tempting to use `swap_remove` as @MichaReiser suggested in https://github.com/astral-sh/ruff/pull/13401#discussion_r1766580699, since `swap_remove` is O(1)...

<details>
<summary>Diff using `swap_remove`</summary>

```diff
diff --git a/crates/red_knot_python_semantic/src/types/builder.rs b/crates/red_knot_python_semantic/src/types/builder.rs
index b35c76df6..c218c36d3 100644
--- a/crates/red_knot_python_semantic/src/types/builder.rs
+++ b/crates/red_knot_python_semantic/src/types/builder.rs
@@ -76,27 +76,10 @@ impl<'db> UnionBuilder<'db> {
                         to_remove.push(index);
                     }
                 }
-                match to_remove[..] {
-                    [] => self.elements.push(to_add),
-                    [index] => self.elements[index] = to_add,
-                    _ => {
-                        let mut retain =
-                            Vec::with_capacity(self.elements.len() - to_remove.len() + 1);
-                        let mut remove_iter = to_remove.iter();
-                        let mut elements_iter = self.elements.iter().enumerate();
-                        'outer: for remove_index in remove_iter.by_ref() {
-                            for (index, element) in elements_iter.by_ref() {
-                                if index == *remove_index {
-                                    continue 'outer;
-                                }
-                                retain.push(*element);
-                            }
-                        }
-                        retain.extend(elements_iter.map(|(_, element)| element));
-                        retain.push(to_add);
-                        self.elements = retain;
-                    }
+                for index in to_remove {
+                    self.elements.swap_remove(index);
                 }
+                self.elements.push(to_add);
             }
         }
```

</details>

But I think the price is perhaps too much to pay, because of course we'll use `UnionBuilder::add()` to construct (and eagerly simplify) even unions that users have provided to us in type annotations. And it just seems too confusing to display union elements to users in a completely different order to the order in which they gave us the elements.

_Unless_ we retained the `TextRange` of the original annotation, and used that to get back to the original source code for the annotation they used when constructing the diagnostic... but that could add complications, because we'd have to distinguish between user-supplied unions (which would have `TextRange`s attached to them) and red-knot-constructed unions (which would not). And storing an `Option<TextRange>` field on `UnionType` instances obviously wouldn't be free either.

It's quite a fun problem figuring out how to micro-optimise this while retaining order as best as possible! Here's another solution that uses `Vec::retain()`, which means that it operates in-place instead of allocating a new `Vec` and replacing `self.elements` with the new `Vec` (it takes its inspiration from [the second example in the docs](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.retain)):

<details>
<summary>Alternative solution using `Vec::retain()`</summary>

```diff
--- a/crates/red_knot_python_semantic/src/types/builder.rs
+++ b/crates/red_knot_python_semantic/src/types/builder.rs
@@ -25,6 +25,8 @@
 //!   * No type in an intersection can be a supertype of any other type in the intersection (just
 //!     eliminate the supertype from the intersection).
 //!   * An intersection containing two non-overlapping types should simplify to [`Type::Never`].
+use rustc_hash::FxHashSet;
+
 use crate::types::{builtins_symbol_ty, IntersectionType, Type, UnionType};
 use crate::{Db, FxOrderSet};
 
@@ -59,11 +61,11 @@ impl<'db> UnionBuilder<'db> {
                     None
                 };
                 let mut to_add = ty;
-                let mut to_remove = vec![];
+                let mut removals = UnionRemovals::default();
                 for (index, element) in self.elements.iter().enumerate() {
                     if Some(*element) == bool_pair {
                         to_add = builtins_symbol_ty(self.db, "bool");
-                        to_remove.push(index);
+                        removals.push_index(index);
                         // The type we are adding is a BooleanLiteral, which doesn't have any
                         // subtypes. And we just found that the union already contained our
                         // mirror-image BooleanLiteral, so it can't also contain bool or any
@@ -73,30 +75,11 @@ impl<'db> UnionBuilder<'db> {
                     if ty.is_subtype_of(self.db, *element) {
                         return self;
                     } else if element.is_subtype_of(self.db, ty) {
-                        to_remove.push(index);
-                    }
-                }
-                match to_remove[..] {
-                    [] => self.elements.push(to_add),
-                    [index] => self.elements[index] = to_add,
-                    _ => {
-                        let mut retain =
-                            Vec::with_capacity(self.elements.len() - to_remove.len() + 1);
-                        let mut remove_iter = to_remove.iter();
-                        let mut elements_iter = self.elements.iter().enumerate();
-                        'outer: for remove_index in remove_iter.by_ref() {
-                            for (index, element) in elements_iter.by_ref() {
-                                if index == *remove_index {
-                                    continue 'outer;
-                                }
-                                retain.push(*element);
-                            }
-                        }
-                        retain.extend(elements_iter.map(|(_, element)| element));
-                        retain.push(to_add);
-                        self.elements = retain;
+                        removals.push_index(index);
                     }
                 }
+                removals.apply(&mut self);
+                self.elements.push(to_add);
             }
         }
 
@@ -112,6 +95,59 @@ impl<'db> UnionBuilder<'db> {
     }
 }
 
+#[derive(Debug, Default)]
+enum UnionRemovals {
+    #[default]
+    None,
+    Single {
+        removal_index: usize,
+    },
+    Multiple {
+        removal_indices: FxHashSet<usize>,
+    },
+}
+
+impl UnionRemovals {
+    fn push_index(&mut self, index: usize) {
+        match self {
+            Self::None => {
+                *self = Self::Single {
+                    removal_index: index,
+                }
+            }
+            Self::Single {
+                removal_index: element_index,
+            } => {
+                *self = Self::Multiple {
+                    removal_indices: FxHashSet::from_iter([*element_index, index]),
+                }
+            }
+            Self::Multiple {
+                removal_indices: element_indices,
+            } => {
+                element_indices.insert(index);
+            }
+        }
+    }
+
+    fn apply(self, union_builder: &mut UnionBuilder) {
+        match self {
+            Self::None => {}
+            Self::Single { removal_index } => {
+                union_builder.elements.remove(removal_index);
+            }
+            Self::Multiple { removal_indices } => {
+                let existing_elements = &mut union_builder.elements;
+                let mut should_retain = (0..existing_elements.len())
+                    .map(|element_index| !removal_indices.contains(&element_index));
+                union_builder
+                    .elements
+                    .retain(|_| should_retain.next().unwrap());
+            }
+        }
+    }
+}
+
 #[derive(Clone)]
 pub(crate) struct IntersectionBuilder<'db> {
     // Really this builds a union-of-intersections, because we always keep our set-theoretic types
@@ -397,7 +433,7 @@ mod tests {
         assert_eq!(u0, t0);
         assert_eq!(u1, t0);
         assert_eq!(u2.expect_union().elements(&db).as_ref(), &[t0, t2]);
-        assert_eq!(u3.expect_union().elements(&db).as_ref(), &[t0, t2]);
+        assert_eq!(u3.expect_union().elements(&db).as_ref(), &[t2, t0]);
     }
```

</details>

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:331 on 2024-09-20 00:21_

you could also do this with

```suggestion
        assert_eq!(**union.elements(&db), [t0, t1]);
```

(etc. for all the other `.as_ref()`s you added for the `assert_eq!` calls in this file)

---

_@AlexWaygood reviewed on 2024-09-20 00:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-20 06:31_

It would be nice if we could use `SmallVec` here.... except that we can't because of its lifetime variance bug :(

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1003 on 2024-09-20 06:32_

What's the motivation for removing this method? it doesn't seem necessary for the change in this PR

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:80 on 2024-09-20 06:43_

Nit: Some blank lines to group the relevant logic could help readability. It makes it clear to readers that a "new step" begins. Add blank lines before:

* `let mut to_add`
* `match to_remove`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:62 on 2024-09-20 06:54_

I haven't tested this but I think the simplest implementation is:

```rust
                for (index, element) in self.elements.iter().enumerate() {
                    if Some(*ty) == bool_pair {
                        self.elements.push(builtins_symbol_ty(self.db, "bool"));
                        self.elements.swap_remove(index);

                        // The type we are adding is a BooleanLiteral, which doesn't have any
                        // subtypes. And we just found that the union already contained our
                        // mirror-image BooleanLiteral, so it can't also contain bool or any
                        // supertype of bool. Therefore, we are done.
                        return self;;
                    }

                    if ty.is_subtype_of(self.db, *element) {
                        return self;
                    }
                }

                self.elements.retain(|ty| {
                    !ty.is_subtype_of(self.db, *ty)
                });

                self.elements.push(ty);
```

It has the shortcoming that it doesn't place `to_add` into the position of a removed element and that we iterate the elements twice even if there are no elements to remove. But it avoids copying the elements into a new vector, which is roughly the same as doing a `vec.clone()` 

We can preserve the ordering by using `retain_mut`. I think this is my preferred solution.

```rust
                let bool_pair = if let Type::BooleanLiteral(b) = ty {
                    Some(Type::BooleanLiteral(!b))
                } else {
                    None
                };

                for (index, element) in self.elements.iter().copied().enumerate() {
                    if Some(element) == bool_pair {
                        self.elements.push(builtins_symbol_ty(self.db, "bool"));
                        self.elements.swap_remove(index);

                        // The type we are adding is a BooleanLiteral, which doesn't have any
                        // subtypes. And we just found that the union already contained our
                        // mirror-image BooleanLiteral, so it can't also contain bool or any
                        // supertype of bool. Therefore, we are done.
                        return self;
                    }

                    if ty.is_subtype_of(self.db, element) {
                        return self;
                    }
                }

                let mut ty_add = Some(ty);
                self.elements.retain_mut(|element| {
                    if element.is_subtype_of(self.db, ty) {
                        if let Some(to_add) = ty_add.take() {
                            *element = to_add;
                            true
                        } else {
                            false
                        }
                    } else {
                        true
                    }
                });

                if let Some(ty_add) = ty_add {
                    self.elements.push(ty_add);
                }
```


A third version avoiding copying the vec uses retain and is similar to what Alex proposes.


```rust
                let mut to_add = ty;
                let mut to_remove = vec![];

                for (index, element) in self.elements.iter().enumerate() {
                    if Some(*element) == bool_pair {
                        to_add = builtins_symbol_ty(self.db, "bool");
                        to_remove.push(index);
                        // The type we are adding is a BooleanLiteral, which doesn't have any
                        // subtypes. And we just found that the union already contained our
                        // mirror-image BooleanLiteral, so it can't also contain bool or any
                        // supertype of bool. Therefore, we are done.
                        break;
                    }
                    if ty.is_subtype_of(self.db, *element) {
                        return self;
                    } else if element.is_subtype_of(self.db, ty) {
                        to_remove.push(index);
                    }
                }

                self.elements.push(to_add);
                let mut to_remove = to_remove.into_iter();
                if let Some(first_index) = to_remove.next() {
                    // Swaps `first_index` with `to_add`: O(1)
                    self.elements.swap_remove(first_index);
                }

                let mut current_index = 0;
                let mut next_to_remove_index = to_remove.next();

                if next_to_remove_index.is_some() {
                    self.elements.retain(|_ty| {
                        let retain = if Some(current_index) == next_to_remove_index {
                            next_to_remove_index = to_remove.next();
                            false
                        } else {
                            true
                        };
                        current_index += 1;
                        retain
                    })
                }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-20 07:09_

I saw that it's now necessary to do some awkward dereferencing because the salsa accessor returns `&Box<[Type<'db>]`. What I liked to do in the past is to implement my manual accessor:

```rust
    #[return_ref]
	  elements_slice: Box<[Type<'db>]>

    ...


impl <'db> UnionType<'db> [
	fn elements(&self, db: &'db dyn Db) -> &'db [Type<'db>] {
		&*self.elements_slice(db)
	}
```



---

_@MichaReiser approved on 2024-09-20 07:10_

Thanks for following up on this. I think there's room to simplify the implementation, although it comes with trade-offs. It's up to you which version you prefer (yours, Alex, or one proposed by myself) 

---

_@carljm reviewed on 2024-09-20 15:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-20 15:11_

> It would be nice if we could use `SmallVec` here.... except that we can't because of its lifetime variance bug

I feel like `SmallVec` isn't that complicated, and maybe we should just implement our own version of it.

---

_@carljm reviewed on 2024-09-20 15:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1003 on 2024-09-20 15:13_

It's not _necessary_ for the change in this PR, but it's related, because we switch from a set to a boxed array, which means `contains` is now `O(n)` instead of `O(1)`, which means we should avoid using it. And we shouldn't need it, either, because generally we should be looking at subtype/assignability, not type identity.

The only uses of this method are related to `Type::Unbound`, and I want to remove that from the type system entirely.

---

_@MichaReiser reviewed on 2024-09-20 15:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-20 15:18_

An as efficient version as small vec is a bit involved. But let's do this as a separate PR

---

_@MichaReiser reviewed on 2024-09-20 15:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1003 on 2024-09-20 15:19_

Fair. You could also mark it as deprecated and allow its usage. But it's not really important

---

_@carljm reviewed on 2024-09-20 15:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:95 on 2024-09-20 15:34_

Thanks for all the good suggestions! Still evaluating both yours and Micha's, but one note here: the initial simplification you suggest in the suggested change here doesn't work, because it causes the enumeration to start over at 0 each time we re-enter the inner loop. But this was also useful, because it made me realize the tests were inadequate; I've now added a test for subsuming multiple items at once, which fails with that change.

---

_@carljm reviewed on 2024-09-20 15:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:331 on 2024-09-20 15:38_

I think I'll go with Micha's suggestion of a slice-returning accessor to eliminate the need for either of these.

---

_@carljm reviewed on 2024-09-20 15:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:997 on 2024-09-20 15:49_

Implemented the accessor to simplify usage of union elements; thanks for the suggestion!

---

_Comment by @carljm on 2024-09-20 17:22_

Thanks both for all the great suggestions!

I did some benchmarking to validate my performance assumptions. "Removing nothing" is by far the most common case, and "removing one thing" is the majority of the remaining cases, leaving "removing multiple" as a rare edge case. And I did find that de-optimizing the common cases, even in a small way (for example, double iterating the elements) caused a noticeable regression in the benchmark locally. So I want to keep the characteristic of my initial version that it prioritizes doing as little as possible in the remove-none and remove-one cases. In that same vein, I also observed that using a `SmallVec` for `to_remove` is a significant win, since it avoids allocating a heap vector in the remove-one case (an empty vec doesn't allocate, so remove-none was already ok), so I made that change.

All that said, I think it's definitely better to use `.retain` and avoid allocating a new vector in the remove-multiple case.

So the solution I ended up with is very similar in what it actually does to both of your solutions using `.retain`, and the remaining questions are about code structure and readability. I don't find it an improvement in readability to extract a new struct or use an explicit enum for zero/one/multiple cases; it's just a lot more code for the reader to absorb. And I prefer to structure it so that in the remove-one case we don't have to first push and then later `swap_remove`, we can just directly replace the element, so I kept the match over `to_remove` cardinality rather than fully using Micha's version.

Hopefully that all makes sense!

---

_Comment by @carljm on 2024-09-20 17:27_

Oh, and I also added `is_subtype_of` support for `object`, because I thought I needed it in order to write a test for the remove-multiple case. I didn't actually (I could have just use two different literal types being subsumed by their common base type), but by the time I realized that I'd already added it, and it's pretty simple.

---

_Merged by @carljm on 2024-09-20 17:49_

---

_Closed by @carljm on 2024-09-20 17:49_

---

_Branch deleted on 2024-09-20 17:49_

---
