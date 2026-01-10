```yaml
number: 13712
title: "[red-knot] Implement `Type::Tuple` Comparisons"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-tuple-comparision
created_at: 2024-10-11T06:48:38Z
updated_at: 2024-10-16T13:57:40Z
url: https://github.com/astral-sh/ruff/pull/13712
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Implement `Type::Tuple` Comparisons

---

_Pull request opened by @cake-monotone on 2024-10-11 06:48_

## Summary

This PR implements comparisons for (tuple, tuple).

It will close #13688 and complete an item in #13618 once merged.

## Test Plan

Basic tests are included for (tuple, tuple) comparisons.

---

_Review requested from @carljm by @cake-monotone on 2024-10-11 06:48_

---

_Review requested from @MichaReiser by @cake-monotone on 2024-10-11 06:48_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-10-11 06:48_

---

_@cake-monotone reviewed on 2024-10-11 06:50_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2777 on 2024-10-11 06:50_

I’m unsure if implementing this part in such a complex way is the best approach. Couldn’t we simply use `Some(KnownClass::Bool.to_instance(self.db))` instead? While the current implementation can provide powerful inference in limited specific cases, the cost of maintenance might outweigh the benefits. Feedback on this part would be appreciated.

---

_Comment by @github-actions[bot] on 2024-10-11 07:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-11 16:03_

It looks like this is not "unsupported op between specific types" case but rather "not supported by this method" case, so I don't think we should return `None` here. It's a programming error if this method is ever called with an op not in this list, so this should just be a debug assert instead.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2753 on 2024-10-11 16:07_

I think in this case there is an implicit invariant that `eq_result_ty` should never be `None`, because equality comparison is supported between all types (it never raises `TypeError` at runtime). We could assert that, to be sure. Because  if somehow a `None` return were possible, we would want to propagate it so we get an unsupported operation diagnostic message.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2777 on 2024-10-11 16:07_

same comment as above about `None` return

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2777 on 2024-10-11 16:09_

This is a great question. My feeling is that it is worth it. I think once we get the logic correct the maintenance cost will not be high. And there will be a few common cases where this will definitely make a difference. For instance, we will need to implement detailed comparison of `sys.version_info` tuples anyway; I think it's much nicer if this just uses general support for tuple comparison inference rather than being handled as a special case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2845 on 2024-10-11 16:29_

It is possible for an equality comparison to be supported but return a non-bool type. See the runtime behavior:

```
>>> class A:
...     def __eq__(self, other):
...         return other
...
>>> (1, A()) == (1, 4)
True
```

Note the tuple comparison swallows the non-bool type from `A.__eq__` (converts it to bool) and ends up always returning `True` or `False`.

So I think our handling here can/should be a bit more general, we can just call `Type::bool` on any returned type and consider it equal / not equal / unknown according to that `Truthiness` result? The only special case I think we might want is the Type::Todo propagation.

And similar to the comments above, I think `Eq` comparison should never return `None`, so there also shouldn't be any case here where we return `None` explicitly; though we might pass through a `None` from the call back to `infer_binary_type_comparison` with a non-Eq op.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2802 on 2024-10-11 16:33_

In this case I think if we end up comparing tuples with non-comparable types in them, as in this example:
```
>>> (1,) < ("foo",)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<' not supported between instances of 'int' and 'str'
```

We will give a less informative diagnostic than the runtime does, because we just propagate a `None`, we don't propagate details about which two types were ultimately not orderable. That is, our diagnostic would say something like operator `<` not supported between `tuple[int]` and `tuple[str]`, rather than saying (like runtime does) that the real problem is `int` vs `str`. (It's obvious in a minimal example of course, maybe not in a bigger one.)

The fix for this will to have `infer_binary_type_comparison` return a `Result` rather than `Option`, where the `Err` variant carries with it the two types we could not compare, and then we use this information in the diagnostic.

I think it's out of scope to do that in this PR, but I would like a test for the pass-through-None case here, that checks the diagnostic, and has a TODO for the more informative diagnostic.

(I think currently the only case where `infer_binary_type_comparison` returns `None` is an `In` compare, which can't be triggered here. But it would be easy to add other "mixed literal ordering" compare cases that should always return `None`, to make this path possible.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4305 on 2024-10-11 16:37_

So just this week we landed a brand-new test framework, see the tests (as Markdown files) in `crates/red_knot_python_semantic/resources/mdtest` and the documentation in `crates/red_knot_test/README.md`. Ultimately we would like all of these type inference tests to be written in that form instead of as Rust tests like this. It would be great if we could add this new test as a Markdown test instead of here. It would mean adding a new file (let's say `.../mdtest/comparison/tuples.md`) that includes the Python snippets from this test as embedded Python code blocks, and use `reveal_type(...)` and `# revealed: ` assertions for the types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4332 on 2024-10-11 16:41_

