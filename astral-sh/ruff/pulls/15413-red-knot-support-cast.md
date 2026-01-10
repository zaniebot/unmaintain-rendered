```yaml
number: 15413
title: "[red-knot] Support `cast`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-cast
created_at: 2025-01-11T05:19:57Z
updated_at: 2025-01-13T18:55:12Z
url: https://github.com/astral-sh/ruff/pull/15413
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Support `cast`

---

_Pull request opened by @InSyncWithFoo on 2025-01-11 05:19_

## Summary

Resolves #15379.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-11 05:19_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-11 05:19_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-11 05:19_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-11 05:19_

---

_Comment by @github-actions[bot] on 2025-01-11 05:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2025-01-11 05:45_

I think reusing `arguments` is easier than the workaround mentioned [here](https://github.com/astral-sh/ruff/issues/15379#issuecomment-2584001258).

---

_Label `red-knot` added by @MichaReiser on 2025-01-11 09:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3491 on 2025-01-11 12:33_

As your PR curerently stands, there's no need to add this extra variant, as it has exactly the same rules as the first variant in this enum. If I make this change, all your tests pass:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -3447,7 +3447,7 @@ impl KnownFunction {
     /// Return the [`ParameterExpectations`] for this function.
     const fn parameter_expectations(self) -> ParameterExpectations {
         match self {
-            Self::IsFullyStatic | Self::IsSingleton | Self::IsSingleValued => {
+            Self::IsFullyStatic | Self::IsSingleton | Self::IsSingleValued | Self::Cast => {
                 ParameterExpectations::SingleTypeExpression
             }
 
@@ -3457,7 +3457,6 @@ impl KnownFunction {
             | Self::IsDisjointFrom => ParameterExpectations::TwoTypeExpressions,
 
             Self::AssertType => ParameterExpectations::ValueExpressionAndTypeExpression,
-            Self::Cast => ParameterExpectations::TypeExpressionAndValueExpression,
 
             Self::ConstraintFunction(_)
             | Self::Len
@@ -3478,16 +3477,14 @@ enum ParameterExpectations {
     /// All parameters in the function expect value expressions
     #[default]
     AllValueExpressions,
-    /// The first parameter in the function expects a type expression
+    /// The first parameter in the function expects a type expression;
+    /// all other parameters expect value expressions
     SingleTypeExpression,
     /// The first two parameters in the function expect type expressions
     TwoTypeExpressions,
     /// The first parameter in the function expects a value expression,
     /// and the second expects a type expression
     ValueExpressionAndTypeExpression,
-    /// The first parameter in the function expects a type expression,
-    /// and the second expects a value expression
-    TypeExpressionAndValueExpression,
 }
 
 impl ParameterExpectations {
@@ -3516,13 +3513,6 @@ impl ParameterExpectations {
                     ParameterExpectation::ValueExpression
                 }
             }
-            Self::TypeExpressionAndValueExpression => {
-                if parameter_index == 1 {
-                    ParameterExpectation::ValueExpression
-                } else {
-                    ParameterExpectation::TypeExpression
-                }
-            }
         }
     }
 }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 12:39_

`cast()` _can_ actually be used with keyword arguments (not necessarily in the same order as the parameters!) at runtime:

```pycon
>>> from typing import cast
>>> cast(val="foo", typ=int)
'foo'
```

Other type checkers do not support `cast()` being called with keyword arguments, so I don't think we need to either. However, we _do_ need to make sure that we give a comphrehensible error message if either argument passed is a keyword argument. Right now, running red-knot with this branch on the following snippet...

```py
from typing_extensions import cast, reveal_type
reveal_type(cast(val="foo", typ=int))
```

...yields this output:

```
info[revealed-type] /Users/alexw/dev/experiment/foo.py:3:1 Revealed type is `Unknown`
warning[lint:unresolved-reference] /Users/alexw/dev/experiment/foo.py:3:23 Name `foo` used when not defined
```

By comparison, [mypy reports](https://mypy-play.net/?mypy=latest&python=3.12&gist=2b296c518c2a2652ab156353c6c9e762):

```
main.py:3: error: "cast" must be called with 2 positional arguments  [misc]
main.py:3: note: Revealed type is "Any"
```

which is an error message that makes much more sense.

I think we do need to add the logic forbidding keyword arguments manually. Even when we support overloads and understand the signature that typeshed gives for this function, it won't help us here, because the typeshed signature permits keyword arguments.

---

_@AlexWaygood reviewed on 2025-01-11 12:39_

---

_@carljm reviewed on 2025-01-11 16:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 16:08_

The context here is that this PR's argument binding is a temporary hack to work around the fact that `cast` is typed with overloads, and we don't understand overloads yet. Better error messages for various kinds of calls will fall out automatically when this uses real call binding, when we support overloads. So I don't think we should spend significant effort here improving diagnostics related to edge cases of argument binding, in this version. We should either decide that we want `cast` support sooner, that generally works on the typical use cases, or that we want to hold off on `cast` support entirely until after we support overloads. I would be inclined to land this PR with TODOs. (But I haven't done a full review of it yet.)

---

_@AlexWaygood reviewed on 2025-01-11 16:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 16:11_

@carljm, as I said above, I think support for overloads is irrelevant to the issue I'm pointing out here:

> I think we do need to add the logic forbidding keyword arguments manually. Even when we support overloads and understand the signature that typeshed gives for this function, it won't help us here, because the typeshed signature permits keyword arguments.

Pyright supports overloads, of course, but it gives a terrible error message because it falls down the same pitfall that this PR has: https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDGAhgM4wBQ5IApgG7VEA2A%2BvAtQBTFke1MC8AImBgwggDSxE-VDACUc8kA. But mypy does much better here.

---

_@AlexWaygood reviewed on 2025-01-11 16:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 16:19_

I am okay with leaving this as-is for now if you think that the logical place to insert the necessary special-casing will be moved about/rewritten once we have support for overloads. But I think we should at least add a test (with a TODO for a better diagnostic) that demonstrates what happens if you pass arguments out of order by keyword

---

_@InSyncWithFoo reviewed on 2025-01-11 17:03_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:3491 on 2025-01-11 17:03_

That would be correct, but I think the semantic difference is important.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3491 on 2025-01-11 17:06_

in which case I would at least make this change:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -3495,7 +3495,7 @@ impl ParameterExpectations {
     fn expectation_at_index(self, parameter_index: usize) -> ParameterExpectation {
         match self {
             Self::AllValueExpressions => ParameterExpectation::ValueExpression,
-            Self::SingleTypeExpression => {
+            Self::SingleTypeExpression | Self::TypeExpressionAndValueExpression => {
                 if parameter_index == 0 {
                     ParameterExpectation::TypeExpression
                 } else {
@@ -3516,13 +3516,6 @@ impl ParameterExpectations {
                     ParameterExpectation::ValueExpression
                 }
             }
-            Self::TypeExpressionAndValueExpression => {
-                if parameter_index == 1 {
-                    ParameterExpectation::ValueExpression
-                } else {
-                    ParameterExpectation::TypeExpression
-                }
-            }
         }
     }
 }
```

