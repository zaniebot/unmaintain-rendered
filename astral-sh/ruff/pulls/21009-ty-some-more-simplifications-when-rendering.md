```yaml
number: 21009
title: "[ty] Some more simplifications when rendering constraint sets"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/constraint-display
created_at: 2025-10-21T01:52:20Z
updated_at: 2025-10-22T17:39:10Z
url: https://github.com/astral-sh/ruff/pull/21009
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Some more simplifications when rendering constraint sets

---

_Pull request opened by @dcreager on 2025-10-21 01:52_

This PR adds another useful simplification when rendering constraint sets: `T = int` instead of `T = int ∧ T ≠ str`. (The "smaller" constraint `T = int` implies the "larger" constraint `T ≠ str`. Constraint set clauses are intersections, and if one constraint in a clause implies another, we can throw away the "larger" constraint.)

While we're here, we also normalize the bounds of a constraint, so that we equate e.g. `T ≤ int | str` with `T ≤ str | int`, and change the ordering of BDD variables so that all constraints with the same typevar are ordered adjacent to each other.

Lastly, we also add a new `display_graph` helper method that prints out the full graph structure of a BDD.

---

_Comment by @dcreager on 2025-10-21 01:52_

Pushing this up for initial CI checks and to checkpoint my work. Will add some mdtests highlighting the rendering change tomorrow

---

_Label `internal` added by @dcreager on 2025-10-21 01:53_

---

_Label `ty` added by @dcreager on 2025-10-21 01:53_

---

_Comment by @github-actions[bot] on 2025-10-21 01:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 01:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:624 on 2025-10-21 13:57_

Before this PR, this rendered as

```
((T@_ = bool) ∧ (U@_ = bool) ∧ (U@_ ≠ str)) ∨ ((T@_ = bool) ∧ (U@_ = str)) ∨ ((T@_ = str) ∧ (T@_ ≠ bool) ∧ (U@_ = bool) ∧ (U@_ ≠ str)) ∨ (T@_ = str)
```

and the `display_graph` rendering for this BDD is

```
  (T@_ = str)
  ┡━₁ (U@_ = str)
  │   ┡━₁ always
  │   └─₀ (U@_ = bool)
  │       ┡━₁ always
  │       └─₀ never
  └─₀ (T@_ = bool)
      ┡━₁ (U@_ = str)
      │   ┡━₁ always
      │   └─₀ (U@_ = bool)
      │       ┡━₁ always
      │       └─₀ never
      └─₀ never