This shouldn't be unknown, it should be `Literal[False]` -- we know (or we should know) that `Literal["s"]` and `Literal[2]` cannot be equal.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4378 on 2024-10-11 16:47_

I think we can have a TODO there, this should be inferrable as `Literal[False]` if we correctly inferred `1 == "a"` as `Literal[False]`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4376 on 2024-10-11 16:51_

Thanks for adding thorough tests!

A few tests I'd like to add:

* Some tests of the diagnostic we get for unsupported comparisons inside a tuple, with TODO comment to improve that diagnostic, as discussed above.
* A test for the case shown above where `__eq__` returns a non-bool. (This will just be a Todo for now, unless we implement Type::Instance rich comparison support for `__eq__`, but it will be useful to have the test in place for future anyway.)

---

_@carljm requested changes on 2024-10-11 16:51_

This is really excellent, thank you!! A few comments.

---

_Label `red-knot` added by @carljm on 2024-10-11 19:01_

---

_@cake-monotone reviewed on 2024-10-13 06:43_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-13 06:43_

I agree with your point. This is indeed a programming error, and makes meaning of `None` ambiguous.

However, as you know, lexicographic order cannot be defined outside the comparison operators `==`, `!=`, `<`, `<=`, `>`, and `>=`. Even if we use `debug_assert!`, avoiding `None` would be impossible without using `unreachable!`.

So, what if we change the type of the `op` parameter to prevent errors at compile-time instead?