Your PR currently has a logic bug: if three arguments were passed to `cast()`, we'd call `infer_type_expression()` on the third parameter, when we should be calling `infer_expression()` on it. Calling `cast()` with three arguments is obviously invalid, but we still need to call either `infer_expression()` or `infer_type_expression()` on every argument passed to every call, and for everything except the first argument to `cast()`, it should be `infer_expression()` rather than `infer_type_expression()`.

---

_@AlexWaygood reviewed on 2025-01-11 17:06_

---

_@carljm reviewed on 2025-01-11 17:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 17:17_

Ah yeah, I just realized that what you're talking about is actually the same problem that I was just thinking about last night! I don't think any special-casing for `cast` is what we will want here; the problem is that our "type expression or value expression" feature is fundamentally wrong, because it is currently based on argument order instead of parameter order. What we should do is first (before we even infer types for arguments) establish the mapping of arguments to parameters, then infer types for arguments (possibly inferring some as type expressions based not on their argument index, but on the index of the _parameter_ they map to), and then check argument types against parameter types.

If we both fix this, and support overloads, then we'll get correct behavior in the case you're discussing, without any special-casing.

This will require some re-working of the call checking code, to split up "map arguments to parameters" from "check types", and allow type inference of arguments to occur in between those two. It's something we will need to do anyway for contextual type inference of arguments. I don't think it has to block this PR, but I agree we should add a clear test with TODO.

---

_@AlexWaygood reviewed on 2025-01-11 17:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 17:20_

> I don't think any special-casing for `cast` is what we will want here; the problem is that our "type expression or value expression" feature is fundamentally wrong, because it is currently based on argument order instead of parameter order. What we should do is first (before we even infer types for arguments) establish the mapping of arguments to parameters, then infer types for arguments (possibly inferring some as type expressions based on which _parameter_ they map to), and then check argument types against parameter types.

