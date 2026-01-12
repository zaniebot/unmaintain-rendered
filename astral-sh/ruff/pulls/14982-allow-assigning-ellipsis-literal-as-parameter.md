```yaml
number: 14982
title: Allow assigning ellipsis literal as parameter default value
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: ellipsis-default-type
created_at: 2024-12-15T12:01:06Z
updated_at: 2025-01-05T19:44:34Z
url: https://github.com/astral-sh/ruff/pull/14982
synced_at: 2026-01-12T15:55:49Z
```

# Allow assigning ellipsis literal as parameter default value

---

_@Glyphack_

Resolves #14840

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Usage of ellipsis literal as default argument is allowed in stub files.

## Test Plan

Added mdtest for both python files and stub files.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-12-15 12:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-12-15 12:18_

---

_@InSyncWithFoo reviewed on 2024-12-15 17:05_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:11 on 2024-12-15 17:05_

I think a `(x: int = Ellipsis)` test case should be added, if only for the record.

---

_@Glyphack reviewed on 2024-12-15 19:23_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:11 on 2024-12-15 19:23_

Thanks for the suggestion. I added it.

---

_Comment by @AlexWaygood on 2024-12-15 21:11_

The pre-commit failure will go away if you merge in `main` or rebase :-)

---

_Comment by @Glyphack on 2024-12-15 21:26_

Wow there's a bug in the new github UI for PRs it was not showing the rebase button I thought it's already up to date.

---

_Marked ready for review by @Glyphack on 2024-12-15 21:26_

---

_Review requested from @carljm by @Glyphack on 2024-12-15 21:26_

---

_Review requested from @MichaReiser by @Glyphack on 2024-12-15 21:26_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-12-15 21:26_

---

_Review requested from @sharkdp by @Glyphack on 2024-12-15 21:26_

---

_Comment by @AlexWaygood on 2024-12-16 19:10_

> Wow there's a bug in the new github UI for PRs it was not showing the rebase button I thought it's already up to date.

Oh, that only appears for a repository if the maintainers have checked a certain box in the repository settings on GitHub. We haven't enabled it for Astral repositories because the rate of development is quite rapid, and some folks found it annoying to be constantly told that all their PRs were out of date with the `main` branch 😆

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:7 on 2024-12-16 19:39_

This special case for `...` default values is only allowed in stub files, and the default mdtest path is `test.py`, which is not a stub file. So every test showing the special case working should have an explicit file path with `pyi` prefix, and we should also have a test for a `.py` filename showing that the special case is not applied, and a diagnostic for invalid default-value type is emitted.
```suggestion
For default values the ellipsis literal `...` can be used, in a stub file only.

```py path=test.pyi
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:11 on 2024-12-16 19:40_

importing `typing.Dict` seems like an extraneous wrinkle for these tests. since there's no special behavior around it here.
```suggestion
def f(x: int = ...) -> None: ...
def f2(x: dict = ...) -> None: ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:26 on 2024-12-16 19:41_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:61 on 2024-12-16 19:41_

```suggestion
```py path=test.pyi
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:61 on 2024-12-16 19:41_

```suggestion
```py path=test.pyi
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:14 on 2024-12-16 19:42_

This seems redundant with the dedicated "Class and Module Level Attributes" test below.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1130 on 2024-12-16 19:43_

```suggestion
                } else if self.file.is_stub() && default