In the Python docs, comparison operations are divided into three categories: [(Python docs)](https://docs.python.org/3/reference/expressions.html#value-comparisons)

1. Value Comparison
2. Membership Test Comparison
3. Identity Comparison

What do you think about defining new enums and using it like this?

```rust
enum ValueCmpOp {
    Eq,
    NotEq,
    Lt,
    LtE,
    Gt,
    GtE,
}

//...

enum CmpOpSubset {
    Value(ValueCmpOp),
    MembershipTest(MembershipTestCmpOp),
    Identity(IdentityCmpOp),
}

impl From<ast::CmpOp> for CmpOpSubset {
    // ...
}

fn infer_lexicographic_type_comparison(
    &mut self,
    left: &[Type<'db>],
    op: ValueCmpOp,
    right: &[Type<'db>],
) -> Option<Type<'db>> {
    // implementation
}
```

This would allow for an implementation like this, where only the necessary op can be passed, eliminating the need to return None in `infer_lexicographic_type_comparison`:
```rust
match op.into() {
    CmpOpSubset::Value(op) => {
        self.infer_lexicographic_type_comparison(lhs_elements, op, rhs_elements)
    }
    CmpOpSubset::MembershipTest(op) => {
        // ...
    }
    CmpOpSubset::Identity(op) => {
        // ...
    }
}
```

---

_@cake-monotone reviewed on 2024-10-13 07:00_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2777 on 2024-10-13 07:00_

Thanks for the insight! I hadn’t thought about `sys.version_info`.
I’m excited to see how powerful the type inference will become 👍 

---

_Converted to draft by @cake-monotone on 2024-10-14 12:51_

---

_@cake-monotone reviewed on 2024-10-15 16:36_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2802 on 2024-10-15 16:36_

Yes! If I understood correctly, I’ve added some test cases here:

[MD Test - Comparison Unsupported Part](https://github.com/astral-sh/ruff/pull/13712/files#diff-eff9f162b9d83ddfe43d3f6b28ed022767bc58d12d7ccb0c95d985e827471362R83-R103)

---

_@cake-monotone reviewed on 2024-10-15 16:36_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:4376 on 2024-10-15 16:36_

- unsupported comparison
https://github.com/astral-sh/ruff/pull/13712/files#diff-eff9f162b9d83ddfe43d3f6b28ed022767bc58d12d7ccb0c95d985e827471362R83-R103
- non-bool 
(https://github.com/astral-sh/ruff/pull/13712/files#diff-eff9f162b9d83ddfe43d3f6b28ed022767bc58d12d7ccb0c95d985e827471362R83-R103)

---

_Comment by @cake-monotone on 2024-10-15 16:41_

In addition to the changes mentioned in your review, I have enhanced the inference for `in` and `not In` comparisons to make it more powerful.

---

_Marked ready for review by @cake-monotone on 2024-10-15 16:41_

---

_Review requested from @carljm by @cake-monotone on 2024-10-15 16:42_

---

_@cake-monotone reviewed on 2024-10-15 16:46_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:4378 on 2024-10-15 16:46_

also in here!: https://github.com/astral-sh/ruff/pull/13712/files#diff-eff9f162b9d83ddfe43d3f6b28ed022767bc58d12d7ccb0c95d985e827471362R92-R96

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:98 on 2024-10-15 17:59_

```suggestion
# TODO: should be Unknown and add more informative diagnostics
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:159 on 2024-10-15 18:03_

nit: backticks should be used for quoting code, not prose

```suggestion
"Membership Test Comparisons" refers to the operators `in` and `not in`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:182 on 2024-10-15 18:03_

```suggestion
"Identity Comparisons" refers to `is` and `is not`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2723 on 2024-10-15 18:07_

As a general principle, I prefer to use full words in variable names -- and in this specific case I would somewhat strongly prefer to use "count" for these names

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2744 on 2024-10-15 18:14_

If we rely on the invariant you describe in this comment, maybe we should do something like this? (Same applies for the next `debug_assert!` in this PR)

```suggestion
                            let eq_result = self.infer_binary_type_comparison(
                                Type::Tuple(lhs),
                                ast::CmpOp::Eq,
                                *ty,
                            )
                            .expect("infer_binary_type_comparison should never return None for `CmpOp::Eq`");
                            match eq_result {
                                Type::Todo => return Type::Todo,
                                ty => match ty.bool(self.db) {
                                    Truthiness::AlwaysTrue => eq_cnt += 1,
                                    Truthiness::AlwaysFalse => not_eq_cnt += 1,
                                    Truthiness::Ambiguous => (),
                                },
                            }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2823 on 2024-10-15 18:22_

if you do this then you don't have to dereference the types very time you use them in the loop body below:

```suggestion
        for (l_ty, r_ty) in left.iter().copied().zip(right.iter().copied()) {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-15 18:43_

I'd much prefer it if we could make the signature of this more type-safe; this makes me think that we should define a custom enum that this function accepts. Something like this? (The diff is relative to your PR branch)

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -2709,15 +2709,16 @@ impl<'db> TypeInferenceBuilder<'db> {
                 let lhs_elements = lhs.elements(self.db).as_ref();
                 let rhs_elements = rhs.elements(self.db).as_ref();
 
+                let mut lexicographic_type_comparison =
+                    |op| self.infer_lexicographic_type_comparison(lhs_elements, op, rhs_elements);
+
                 match op {
-                    ast::CmpOp::Eq
-                    | ast::CmpOp::NotEq
-                    | ast::CmpOp::Lt
-                    | ast::CmpOp::LtE
-                    | ast::CmpOp::Gt
-                    | ast::CmpOp::GtE => {
-                        self.infer_lexicographic_type_comparison(lhs_elements, op, rhs_elements)
-                    }
+                    ast::CmpOp::Eq => lexicographic_type_comparison(RichCompareOperator::Eq),
+                    ast::CmpOp::NotEq => lexicographic_type_comparison(RichCompareOperator::Ne),
+                    ast::CmpOp::Lt => lexicographic_type_comparison(RichCompareOperator::Lt),
+                    ast::CmpOp::LtE => lexicographic_type_comparison(RichCompareOperator::Le),
+                    ast::CmpOp::Gt => lexicographic_type_comparison(RichCompareOperator::Gt),
+                    ast::CmpOp::GtE => lexicographic_type_comparison(RichCompareOperator::Ge),
                     ast::CmpOp::In | ast::CmpOp::NotIn => {
                         let mut eq_cnt = 0usize;
                         let mut not_eq_cnt = 0usize;
@@ -2757,7 +2758,7 @@ impl<'db> TypeInferenceBuilder<'db> {
                         // - `[ast::CmpOp::IsNot]`: returns `true` if the elements are definitely unequal, otherwise `bool`
                         let eq_result = self.infer_lexicographic_type_comparison(
                             lhs_elements,
-                            ast::CmpOp::Eq,
+                            RichCompareOperator::Eq,
                             rhs_elements,
                         );
                         // In Python, if `__eq__` is not defined, it implicitly uses object's `__eq__`. Adding `debug_assert!` as a precaution.
@@ -2800,25 +2801,9 @@ impl<'db> TypeInferenceBuilder<'db> {
     fn infer_lexicographic_type_comparison(
         &mut self,
         left: &[Type<'db>],
-        op: ast::CmpOp,
+        op: RichCompareOperator,
         right: &[Type<'db>],
     ) -> Option<Type<'db>> {
-        if !matches!(
-            op,
-            ast::CmpOp::Eq
-                | ast::CmpOp::NotEq
-                | ast::CmpOp::Lt
-                | ast::CmpOp::LtE
-                | ast::CmpOp::Gt
-                | ast::CmpOp::GtE
-        ) {
-            debug_assert!(
-                false,
-                "Unsupported comparison operator for lexicographic type comparison."
-            );
-            return None;
-        }
-
         // Compare paired elements from left and right slices
         for (l_ty, r_ty) in left.iter().zip(right.iter()) {
             let eq_result = self.infer_binary_type_comparison(*l_ty, ast::CmpOp::Eq, *r_ty);
@@ -2836,7 +2821,7 @@ impl<'db> TypeInferenceBuilder<'db> {
                     Truthiness::AlwaysTrue => continue,
                     // Types are not equal, perform the specified comparison and return the result
                     Truthiness::AlwaysFalse => {
-                        return self.infer_binary_type_comparison(*l_ty, op, *r_ty)
+                        return self.infer_binary_type_comparison(*l_ty, ast::CmpOp::Eq, *r_ty)
                     }
                     // If the intermediate result is abiguous, we cannot determine the final result as BoolLiteral.
                     // In this case, we simply return a bool instance.
@@ -2851,13 +2836,12 @@ impl<'db> TypeInferenceBuilder<'db> {
         let (l_len, r_len) = (left.len(), right.len());
 
         Some(Type::BooleanLiteral(match op {
-            ast::CmpOp::Eq => l_len == r_len,
-            ast::CmpOp::NotEq => l_len != r_len,
-            ast::CmpOp::Lt => l_len < r_len,
-            ast::CmpOp::LtE => l_len <= r_len,
-            ast::CmpOp::Gt => l_len > r_len,
-            ast::CmpOp::GtE => l_len >= r_len,
-            _ => return None,
+            RichCompareOperator::Eq => l_len == r_len,
+            RichCompareOperator::Ne => l_len != r_len,
+            RichCompareOperator::Lt => l_len < r_len,
+            RichCompareOperator::Le => l_len <= r_len,
+            RichCompareOperator::Gt => l_len > r_len,
+            RichCompareOperator::Ge => l_len >= r_len,
         }))
     }
 
@@ -3318,6 +3302,16 @@ impl<'db> TypeInferenceBuilder<'db> {
     }
 }
 
+#[derive(Debug, Clone, Copy, PartialEq, Eq)]
+enum RichCompareOperator {
+    Eq,
+    Ne,
+    Gt,
+    Ge,
+    Lt,
+    Le,
+}
+
```

I see this is similar to the idea you mentioned in https://github.com/astral-sh/ruff/pull/13712#discussion_r1798097932!

---

_@AlexWaygood reviewed on 2024-10-15 18:44_

Thanks! Some comments below:

---

_@dhruvmanila reviewed on 2024-10-16 05:40_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:2851 on 2024-10-16 05:40_

```suggestion
        let (left_len, right_len) = (left.len(), right.len());
```

---

_@dhruvmanila reviewed on 2024-10-16 05:46_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-16 05:46_

And, we could avoid the `Option` in the return type, I think? Or, we could make it a hard panic instead of a `debug_assert` and add it to the function docs. Regardless of the solution, I'd prefer to avoid the return type to be an `Option` as I think the `None` variant is returned only for unsupported comparison operator.

---

_@cake-monotone reviewed on 2024-10-16 08:30_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-16 08:30_

> I'd much prefer it if we could make the signature of this more type-safe; this makes me think that we should define a custom enum that this function accepts. Something like this? (The diff is relative to your PR branch)
> 
> ...
> 
> I see this is similar to the idea you mentioned in [#13712 (comment)](https://github.com/astral-sh/ruff/pull/13712#discussion_r1798097932)!


Looks great! I think this enum could also be useful for implementing inference for rich-comparisons. I’ve applied it!


---

_@cake-monotone reviewed on 2024-10-16 08:38_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-16 08:38_

> And, we could avoid the `Option` in the return type, I think? Or, we could make it a hard panic instead of a `debug_assert` and add it to the function docs. Regardless of the solution, I'd prefer to avoid the return type to be an `Option` as I think the `None` variant is returned only for unsupported comparison operator.

Hmm, I don’t think we can avoid returning an `Option` in `infer_lexicographic_type_comparison`. Like `(1, "hello") < (1, 2)`, we need to propagate `None` from `infer_binary_type_comparison`. `None` can be returned in this line: [here](https://github.com/astral-sh/ruff/pull/13712/files#diff-65c2c229c88f4021638c996a7496384000d9e7b53b08426b34e92f120bd30b06R2807).

---

_@AlexWaygood reviewed on 2024-10-16 08:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:98 on 2024-10-16 08:51_

```suggestion
# TODO: should be Unknown and add more informative diagnostics
```

---

_@cake-monotone reviewed on 2024-10-16 08:52_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:98 on 2024-10-16 08:52_

Oh my gosh, thank you!! I'll fix it soon.

---

_@cake-monotone reviewed on 2024-10-16 08:53_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-16 08:53_

I think we can merge this with the discussion in https://github.com/astral-sh/ruff/pull/13712#discussion_r1801739049

---

_@AlexWaygood reviewed on 2024-10-16 08:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:98 on 2024-10-16 08:54_

hehe, no worries

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2786 on 2024-10-16 10:53_

```suggestion
    /// it returns `bool`. Returns `None` if the comparison is not supported.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2816 on 2024-10-16 10:54_

```suggestion
        // At this point, the lengths of the two slices may be different, but the prefix of
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2809 on 2024-10-16 10:57_

```suggestion
                    // If the intermediate result is ambiguous, we cannot determine the final result as BooleanLiteral.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-16 10:59_

> Hmm, I don’t think we can avoid returning an `Option` in `infer_lexicographic_type_comparison`. Like `(1, "hello") < (1, 2)`, we need to propagate `None` from `infer_binary_type_comparison`. `None` can be returned in this line: [here](https://github.com/astral-sh/ruff/pull/13712/files#diff-65c2c229c88f4021638c996a7496384000d9e7b53b08426b34e92f120bd30b06R2807).

Yes, this seems more tricky/requiring a larger refactor... I'm okay with leaving it for now, but maybe @dhruvmanila sees an easy solution :-)

---

_@AlexWaygood reviewed on 2024-10-16 10:59_

---

_@AlexWaygood approved on 2024-10-16 11:00_

Thanks! This LGTM now, but I'd love a second approval from either @carljm or @dhruvmanila

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2820 on 2024-10-16 11:31_

Returning `Option` here is correct. `None` signifies "unsupported comparison" and should emit a diagnostic. We may want to refactor to have `infer_binary_type_comparison`, and this method, return a `Result` instead of an `Option`, for the sake of a more informative diagnostic (that tells us precisely which two inner types can't be compared, rather than just telling us the two tuple types can't be compared), as I mentioned in another comment. But this wouldn't be a correctness fix, just a more-informative-diagnostics fix.

---

_@carljm approved on 2024-10-16 11:34_

This looks good to me, modulo a few minor comment nits. Thanks for your work here!

I would like better test coverage (fewer todos). For example, right now the "unsupported comparison" case isn't really covered by the tests, because we don't have any rich-comparison operators implemented to return `None` from `infer_binary_type_comparison` for unsupported comparisons yet.

But the code here looks correct, so I'm happy to merge it as-is; the effective test coverage here will improve as we flesh out `infer_binary_type_comparison`.

---

_Merged by @carljm on 2024-10-16 11:39_

---

_Closed by @carljm on 2024-10-16 11:39_

---

_Branch deleted on 2024-10-16 13:57_

---
