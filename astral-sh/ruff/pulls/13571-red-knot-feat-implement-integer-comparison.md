```yaml
number: 13571
title: "[red-knot] feat: implement integer comparison"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/int-comparison
created_at: 2024-09-30T15:16:25Z
updated_at: 2024-10-04T17:40:59Z
url: https://github.com/astral-sh/ruff/pull/13571
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] feat: implement integer comparison

---

_Pull request opened by @Slyces on 2024-09-30 15:16_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements the comparison operator for `[Type::IntLiteral]` and `[Type::BoolLiteral]` (as an artifact of special handling of `True` and `False` in python).
Sets the framework to implement more comparison for types known at static time (e.g. `BoolLiteral`, `StringLiteral`), allowing us to only implement cases of the triplet `<left> Type`, `<right> Type`, `CmpOp`.
Contributes to astral-sh/ty#244 (without checking off an item yet).

## Implementation Details

I couldn't avoid allocating a `Vec<Type>` for the first evaluation of initial expressions before iterating over pairs of expression types (`.windows(2)`). Currently, I couldn't use `.windows` on iterators. This seems to be possible using some crates or an experimental API. If anyone has an idea on how to achieve this without allocation, I'd love to hear it.

## Test Plan

- Added a test for the comparison of literals that should include most cases of note.
- Added a test for the comparison of int instances

Please note that the cases do not cover 100% of the branches as there are many and the current testing strategy with variables make this fairly confusing once we have too many in one test.

I'll probably open a separate PR with a candidate util function for cases like this (checking the public type of many separate statements).


---

_Review requested from @carljm by @Slyces on 2024-09-30 15:16_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-30 15:16_

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-30 15:16_

---

_@AlexWaygood reviewed on 2024-09-30 16:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2483 on 2024-09-30 16:58_

I haven't looked at the PR as a whole yet, but you might be able to use [`itertools::Itertools::tuple_windows`](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.tuple_windows) here. The method can be a bit dangerous to use, since it clones the iterator elements -- but `Type` instances should be very cheap to clone, so I think that's _probably_ a trade worth making to avoid allocating the `Vec`. You might have to add `itertools` as a dependency of the crate

---

_Comment by @carljm on 2024-09-30 17:49_

> I'll probably open a separate PR with a candidate util function for cases like this (checking the public type of many separate statements).

You can do this, but note that I'm also working on a test framework (see discussion in https://github.com/astral-sh/ruff/issues/11664 ) which should also make this a lot less verbose. So it may not be worth investing a lot into test utils like this one in the meantime.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2409 on 2024-09-30 17:53_

It's safe to return `False` for an `is` comparison or `True` for `is not`, if the integers are not equal, but we should never return `True` for an `is` comparison or `False` for an `is not` comparison, only `bool`. Integer identity is not reliable in Python and not part of the language spec.

```
>>> x = 1024
>>> y = 1024
>>> x is y
False
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2483 on 2024-09-30 17:54_

I was going to suggest an approach that I think is pretty similar to what `tuple_windows` is doing internally, but I think just using `tuple_windows` makes more sense :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2500 on 2024-09-30 18:37_

For better or worse, Python supports returning arbitrary types from comparison dunder methods:

```
>>> class C:
...     def __lt__(self, other):
...             return "foo"
...
>>> C() < 2
'foo'
```

So the final inferred type here is not necessarily a boolean, it can be any type.

The short-circuiting semantics for chained comparisons fall out directly from the equivalence to a sequence of and-ed conditions specified in the quoted language reference spec above, as seen in this more complex example:

```
>>> class C:
...     def __init__(self, val):
...             self.val = val
...     def __lt__(self, other):
...             if self.val < other.val:
...                     return self
...             return other
...     def __bool__(self):
...             print(f"bool of {self.val}")
...             return bool(self.val)
...     def __repr__(self):
...             return f"C({self.val})"
...
>>> C(1) < C(2)
C(1)
>>> C(1) < C(2) < C(3)
bool of 1
C(2)
>>> C(1) < C(2) and C(2) < C(3)
bool of 1
C(2)
```

So I think it is correct that we call `Type::bool` here to get the `Truthiness` of the inferred type of each comparison, but only on comparisons other than the last one (and thus never, in the case of non-chained comparisons), in order to determine when to short-circuit. But the type we end up inferring should not be the `into_type` of the `Truthiness`, it should be the original type result from the comparison.

It may make sense to try to actually extract and reuse the logic from our boolean `and` comparison, once we have here an iterator of types resulting from the comparisons that should be anded together? Or it may not, up to your judgment on that.