```

---

_@carljm reviewed on 2024-12-16 19:43_

Thank you!!

---

_Comment by @Glyphack on 2024-12-16 21:25_

Thanks for the review.
I have a question, pyright behavior here is that in all files the assignment works only if the function body is also ellipsis. The link does not specify if it's stub file but pasting this into a python file locally shows the same warnings.
https://pyright-play.net/?strict=true&code=CYUwZgBGAUAeBcECWA7ALhAvBAdHglBALQB8EAcgPYojwBQEjEATiGgK7MoSx12iQYCZOiy4CxMlRqI8OIA
Mypy does the same. Is this just a limitation of them or the specs are not strictly forbidding this?

---

_Comment by @AlexWaygood on 2024-12-16 21:44_

> This special case is only allowed in function and method arguments. Based on what I understand from [typing.readthedocs.io/en/latest/guides/writing_stubs.html#module-level-attributes](https://typing.readthedocs.io/en/latest/guides/writing_stubs.html#module-level-attributes) and [typing.readthedocs.io/en/latest/guides/writing_stubs.html#classes](https://typing.readthedocs.io/en/latest/guides/writing_stubs.html#classes).
> 
> > Do not unnecessarily use an assignment for module-level attributes.

Ah, I think there's a small misunderstanding here. There are two halves of typing.readthedocs.io:
- One half of the website (https://typing.readthedocs.io/en/latest/spec/) is a formal typing spec: this is a set of rules that type checkers _must_ follow if they claim to support all features of Python's typing system
- The other half of the website (https://typing.readthedocs.io/en/latest/guides/) is a set of guides for users on how they _should_ use annotations in their own runtime code and stub files. These are guides to best practices, but they aren't necessarily things that type checkers need to _enforce_

The two pages you link to there are both in the "second half" of the website: those paragraphs describe best practices for people writing stub files, but these aren't necessarily rules that type checkers have to _enforce_ regarding stub files. The typing spec part of typing.readthedocs.io is here: https://typing.readthedocs.io/en/latest/spec/distributing.html#value-expressions. It states that type checkers should allow `...` as a value in many more places, even if the "guides" part of typing.readthedocs.io states that it wouldn't necessarily be _best practice_ for users to do so. Here's what the spec has to say:

> In locations where value expressions can appear, such as the right-hand side of assignment statements and function parameter defaults, type checkers should support the following expressions:

> - The ellipsis literal, `...`, which can stand in for any value
> -Any value that is a [legal parameter for typing.Literal](https://typing.readthedocs.io/en/latest/spec/literal.html#literal-legal-parameters)
> - Floating point literals, such as 3.14
> - Complex literals, such as 1 + 2j

And later:

> Type checkers should support module-level variable annotations, with and without assignments:
> 
> ```py
> x: int
> x: int = 0
> x = 0  # type: int
> x = ...  # type: int
> ```

---

_Comment by @AlexWaygood on 2024-12-16 21:45_

So I think we should allow `...` as a value for any annotated symbol in stub files, in any context, not just parameter defaults

---

_@AlexWaygood reviewed on 2024-12-16 21:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:1 on 2024-12-16 21:46_

Could you possibly also add a test snippet demonstrating that `...` is _not_ allowed as a default value in a non-stub file if it contradicts the annotation?

---

_Comment by @carljm on 2024-12-16 21:49_

Thanks for that clarification, Alex! I was surprised that `x: int = ...` wouldn't be supported in a stub, and I read the links without noticing they weren't part of the spec.

I feel like the wording of the spec:

> In locations where value expressions can appear... type checkers should support the following expressions: ... The ellipsis literal, ..., which can stand in for any value

Suggests a totally different implementation approach here, which is that in a stub file we should simply infer literal `...` expressions as type `Any`, which is assignable to anything. That seems better than trying to handle this at each point where we enforce assignability.

---

_Comment by @carljm on 2024-12-16 21:52_

> I have a question, pyright behavior here is that in all files the assignment works only if the function body is also ellipsis ... Mypy does the same

That's very interesting, good observation! I don't actually know why that is. It may be in order to support ellipsis defaults in overloads and protocol methods. I'm a little surprised that this appears to be handled by allowing it based on the body being `...`, rather than just allowing it contextually for overloads and protocols.

I'm not convinced we want to follow suit here; it seems incorrect to do this based on a body of `...`, considering that is a valid body for a regular Python function.

So I would say let's implement this only for stubs for now, and consider the best approach when we add overloads/protocols support.



---

_Comment by @AlexWaygood on 2024-12-16 21:53_

> Suggests a totally different implementation approach here, which is that in a stub file we should simply infer literal `...` expressions as type `Any`, which is assignable to anything. That seems better than trying to handle this at each point where we enforce assignability.

That seems okay to me as long as we don't start inferring unions with `Any` for things imported from stub files... but I don't _think_ we will, because declared types take precedence over inferred types for things imported from other modules, right?

---

_Comment by @carljm on 2024-12-16 21:56_

> That seems okay to me as long as we don't start inferring unions with `Any` for things imported from stub files... but I don't _think_ we will, because declared types take precedence over inferred types for things imported from other modules, right?

Yes, that's right.

---

_Comment by @Glyphack on 2024-12-16 22:19_

Thanks let me do try to change this to infer the type of ellipsis literal as Any instead of adding this special case.

---

_Comment by @carljm on 2024-12-16 22:35_

> Suggests a totally different implementation approach here, which is that in a stub file we should simply infer literal `...` expressions as type `Any`, which is assignable to anything. That seems better than trying to handle this at each point where we enforce assignability.

Ok, after further discussion (thanks @AlexWaygood!) I think this is not a good idea, and might cause undesirable behavior in examples like this:

```py
class Foo: ...