```

---

_@dcreager reviewed on 2025-10-21 13:58_

---

_Marked ready for review by @dcreager on 2025-10-21 14:47_

---

_Review requested from @carljm by @dcreager on 2025-10-21 14:47_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-21 14:47_

---

_Review requested from @sharkdp by @dcreager on 2025-10-21 14:47_

---

_@dcreager reviewed on 2025-10-21 14:56_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1285 on 2025-10-21 14:56_

This is the big one that lets us simplify `T = int ∧ T ≠ str` to `T = int` when displaying.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1371 on 2025-10-21 15:35_

```suggestion
    /// want to remove the larger one and keep the smaller one.)
    ///
    /// Returns a boolean that indicates whether any simplifications were made.
    fn simplify(&mut self, db: &'db dyn Db) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:615 on 2025-10-21 15:35_

nit: I find all these functions being called `_` a bit odd for the tests in this file specifically, because my brain keeps parsing `@_` as a single glpyh -- the underscore looks like a continuation of the `@` to me 😆 would it be possible to call the function `f` (or really, anything that doesn't start with `_`)?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:315 on 2025-10-21 15:39_

Should this be a `std::cmp::Ord` impl? It feels unidiomatic to have a `cmp` function on a type but not implement `Ord`.

Hmm, but maybe this doesn't produce results that are consistent with the `PartialEq` and `Eq` implementations that are automatically derived as a result of this being `salsa::interned`? It looks like it's possible for `t1.cmp(t2)` to return `Ordering::Equal` but not for them to compare equal using `==`. In which case this shouldn't be an `Ord` implementation, but it should probably be renamed to clearly indicate that it doesn't have anything to do with the `Ord` trait

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:342 on 2025-10-21 15:40_

```suggestion
    /// Return `true` if this constraint can be safely simplified out of a constraint set
    /// due to the presence of `other` in the constraint set.
    fn implies(self, db: &'db dyn Db, other: Self) -> bool {
        other.contains(db, self)
    }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:797 on 2025-10-21 15:41_

Would you also be able to add a doc-comment that describes what this method does, for other readers (and future debuggers!) of the code?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1254 on 2025-10-21 15:41_

Could you possibly add a doc-comment to this method?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/definition.rs`:31 on 2025-10-21 15:43_

For lots of other Salsa-tracked and Salsa-interned structs, we have these notes included in the doc-comment:

https://github.com/astral-sh/ruff/blob/e1cada1ec30d289acb00390058fab8bce765836e/crates/ty_python_semantic/src/types.rs#L513-L521

It might be worth doing similarly here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:607 on 2025-10-21 15:44_

I think I might have stumbled across this terminology in one of your PRs before 😅

When you say "render" here, are you just talking about "display rendering"? I.e., this "rendering" should have no impact on semantics?

---

_@AlexWaygood reviewed on 2025-10-21 15:53_

thank you!

---

_@dcreager reviewed on 2025-10-21 16:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:315 on 2025-10-21 16:33_

It originally was an `Ord` impl (via derive), but it now needs a `db` parameter since it has to pull out fields from the salsa-interned struct. That's also why there's churn replacing `a > b` calls with `a.cmp(db, b) == Ordering::Greater`, which I don't love the spelling of.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:607 on 2025-10-21 16:40_

Yeah, I mean "render" in the sense of "create a visual representation of", like you'd render an image. It doesn't change the semantics at all, just how the constraint sets are displayed.  I can change this to "display"

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:615 on 2025-10-21 16:42_

Done. I did this only locally in this section, since doing that to the rest of the file would be a noisy diff. I can tackle that as a follow-on.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/definition.rs`:31 on 2025-10-21 16:43_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:342 on 2025-10-21 16:44_

I went with a slightly different wording, to also describe what implies _means_, not just where it's used

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:797 on 2025-10-21 16:48_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1254 on 2025-10-21 16:48_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:315 on 2025-10-21 17:03_

I replaced this with a new `ConstraintOrdering` wrapper, which holds onto the `db`, and which I can implement `PartialOrd` for. That also gives us a nice place to document what ordering we chose and why. lmkwyt

---

_@dcreager reviewed on 2025-10-21 17:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:319 on 2025-10-22 06:53_

```suggestion
    /// Returns whether this constraint contains another — i.e., whether every type that
```

Or 

```suggestion
    /// Returns whether this constraint implies another — i.e., whether every type that
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1272 on 2025-10-22 06:58_

```suggestion
    /// Returns whether this constraint contains another — i.e., whether every type that
```

Or 

```suggestion
    /// Returns whether this constraint implies another — i.e., whether every type that
```

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-10-22 13:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 13:47_

If that's the case, I recommend implementing `Ord` for `ConstraintOrdering` 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:827 on 2025-10-22 13:48_

Nit: Would it be difficult to make this a doctest or have a snapshot unit test for this very example (which would ensure it doesn't get outdated)

---

_@MichaReiser approved on 2025-10-22 13:48_

I'm a bit out of my comfort zone here, so please forgive any dumb questions 😅 

> This PR adds another useful simplification when rendering constraint sets: T = int instead of T = int ∧ T ≠ str. (The "smaller" constraint T = int implies the "larger" constraint T ≠ str. Constraint set clauses are intersections, and if one constraint in a clause implies another, we can throw away the "larger" constraint.)

What's the reason why we only do this during rendering and don't apply this as a general simplification? 

---

_@AlexWaygood reviewed on 2025-10-22 13:50_

---

_@AlexWaygood reviewed on 2025-10-22 14:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:607 on 2025-10-22 14:01_

I sort of have the same question as Micha in https://github.com/astral-sh/ruff/pull/21009#pullrequestreview-3365893127 in that case. If we do this simplification only when displaying these constraints, isn't there a risk that we'll be seeing something different to what is "actually happening behind the scenes" when we try to debug these constraints? The simplification logic isn't trivial! It feels like it would be safer to always apply these simplifications, so that we can be sure that what we see when we debug these constraints is actually what's being used in our subtyping/assignability logic by ty in production

---

_@AlexWaygood reviewed on 2025-10-22 14:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 14:17_

yes -- something like this feels like it would express the invariants here a bit better?

```diff
diff --git a/crates/ty_python_semantic/src/types/constraints.rs b/crates/ty_python_semantic/src/types/constraints.rs
index f736452116..18dc881657 100644
--- a/crates/ty_python_semantic/src/types/constraints.rs
+++ b/crates/ty_python_semantic/src/types/constraints.rs
@@ -405,27 +405,36 @@ impl<'db> ConstrainedTypeVar<'db> {
 /// simplifications that we perform that operate on constraints with the same typevar, and this
 /// ensures that we can find all candidate simplifications more easily.
 #[derive(Clone, Copy)]
-struct ConstraintOrdering<'db>(&'db dyn Db, ConstrainedTypeVar<'db>);
+struct OrderedTypeVar<'db>(&'db dyn Db, ConstrainedTypeVar<'db>);
 
-impl<'db> PartialEq<ConstrainedTypeVar<'db>> for ConstraintOrdering<'db> {
-    fn eq(&self, other_constraint: &ConstrainedTypeVar<'db>) -> bool {
-        let ConstraintOrdering(_, self_constraint) = *self;
-        self_constraint == *other_constraint
+impl PartialEq for OrderedTypeVar<'_> {
+    fn eq(&self, other: &Self) -> bool {
+        let OrderedTypeVar(_, self_constraint) = *self;
+        let OrderedTypeVar(_, other_constraint) = *other;
+        self_constraint == other_constraint
     }
 }
 
-impl<'db> PartialOrd<ConstrainedTypeVar<'db>> for ConstraintOrdering<'db> {
-    fn partial_cmp(&self, other_constraint: &ConstrainedTypeVar<'db>) -> Option<Ordering> {
-        let ConstraintOrdering(db, self_constraint) = *self;
-        if self_constraint == *other_constraint {
-            return Some(Ordering::Equal);
+impl Eq for OrderedTypeVar<'_> {}
+
+impl Ord for OrderedTypeVar<'_> {
+    fn cmp(&self, other: &Self) -> Ordering {
+        let OrderedTypeVar(db, self_constraint) = *self;
+        let OrderedTypeVar(_, other_constraint) = *other;
+
+        if self_constraint == other_constraint {
+            return Ordering::Equal;
         }
-        Some(
-            self_constraint
-                .typevar(db)
-                .cmp(&other_constraint.typevar(db))
-                .then_with(|| self_constraint.as_id().cmp(&other_constraint.as_id())),
-        )
+        self_constraint
+            .typevar(db)
+            .cmp(&other_constraint.typevar(db))
+            .then_with(|| self_constraint.as_id().cmp(&other_constraint.as_id()))
+    }
+}
+
+impl PartialOrd for OrderedTypeVar<'_> {
+    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
+        Some(self.cmp(other))
     }
 }
 
@@ -460,11 +469,11 @@ impl<'db> Node<'db> {
         if_false: Node<'db>,
     ) -> Self {
         debug_assert!((if_true.root_constraint(db)).is_none_or(|root_constraint| {
-            ConstraintOrdering(db, root_constraint) > constraint
+            OrderedTypeVar(db, root_constraint) > OrderedTypeVar(db, constraint)
         }));
         debug_assert!(
             (if_false.root_constraint(db)).is_none_or(|root_constraint| {
-                ConstraintOrdering(db, root_constraint) > constraint
+                OrderedTypeVar(db, root_constraint) > OrderedTypeVar(db, constraint)
             })
         );
         if if_true == if_false {
@@ -925,26 +934,25 @@ impl<'db> InteriorNode<'db> {
     fn or(self, db: &'db dyn Db, other: Self) -> Node<'db> {
         let self_constraint = self.constraint(db);
         let other_constraint = other.constraint(db);
-        match ConstraintOrdering(db, self_constraint).partial_cmp(&other_constraint) {
-            Some(Ordering::Equal) => Node::new(
+        match OrderedTypeVar(db, self_constraint).cmp(&OrderedTypeVar(db, other_constraint)) {
+            Ordering::Equal => Node::new(
                 db,
                 self_constraint,
                 self.if_true(db).or(db, other.if_true(db)),
                 self.if_false(db).or(db, other.if_false(db)),
             ),
-            Some(Ordering::Less) => Node::new(
+            Ordering::Less => Node::new(
                 db,
                 self_constraint,
                 self.if_true(db).or(db, Node::Interior(other)),
                 self.if_false(db).or(db, Node::Interior(other)),
             ),
-            Some(Ordering::Greater) => Node::new(
+            Ordering::Greater => Node::new(
                 db,
                 other_constraint,
                 Node::Interior(self).or(db, other.if_true(db)),
                 Node::Interior(self).or(db, other.if_false(db)),
             ),
-            None => unreachable!("constraints should always be comparable"),
         }
     }
 
@@ -952,26 +960,25 @@ impl<'db> InteriorNode<'db> {
     fn and(self, db: &'db dyn Db, other: Self) -> Node<'db> {
         let self_constraint = self.constraint(db);
         let other_constraint = other.constraint(db);
-        match ConstraintOrdering(db, self_constraint).partial_cmp(&other_constraint) {
-            Some(Ordering::Equal) => Node::new(
+        match OrderedTypeVar(db, self_constraint).cmp(&OrderedTypeVar(db, other_constraint)) {
+            Ordering::Equal => Node::new(
                 db,
                 self_constraint,
                 self.if_true(db).and(db, other.if_true(db)),
                 self.if_false(db).and(db, other.if_false(db)),
             ),
-            Some(Ordering::Less) => Node::new(
+            Ordering::Less => Node::new(
                 db,
                 self_constraint,
                 self.if_true(db).and(db, Node::Interior(other)),
                 self.if_false(db).and(db, Node::Interior(other)),
             ),
-            Some(Ordering::Greater) => Node::new(
+            Ordering::Greater => Node::new(
                 db,
                 other_constraint,
                 Node::Interior(self).and(db, other.if_true(db)),
                 Node::Interior(self).and(db, other.if_false(db)),
             ),
-            None => unreachable!("constraints should always be comparable"),
         }
     }
 
@@ -979,26 +986,25 @@ impl<'db> InteriorNode<'db> {
     fn iff(self, db: &'db dyn Db, other: Self) -> Node<'db> {
         let self_constraint = self.constraint(db);
         let other_constraint = other.constraint(db);
-        match ConstraintOrdering(db, self_constraint).partial_cmp(&other_constraint) {
-            Some(Ordering::Equal) => Node::new(
+        match OrderedTypeVar(db, self_constraint).cmp(&OrderedTypeVar(db, other_constraint)) {
+            Ordering::Equal => Node::new(
                 db,
                 self_constraint,
                 self.if_true(db).iff(db, other.if_true(db)),
                 self.if_false(db).iff(db, other.if_false(db)),
             ),
-            Some(Ordering::Less) => Node::new(
+            Ordering::Less => Node::new(
                 db,
                 self_constraint,
                 self.if_true(db).iff(db, Node::Interior(other)),
                 self.if_false(db).iff(db, Node::Interior(other)),
             ),
-            Some(Ordering::Greater) => Node::new(
+            Ordering::Greater => Node::new(
                 db,
                 other_constraint,
                 Node::Interior(self).iff(db, other.if_true(db)),
                 Node::Interior(self).iff(db, other.if_false(db)),
             ),
-            None => unreachable!("constraints should always be comparable"),
         }
     }
 
@@ -1012,7 +1018,7 @@ impl<'db> InteriorNode<'db> {
         // point in the BDD where the assignment can no longer affect the result,
         // and we can return early.
         let self_constraint = self.constraint(db);
-        if ConstraintOrdering(db, assignment.constraint()) < self_constraint {
+        if OrderedTypeVar(db, assignment.constraint()) < OrderedTypeVar(db, self_constraint) {
             return (Node::Interior(self), false);
         }
```

---

_@AlexWaygood reviewed on 2025-10-22 14:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:615 on 2025-10-22 14:17_

TYVM!

---

_@dcreager reviewed on 2025-10-22 14:29_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 14:29_

This is exactly what I had originally! https://github.com/astral-sh/ruff/pull/21009/commits/21f75eb6e02ba22b3c260fa27cbbedce68ed360d is where I moved it to the current representation. I didn't love how I had to wrap both arguments (and pass in `db` in both places) to get the right comparison behavior. Though I agree that I _also_ don't love needing the `None` catch-all in those match statements...

---

_@AlexWaygood reviewed on 2025-10-22 14:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 14:36_

> I didn't love how I had to wrap both arguments (and pass in `db` in both places) to get the right comparison behavior

I think it's worth it to have this kind of invariant checked by the compiler rather than having to have these `unreachable!()` branches everywhere :-)

---

_@AlexWaygood reviewed on 2025-10-22 14:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 14:37_

it's also a similar pattern that we use for comparing AST nodes -- see https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/comparable.rs

---

_@MichaReiser reviewed on 2025-10-22 15:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 15:07_

Oh, I see now what the problem is. It's because you can't implement `Ord` for `ConstraintOrdering`. 

A pattern we use else where is that a type has an `ordering` method, e.g. see here https://github.com/astral-sh/ruff/blob/61a49c89eb809a238b01616b0a50a0f785702891/crates/ruff_text_size/src/range.rs#L338-L346 or a `sort_key` method (see here https://github.com/astral-sh/ruff/blob/e64d77278830954a323d227e8f9f714c1d0e4c57/crates/ruff_db/src/diagnostic/mod.rs#L292-L297) where the returned type implements `Ord`. 



Which I think is what Alex suggests above except that we tend to have methods that return those types.

---

_@dcreager reviewed on 2025-10-22 15:15_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:607 on 2025-10-22 15:15_

The issue is that we're talking about two completely different representations, that are used for different purposes.

As an example, one of the display simplifications that this PR adds would simplify `T = int ∧ T ≠ str` into `T = int`. If you know that `T` is an `int`, it doesn't add any information to say that it is also not a `str`.

We could create a BDD for `T = int ∧ T ≠ str`. Depending on what ordering Salsa assigns IDs, the BDD structure you get is either

```
(T = int)
  ┡━₁ (T = str)
  │   ┡━₁ never
  │   └─₀ always
  └─₀ never
```

or

```
(T = str)
  ┡━₁ never
  └─₀ (T = int)
      ┡━₁ always
      └─₀ never
```

The equivalent transformation that you'd perform on the BDD would be something like "for any path through the BDD where `T = int` is true, you don't need to check `T = str`, because it must always be false".

We actually do already [perform that transformation](https://github.com/astral-sh/ruff/blob/81c1d36088c1533282dae787e8ff3b470a18dbb4/crates/ty_python_semantic/src/types/constraints.rs#L1088-L1095)! But it's not necessarily going to _simplify_ the BDD. It might actually make it more complex! For instance, if the BDD is checking other stuff as well, there might be other reasons we need a node that checks `T = str`. Since every node must have two outgoing edges, it might not be possible to remove it even if we wanted to.

(And going further into the weeds as an aside, in the BDD structure, we actually don't really ever want to throw information away, since it makes it easier to compare two BDDs without having to know in advance which particular constraints they check. The better analogue of the transformation we perform is "it's impossible for `T = int` to be true and `T = str` to be false, so _when comparing two BDDs_ ignore any path with those assignments".)

So even though we have performed these transformations on the BDD already, we might still end up with constraints in the rendered formula that are redundant. And so I wanted to be able to print out the simplest formula that faithfully captures the BDD.

> that we can be sure that what we see when we debug these constraints is actually what's being used in our subtyping/assignability logic by ty in production

The other possibility here is that we would avoid performing any display substitutions at all. Then the formula that we print out would be a more direct representation of the BDD structure. But I was finding that very confusing, because even simple cases end up looking more complex than they need to. (A prime example is that half of the time `x ∨ y` will display as `x ∨ y`, and half the time as `(x ∧ ¬y) ∨ y`.)

---

_@dcreager reviewed on 2025-10-22 15:16_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:827 on 2025-10-22 15:16_

Oh I like that! I'll give it a shot

---

_@AlexWaygood reviewed on 2025-10-22 15:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:607 on 2025-10-22 15:38_

Thanks for the explanation. That makes sense to me :-)

---

_@AlexWaygood approved on 2025-10-22 15:40_

---

_@MichaReiser reviewed on 2025-10-22 15:40_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:331 on 2025-10-22 15:40_

Clever :)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:974 on 2025-10-22 15:40_

I like the `ordering` method suggestion best, went with that!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:827 on 2025-10-22 15:42_

This looks doable, but doctests cannot access `pub(crate)` symbols, so it would require marking quite a lot of internals as `pub`. Do you still consider it worthwhile given that?

---

_@dcreager reviewed on 2025-10-22 15:42_

---

_@dcreager reviewed on 2025-10-22 15:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:827 on 2025-10-22 15:48_

Ah I missed your "snapshot unit test" idea. That would not have the same vis problem. Will try

---

_@dcreager reviewed on 2025-10-22 15:57_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:827 on 2025-10-22 15:57_

Done!

---

_Comment by @github-actions[bot] on 2025-10-22 16:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @dcreager on 2025-10-22 17:38_

---

_Closed by @dcreager on 2025-10-22 17:38_

---

_Branch deleted on 2025-10-22 17:39_

---