Hmm, are you really sure it's worth the added complexity? As I demonstrated above, neither mypy or pyright supports using keyword arguments with `cast()`. And the problem doesn't arise with `assert_type()`, as that function only supports arguments being passed positionally.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 17:22_

> Hmm, are you really sure it's worth the added complexity?

Yes, because we will need 100% of this complexity regardless, in order to support contextual type inference of arguments in general. (Things like inferring a dict literal argument as `TypedDict` when the parameter it maps to is annotated as `TypedDict` vs some other mapping/dict type.)

Supporting calls to `cast` with out-of-order keyword arguments is unimportant, but it's fine if it falls out of just generally handling contextual type inference correctly, and it's also nice if comprehensible error messages also fall out without any special-casing.

---

_@carljm reviewed on 2025-01-11 17:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 17:25_

I'm not sure I understand what you're referring to there, but I'm happy to leave this for now if you're sure this will be necessary :-)

---

_@AlexWaygood reviewed on 2025-01-11 17:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3525 on 2025-01-11 17:26_

I still don't understand what the advantage is here to having a separate branch, when the logic is identical to the `Self::SingleTypeExpression` branch above. Why not just do this? It's surely less code for us to maintain?

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -3495,7 +3495,7 @@ impl ParameterExpectations {
     fn expectation_at_index(self, parameter_index: usize) -> ParameterExpectation {
         match self {
             Self::AllValueExpressions => ParameterExpectation::ValueExpression,
-            Self::SingleTypeExpression => {
+            Self::SingleTypeExpression | Self::TypeExpressionAndValueExpression => {
                 if parameter_index == 0 {
                     ParameterExpectation::TypeExpression
                 } else {
@@ -3516,13 +3516,6 @@ impl ParameterExpectations {
                     ParameterExpectation::ValueExpression
                 }
             }
-            Self::TypeExpressionAndValueExpression => {
-                if parameter_index == 0 {
-                    ParameterExpectation::TypeExpression
-                } else {
-                    ParameterExpectation::ValueExpression
-                }
-            }
         }
     }
 }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2018 on 2025-01-11 17:27_

did you mean this?

```suggestion
                        // TODO: Use `.two_parameter_tys()` exclusively when overloads are supported.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:27 on 2025-01-11 17:28_

Let's assert the full error message here so it's clear what the problem is

```suggestion
cast(val="foo", typ=int)  # error: [unresolved-reference] "Name `foo` used when not defined"
```

---

_@AlexWaygood reviewed on 2025-01-11 17:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 17:29_

I'm referring to cases like the ones Eric shows in https://discuss.python.org/t/pep-747-typeexpr-type-hint-for-a-type-expression/55984/67 -- but extend the same cases to functions with multiple arguments and the possibility of out-of-order keyword calls of those functions. In general, there are cases where we have to know which annotated parameter an argument maps to, in order to infer the correct type for that argument among multiple possibly valid types for it. (`TypeForm` will add another case of this.)

---

_@carljm reviewed on 2025-01-11 17:29_

---

_@AlexWaygood reviewed on 2025-01-11 17:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:1 on 2025-01-11 17:33_

Okay, I think I see. So you're saying that other changes that we will anyway need to make to our logic (in order to support contextual inference of arguments passed to calls) will mean that supporting keyword arguments for `cast()` will become trivial, meaning that (unlike other type checkers) we'll be able to support keyword arguments for `cast()` and no special-casing here will be necessary

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:5 on 2025-01-11 23:38_

```suggestion
The (inferred) type of the value and the given type do not need to have any correlation.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:11 on 2025-01-11 23:38_

this import is unused

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/cast.md`:26 on 2025-01-11 23:39_

```suggestion
# TODO: Either support keyword arguments properly,
# or give a comprehensible error message saying they're unsupported
```

---

_@AlexWaygood reviewed on 2025-01-11 23:40_

Looks good to me now, thanks! Just a couple more small nits

---

_@AlexWaygood approved on 2025-01-11 23:48_

---

_Merged by @AlexWaygood on 2025-01-12 13:05_

---

_Closed by @AlexWaygood on 2025-01-12 13:05_

---

_Branch deleted on 2025-01-12 15:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:54 on 2025-01-13 18:54_

I didn't complete a review here before it was landed, but I was going to recommend that we not add this `CallOutcome` variant. Put up a follow-up PR at https://github.com/astral-sh/ruff/pull/15461

---

_@carljm reviewed on 2025-01-13 18:55_

Thank you!

---