X: int = ...
Y: Foo = X
```

Where this should be flagged as an error, even in a type stub, but my suggestion (combined with our current handling of inferred dynamic type) would not consider it an error. (It might be worth including this as a test case.)

So I think the better approach is in fact to special-case handling of literal `...` in three different places: in function parameters, where you're doing it now; in `infer_annotated_assignment_definition`, and in `infer_assignment_definition`.

In the first two cases, if we see a literal `...` as the default-value or RHS, in a stub file, we should just discard that inference and record the declared type as the inferred type. In the third case, there is no declaratiion and we should record `Unknown`.

---

_@Glyphack reviewed on 2024-12-19 20:31_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:1936 on 2024-12-19 20:31_

This was a bit confusing. Using `add_declaration` here fails because this assignment is a binding so the get here fails:
https://github.com/Glyphack/ruff/blob/9963252dfc51e08a53a1047344bd44cde1493d42/crates/red_knot_python_semantic/src/types/infer.rs#L839

This passes the test but can it possibly break some other behavior?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:5 on 2024-12-20 04:52_

```suggestion
The ellipsis literal `...` can be used as a placeholder default value for a function parameter,
 in a stub file only, regardless of the type of the parameter.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:19 on 2024-12-20 04:52_

```suggestion
The ellipsis literal can be assigned to a class or module attribute, regardless of its type, in a
stub file only.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:9 on 2024-12-20 04:54_

I think it would be good to show here that our type inference for the parameter is not impacted by the ellipsis.

(Also lets avoid `dict` here since its revealed type will change when we add generics.)

```suggestion
def f(x: int = ...) -> None:
    reveal_type(x)  # revealed: int
def f2(x: str = ...) -> None:
    reveal_type(x)  # revealed: str
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:28 on 2024-12-20 04:56_

Also lets avoid `float` since its revealed type may change when we add the `float` special case.

```suggestion
y: bytes = ...
reveal_type(y)  # revealed: bytes

class Foo:
    y: int = ...
    
reveal_type(Foo.y)  # revealed: int
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:27 on 2024-12-20 04:57_

```suggestion
In a non-stub file, there's no special treatment of ellipsis literals. An ellipsis literal can only be assigned if
`EllipsisType` is actually assignable to the annotated type.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:41 on 2024-12-20 04:57_

```suggestion
There is no special treatment of the builtin name `Ellipsis`, only of `...` literals.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1857 on 2024-12-20 05:00_

We should use `Type::Unknown` rather than `Type::Any` here. In our model, `Type::Any` is reserved for explicit uses of `typing.Any` as an annotation. Any dynamic type arising from lack of an annotation uses `Type::Unknown`.

Also we should add a test for this case (un-annotated assignment in a stub file, with `...` as RHS.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2024-12-20 05:07_

It would be unusual, but I think valid according to the spec to do `FOO, BAR = ...` in a stub file. So I think we also should add a special case for `...` (only if `file.is_stub(db)` of course) in `infer_unpack_types` (where we infer the `value_ty` from the RHS expression). And a test for this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1930 on 2024-12-20 05:09_

It seems we've forgotten the condition "and the `value` expression is a literal `...`" here. If that doesn't fail any test, we should definitely add a test with e.g. `x: int = 1` in a stub file showing we still infer `Literal[1]` for `x`, not `Unknown`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1987 on 2024-12-20 05:13_

I don't see any need to duplicate the `add_declaration_with_binding` call here.

And the type we fall back to should be the annotated type, not Unknown. We should add a test showing that after `x: int = ...` in a stub file, we infer `int`, not `Unknown`, for `x`.

```suggestion
            let value_ty = if self.file().is_stub(self.db().upcast()) && value.is_ellipsis_literal_expr() {
                annotation_ty
            } else {
                value_ty
            }
            self.add_declaration_with_binding(
                assignment.into(),
                definition,
                annotation_ty,
                value_ty,
            );