It will be a bit difficult to write tests showing that we're doing this correctly without supporting at least some form of comparison on `Type::Instance` (via looking up the return type of a comparison dunder method), since no built-in types return non-bools from comparison methods. It may make most sense to go ahead and implement the full dunder-method lookup logic (described at https://docs.python.org/3/reference/datamodel.html#object.__lt__ ), but if you can find a reasonable subset to implement with a TODO for the rest, that's fine too.

(Note also that we should probably avoid tests showing implementing `__eq__` with a non-bool return, since `object` in typeshed actually implements `__eq__` with bool return, meaning that when we implement incompatible override checks, we would error on such a definition. That's why my example above uses `__lt__` instead.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 18:39_

This should emit a diagnostic and return `Unknown`, not `BooleanLiteral(false)`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2428 on 2024-09-30 19:39_

I don't think we need this TODO; we may end up consolidating some logic around e.g. treating `IntLiteral` as `int` when we can't handle it as a special case, but it's not clear if we will, and there's no missing or incomplete feature here we need to remember.

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2433 on 2024-09-30 19:41_

I think the better approach here is to just recursively call `self.infer_binary_type_comparison` (as you do in some cases above) with `builtins_symbol_ty(self.db, "int").to_instance(self.db)` in place of the `IntLiteral` type, and then we should fall into the normal Instance vs Instance handling based on typeshed.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2446 on 2024-09-30 19:45_

I think if we aren't going to do the full `Type::Instance` handling in this PR, we should just leave this case out entirely, as it will be entirely replaced by generic `Type::Instance` handling.

---

_@carljm requested changes on 2024-09-30 19:46_

Thank you!

---

_@Slyces reviewed on 2024-09-30 20:11_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2433 on 2024-09-30 20:11_

That's both clever and obvious, I'm not sure why I missed it. Thanks 🙏

---

_@Slyces reviewed on 2024-09-30 20:12_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2446 on 2024-09-30 20:12_

I guess I was seeing the special case of (int, IntLiteral) as a part of IntLiteral's logic, but it does indeed fit better as a generic part of instances 

---

_@Slyces reviewed on 2024-09-30 20:14_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 20:14_

I don't have a lot of experience coding diagnostics - do we already have an example of the diagnostic I need to emit here? 
Thank you 🙂

---

_@carljm reviewed on 2024-09-30 20:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 20:22_

Use `self.add_diagnostic` method to emit it. At this stage we aren't super picky about error codes or diagnostic messages yet; that will get cleaned up later. For this case I'd use the error code `"operator-unsupported"` and the diagnostic message `Operator "in" not supported for types '{}' and '{}'.`. (Where you can use `Type::display` method to fill in the type display names.) You can look at existing uses of `add_diagnostic` for more examples.

---

_Label `red-knot` added by @carljm on 2024-09-30 21:02_

---

_@JelleZijlstra reviewed on 2024-09-30 21:09_

---

_Review comment by @JelleZijlstra on `crates/red_knot_python_semantic/src/types/infer.rs`:2500 on 2024-09-30 21:09_

Note that the `in`/`not in` operations always return bool though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2500 on 2024-09-30 21:18_

Yes; also `is` and `not is`. But those are cases that will be reflected in the function that actually handles each binary comparison; it doesn't change the implications for this code, which needs to handle the results from any comparison.

---

_@carljm reviewed on 2024-09-30 21:18_

---

_@Slyces reviewed on 2024-10-02 20:40_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2500 on 2024-10-02 20:40_

Hi guys, thank you so much for this very complete comment on the matter.

> It may make sense to try to actually extract and reuse the logic from our boolean and comparison, once we have here an iterator of types resulting from the comparisons that should be anded together? Or it may not, up to your judgment on that.

I actually started my PR with this approach. I realised that doing so (with my current rust mastery) led to additional allocations, and I know that a lot of effort went into avoiding allocations in the boolean PR, so I scrapped this approach. I think I will code my PR as a separate logic for now, and happily take suggestions on a direction to share the logic if one seems possible (either by accepting more allocations, or pointing me toward rust concepts that could make having both efficiency and code sharing possible).

> It will be a bit difficult to write tests showing that we're doing this correctly without supporting at least some form of comparison on Type::Instance (via looking up the return type of a comparison dunder method), since no built-in types return non-bools from comparison methods. It may make most sense to go ahead and implement the full dunder-method lookup logic (described at https://docs.python.org/3/reference/datamodel.html#object.__lt__ ), but if you can find a reasonable subset to implement with a TODO for the rest, that's fine too.

I think understanding thoroughly what to do here and writing tests will take me some time, but I'm happy to try. I will pass the PR as draft in the meantime.

---

_Comment by @Slyces on 2024-10-02 21:22_

Just dropping this here for future reference (@AlexWaygood sent a similar article for astral-sh/ruff#13590):
- https://snarky.ca/unravelling-rich-comparison-operators/: `==, !=, <=, >=, <, >`
- https://snarky.ca/unravelling-is-and-is-not/: `is, is not`
- Couldn't find one for `in, not in`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3149 on 2024-10-03 15:28_

```suggestion
    // TODO: `object.__ne__` will call `__eq__` if `__ne__` is not defined
```

---

_@AlexWaygood reviewed on 2024-10-03 15:28_

---

_Comment by @Slyces on 2024-10-03 15:41_

I think I have covered all comments (to my knowledge) except the `tuple_windows`, I'll try to give more details on why I couldn't get that to work with code comments.

---

_@Slyces reviewed on 2024-10-03 15:45_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2503 on 2024-10-03 15:45_

Using `.tuple_windows::<(Type<'db>, Type<'db>)>()` creates some problems for me
- Above, `line 2500`, we borrow `*self` as mutable to perform the `infer_expression`
- Then below, `line 2507`, we borrow again `self` (at the minimum we need to borrow `self.db`)

I couldn't do better than that. I could work around some other borrows (remove `self` from some functions that don't need it, store diagnostics in a `Vec` to emit later, ...) but I'm not sure how to get out of the borrow issue explained above.

As always, suggestions welcome!

---

_Review requested from @carljm by @Slyces on 2024-10-03 15:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2578 on 2024-10-03 15:48_

we added a helper method for this recently:

https://github.com/astral-sh/ruff/blob/cc1f766622bd27c24e47362503f44f8545710c6f/crates/red_knot_python_semantic/src/types.rs#L392-L394

```suggestion
                Type::builtin_int_instance(self.db),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2585 on 2024-10-03 15:49_

```suggestion
                Type::builtin_int_instance(self.db),
```

---

_Comment by @github-actions[bot] on 2024-10-03 15:54_

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

_@AlexWaygood reviewed on 2024-10-03 15:58_

---

_@AlexWaygood reviewed on 2024-10-03 15:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2578 on 2024-10-03 15:59_

You could consider adding a `Type::builtin_bool_instance` method as part of this PR as well, if you like. But definitely not mandatory :-)

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2578 on 2024-10-03 16:02_

I want to make a PR that would provide things around builtins as they seem to proliferate recently. I'll probably start working on it as soon as this one is done.

I would try to:
- Make all builtins nice to create (probably with an enum)
- On instance creation, check if that instance is a builtin instance

---

_@Slyces reviewed on 2024-10-03 16:02_

---

_@AlexWaygood reviewed on 2024-10-03 16:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2578 on 2024-10-03 16:06_

Interesting... I agree that the current situation isn't _great_, but I don't know that an enum is definitely the way to go. There's a lot of builtins; it might be a pretty big enum if you want to have them all there! And some builtins we are just going to need to access a lot more than others, so it does make sense to treat different builtins differently, I think. But anyway, definitely curious to see what you come up with :-)

---

_@AlexWaygood reviewed on 2024-10-03 16:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2503 on 2024-10-03 16:31_

It seems like we have to iterate over all the comparators twice, because we need to:
1. Infer expression types for all sub-nodes inside the `ast::Compare`, but
2. Make sure we respect the fact that Python's semantics are such that the comparison might short-circuit

Given this, I think we can avoid the intermediate allocation here by inferring all the types in a separate loop prior to entering this loop, and then simply looking up the already-inferred types from inside this loop. Something like this?

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -28,6 +28,7 @@
 //! definitions once the rest of the types in the scope have been inferred.
 use std::num::NonZeroU32;
 
+use itertools::Itertools;
 use ruff_db::files::File;
 use ruff_db::parsed::parsed_module;
 use ruff_python_ast::{self as ast, AnyNodeRef, ExprContext, UnaryOp};
@@ -2484,6 +2485,11 @@ impl<'db> TypeInferenceBuilder<'db> {
             comparators,
         } = compare;
 
+        self.infer_expression(left);
+        for expr in comparators {
+            self.infer_expression(expr);
+        }
+
         // https://docs.python.org/3/reference/expressions.html#comparisons
         // > Formally, if `a, b, c, …, y, z` are expressions and `op1, op2, …, opN` are comparison
         // > operators, then `a op1 b op2 c ... y opN z` is equivalent to a `op1 b and b op2 c and
@@ -2496,14 +2502,17 @@ impl<'db> TypeInferenceBuilder<'db> {
             ast::BoolOp::And,
             std::iter::once(left.as_ref())
                 .chain(comparators.as_ref().iter())
-                // Evaluate expressions before iterating through pairs with `windows`
-                .map(|expr| self.infer_expression(expr))
-                .collect::<Vec<_>>()
-                .windows(2)
-                //.tuple_windows(2)
+                .tuple_windows::<(_, _)>()
                 .zip(ops.iter())
-                .map(|(pair, op)| {
-                    let (left_ty, right_ty) = (pair[0], pair[1]);
+                .map(|((left, right), op)| {
+                    let left_ty = self
+                        .types
+                        .expression_ty(left.scoped_ast_id(self.db, self.scope));
+
+                    let right_ty = self
+                        .types
+                        .expression_ty(right.scoped_ast_id(self.db, self.scope));
+
                     self.infer_binary_type_comparison(left_ty, *op, right_ty)
                         .unwrap_or_else(|| {
                             // Handle unsupported operators (diagnostic, `bool`/`Unknown` outcome)
```

---

_@Slyces reviewed on 2024-10-03 17:12_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2503 on 2024-10-03 17:12_

Are the inferred types cached somehow? Or are we doing the work of inference twice?

---

_@AlexWaygood reviewed on 2024-10-03 18:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2503 on 2024-10-03 18:01_

> Are the inferred types cached somehow?

Yup! In fact, there are various places where the code will panic if it detects that it's tried to infer the type of the same expression twice.

`self.infer_expression()` does this to store the type of the expression in the cache:

https://github.com/astral-sh/ruff/blob/4aefe523938f7176be0bcaa03b6f126c8ae783fb/crates/red_knot_python_semantic/src/types/infer.rs#L1694-L1698

And then `self.types.expression_ty()` would simply look things up in the cache again:

https://github.com/astral-sh/ruff/blob/4aefe523938f7176be0bcaa03b6f126c8ae783fb/crates/red_knot_python_semantic/src/types/infer.rs#L190-L192

---

_Comment by @brettcannon on 2024-10-03 18:22_

> * Couldn't find one for `in, not in`

https://snarky.ca/unravelling-membership-testing/

All the blog posts can be found under https://snarky.ca/tag/syntactic-sugar/ . If you want to look the posts up by syntax, https://github.com/brettcannon/desugar/blob/main/README.md is a good way to do that.

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2503 on 2024-10-03 19:45_

Amazing, this solved my problems, thank you!!

---

_@Slyces reviewed on 2024-10-03 19:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2449 on 2024-10-04 16:32_

This doesn't need to be generic over `Into<Type<'db>>`, it can just take an `IntoIterator` over `Type<'db>`. This change compiles and passes all tests:

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -2446,10 +2446,10 @@ impl<'db> TypeInferenceBuilder<'db> {
     /// Computes the output of a chain of (one) boolean operation, consuming as input an iterator
     /// of types. The iterator is consumed even if the boolean evaluation can be short-circuited,
     /// in order to ensure the invariant that all expressions are evaluated when inferring types.
-    fn infer_chained_boolean_types<T: Into<Type<'db>>>(
+    fn infer_chained_boolean_types(
         db: &'db dyn Db,
         op: ast::BoolOp,
-        values: impl IntoIterator<Item = T>,
+        values: impl IntoIterator<Item = Type<'db>>,
         n_values: usize,
     ) -> Type<'db> {
         let mut done = false;
@@ -2460,17 +2460,16 @@ impl<'db> TypeInferenceBuilder<'db> {
                     Type::Never
                 } else {
                     let is_last = i == n_values - 1;
-                    let value_ty: Type<'db> = ty.into();
-                    match (value_ty.bool(db), is_last, op) {
-                        (Truthiness::Ambiguous, _, _) => value_ty,
+                    match (ty.bool(db), is_last, op) {
+                        (Truthiness::Ambiguous, _, _) => ty,
                         (Truthiness::AlwaysTrue, false, ast::BoolOp::And) => Type::Never,
                         (Truthiness::AlwaysFalse, false, ast::BoolOp::Or) => Type::Never,
                         (Truthiness::AlwaysFalse, _, ast::BoolOp::And)
                         | (Truthiness::AlwaysTrue, _, ast::BoolOp::Or) => {
                             done = true;
-                            value_ty
+                            ty
                         }
-                        (_, true, _) => value_ty,
+                        (_, true, _) => ty,
                     }
                 }
             }),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2513 on 2024-10-04 16:39_

We've got enough occurrences of this verbose pattern now, it's high time for a helper method:

```diff
+    /// Get the already-inferred type of an expression node.
+    ///
+    /// PANIC if no type has been inferred for this node.
+    fn expression_ty(&self, expr: &ast::Expr) -> Type<'db> {
+        self.types
+            .expression_ty(expr.scoped_ast_id(self.db, self.scope))
+    }
+
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2522 on 2024-10-04 16:42_

nit, to match our usual diagnostic message style:
```suggestion
                                    "Operator `{}` is not supported for types `{}` and `{}`",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2559 on 2024-10-04 16:48_

minor clarification
```suggestion
        // Note: identity (is, is not) for equal builtin types is unreliable and not part of the language spec.
        // - `[ast::CompOp::Is]`: return `false` if unequal, `bool` if equal
        // - `[ast::CompOp::IsNot]`: return `true` if unequal, `bool` if equal
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3139 on 2024-10-04 16:55_

This (and all the TODO comments in this function) are a fantastic resource for someone to flesh this out later -- thank you!!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4138 on 2024-10-04 16:59_

This test, and its comments, are fantastic!

---

_@Slyces reviewed on 2024-10-04 16:59_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:4170 on 2024-10-04 16:59_

Should that be `Literal[False] | A`? There is no runtime path where we can end up with `Literal[True]` here as if the second comparison `1 < A()` evaluates to `True` we would return the value of the last comparison which is `A`.
Only path where we don't take the value of `A` here is if we had a `Literal[False]` in the earlier steps.

But that is more of a comment on how we handle chained `and` I guess?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4170 on 2024-10-04 17:08_

Great point! I think this can be done as a separate follow-up so we can go ahead and get this PR merged; it's kind of separate from this PR as it's an improvement to the existing chained-boolean-expression logic.

I think the logic here is that when we have an actual `bool` type in a chained boolean expression, in non-last-position, rather than adding `bool` to the union we can instead add `Literal[False]` (for AND) or `Literal[True]` (for OR).

There's a more generalized version of this where we add `Falsy` and `Truthy` types, and then we'd always intersect any type in that position (not just `bool`) with `Falsy` or `Truthy`, and then for bools that would simplify out in the intersection (e.g. `bool & Falsy` is `Literal[False]`). I think it's likely we end up doing this for type narrowing anyway, but I don't have strong feelings about whether we go straight for the generalized solution or start with a bool-specific implementation.

---

_@carljm approved on 2024-10-04 17:09_

This is really excellent work, thank you so much!!

I have a few small nits; but since they were small (and I already implemented some locally to make sure they'd work) I'll just push them myself and then land this.

---

_@AlexWaygood reviewed on 2024-10-04 17:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2492 on 2024-10-04 17:12_

This should fix the MSRV build and also reduce the size of the diff by 2 lines!

```suggestion
        for right in comparators.as_ref() {
            self.infer_expression(right);
        }
```

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2465 on 2024-10-04 17:20_

See my comment on the last test, but maybe we could fix this by having a special case for `builtins.bool` (which will be a very common type here)
```suggestion
                        (Truthiness::Ambiguous, false, ast::BoolOp::And) => match value_ty {
                            // Ambiguous types that are not the last in the `and` chain can only be
                            // returned if they are falsy. In the special case of `builtins.bool`,
                            // being falsy is `Literal[False]`.
                            // TODO: we could do this optimisation for other literal that have a
                            // single falsy value (`""`, `0`, ...?)
                            Type::Instance(class) => class
                                .is_stdlib_symbol(db, "builtins", "bool")
                                .then(|| Type::BooleanLiteral(false))
                                .unwrap_or_else(|| value_ty),
                            _ => value_ty,
                        },
                        (Truthiness::Ambiguous, _, _) => value_ty,
```

---

_@Slyces reviewed on 2024-10-04 17:20_

---

_@carljm reviewed on 2024-10-04 17:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4170 on 2024-10-04 17:20_

Created https://github.com/astral-sh/ruff/issues/13632 to track this as a follow-up.

---

_@carljm reviewed on 2024-10-04 17:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2465 on 2024-10-04 17:21_

Let's do this as a follow-up, I created https://github.com/astral-sh/ruff/issues/13632 with some comments about it.

---

_Merged by @carljm on 2024-10-04 17:40_

---

_Closed by @carljm on 2024-10-04 17:40_

---