```

---

_@carljm reviewed on 2024-12-20 05:14_

Thank you! A few comments.

---

_@MichaReiser reviewed on 2024-12-20 06:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1856 on 2024-12-20 06:55_

Nit: It might be worth introducing a `is_stub` function on `InferenceBuilder` that does the `self.file().is_stub(self.db().upcast())`

---

_Converted to draft by @Glyphack on 2024-12-20 09:57_

---

_@Glyphack reviewed on 2024-12-30 13:35_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:1930 on 2024-12-30 13:35_

Yes it was not failing. Added a new test for it in `mdtest/assignment/annotations.md`.

---

_Comment by @Glyphack on 2024-12-30 14:09_

Out of curiosity I checked what pyright and mypy do when they encounter `...` while unpacking RHS([this comment](https://github.com/astral-sh/ruff/pull/14982#discussion_r1893479538)) in assignment.

With this `test.pyi`(in .py files this is invalid):

```pyi
x, y = ...
```

Pyright: 
```
0 errors, 0 warnings, 0 informations
```

Mypy: 
```
test.pyi:1: error: "EllipsisType" object is not iterable  [misc]
Found 1 error in 1 file (checked 1 source file)
```


---

_@Glyphack reviewed on 2024-12-30 18:35_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2024-12-30 18:35_

I applied this change inside the unpacker since that was the first place that value type was being inferred. Also there was a match to check the expression being used. I hope this is what you meant as well.

But if I understand correctly we don't want the unpacking to happen in for loops so I'm checking that unpack value is of type `UnpackerValue::Assign`.

---

_@Glyphack reviewed on 2024-12-31 14:50_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2024-12-31 14:50_

I spent some time to see if I can pull out the code for special case to `infer_unpack_types` itself. But I think we want the whole unpacking behavior to happen.
This could be done if we only want to treat the case where LHS is a tuple and RHS is `...`. But I feel this behavior would not be consistent. I also checked Pyright.

For the following `.pyi` file Pyright infers all variables the same.
```
a, b = ...
reveal_type(a)
reveal_type(b)

[c] = ...
reveal_type(c)
```

I can't decide if we should only allow `a, b = ...` or other forms of unpacking as well so please let me know if you think the other way is better.

---

_@Glyphack reviewed on 2024-12-31 14:55_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/unpack.rs`:80 on 2024-12-31 14:55_

I wanted to keep this consistent with `is_iterable` so I created this. I think https://docs.rs/is-macro/latest/is_macro/ also works here.

---

_Marked ready for review by @Glyphack on 2024-12-31 14:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:19 on 2025-01-05 17:37_

```suggestion
## Class and module symbols

The ellipsis literal can be assigned to a class or module symbol, regardless of its declared type,
in a stub file only.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:65 on 2025-01-05 17:39_

```suggestion
## Use of `Ellipsis` symbol
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:67 on 2025-01-05 17:39_

```suggestion
There is no special treatment of the builtin name `Ellipsis` in stubs, only of `...` literals.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:34 on 2025-01-05 17:41_

```suggestion

No diagnostic is emitted if an ellipsis literal is "unpacked" in a stub file as part of an
assignment statement:

```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/ellipsis.md`:45 on 2025-01-05 17:42_

```suggestion

Iterating over an ellipsis literal as part of a `for` loop in a stub is invalid, however, and
results in a diagnostic:

```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:439 on 2025-01-05 17:46_

nit: I'd call this method `in_stub` rather than `is_stub`, because (as your doc-comment points out!) it asks the question "Are we _in_ a stub file?"

I'd also make it a method on `InferContext` rather than the `TypeInferenceBuilder`, as then we can use it from `unpacker.rs` as well as from `infer.rs`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/unpacker.rs`:57 on 2025-01-05 17:58_

I think the debug assertion here still applies even if we're doing `foo, bar = ...` in a stub file. The debug assertion applies to the _target_ expression (a.k.a. the left-hand side of the assignment statement), not the _value_ expression (the right-hand side fo the assignment statement). Even in a stub file where the right-hand side is an ellipsis literal, this is still an important invariant to maintain. If I apply this diff to your branch, none of the red-knot tests fail:

```diff
--- a/crates/red_knot_python_semantic/src/types/unpacker.rs
+++ b/crates/red_knot_python_semantic/src/types/unpacker.rs
@@ -36,6 +36,11 @@ impl<'db> Unpacker<'db> {
     }
 
     pub(crate) fn unpack(&mut self, target: &ast::Expr, value: UnpackValue<'db>) {
+        debug_assert!(
+            matches!(target, ast::Expr::List(_) | ast::Expr::Tuple(_)),
+            "Unpacking target must be a list or tuple"
+        );
+
         let mut value_ty = infer_expression_types(self.db(), value.expression())
             .expression_ty(value.scoped_expression_id(self.db(), self.scope));
 
@@ -49,11 +54,6 @@ impl<'db> Unpacker<'db> {
             .is_ellipsis_literal_expr();
         if value.is_assign() && is_in_stub_file && value_is_ellipsis_literal {
             value_ty = Type::Unknown;
-        } else {
-            debug_assert!(
-                matches!(target, ast::Expr::List(_) | ast::Expr::Tuple(_)),
-                "Unpacking target must be a list or tuple"
-            );
         }
```

---

_@AlexWaygood reviewed on 2025-01-05 17:59_

Thanks, this looks close! A few comments below

---

_@AlexWaygood reviewed on 2025-01-05 18:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2025-01-05 18:01_

Blegh, it feels a bit unfortunate that we have to add special casing for unpacking in stub files, because it's really extremely rare to encounter unpacking in stub files. I can say with 100% confidence that there are zero assignment statements in typeshed that use unpacking, because there's a flake8-pyi lint rule prohbiting unpacking in stub files, and we run flake8-pyi in CI at typeshed. Having said that, spec compliance is obviously the most important thing here.

---

_@Glyphack reviewed on 2025-01-05 18:37_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/unpacker.rs`:57 on 2025-01-05 18:37_

Thanks for the explanation. I thought this is checking RHS and moved it to this branch. I'll revert the change.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:126 on 2025-01-05 18:41_

```suggestion
## Annotated assignments in stub files are inferred correctly
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:439 on 2025-01-05 18:44_

There can also be a wrapper `TypeInferenceBuilder::in_stub()` method that just does `self.context.in_stub()`

---

_@carljm reviewed on 2025-01-05 18:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/unpacker.rs`:43 on 2025-01-05 18:53_

I don't think any of the changes in the first few lines here (or to the doc-comment) are necessary; let's revert them to keep the diff clean :-)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:49 on 2025-01-05 18:54_

This can now use `InferContext::in_stub` method, right?

---

_@Glyphack reviewed on 2025-01-05 18:54_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:439 on 2025-01-05 18:54_

Good point. Applied both.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:41 on 2025-01-05 18:55_

This is a minor nit, but I don't think we should remove the word "expression" here; the LHS isn't really a list or tuple (no such object is ever constructed), it's just an expression in that syntactic form
```suggestion
            "Unpacking target must be a list or tuple expression"
```

---

_@carljm approved on 2025-01-05 18:56_

Looks good to me, modulo a couple small nits

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/unpacker.rs`:56 on 2025-01-05 18:57_

You're doing a bit more work here than you need to if it's _not_ a stub file, because all these conditions are evaluated whether or not it's a stub file. You can take advantage of boolean short-circuiting if you just use one combined test rather than assigning the intermediate variables:

```suggestion
        if value.is_assign()
            && self.context.in_stub()
            && value
                .expression()
                .node_ref(self.db())
                .is_ellipsis_literal_expr()
        {
            value_ty = Type::Unknown;
        }

```

---

_@AlexWaygood reviewed on 2025-01-05 18:58_

---

_@AlexWaygood reviewed on 2025-01-05 18:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/unpacker.rs`:49 on 2025-01-05 18:58_

Oops, some simultaneous reviewing going on -- see my suggestion in https://github.com/astral-sh/ruff/pull/14982/files#r1903322714

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/unpacker.rs`:56 on 2025-01-05 19:02_

Oh the other change made this outdated. I apply this manually.

---

_@Glyphack reviewed on 2025-01-05 19:02_

---

_@carljm approved on 2025-01-05 19:11_

Looks great, thank you!

---

_Merged by @carljm on 2025-01-05 19:11_

---

_Closed by @carljm on 2025-01-05 19:11_

---

_Branch deleted on 2025-01-05 19:43_

---

_Comment by @Glyphack on 2025-01-05 19:44_

Thanks for your time and review.

---
