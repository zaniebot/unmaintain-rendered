```yaml
number: 16765
title: "[red-knot] Emit error if int/float/complex/bytes/boolean literals appear in type expressions outside `typing.Literal[]`"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: literal-types-invalid-type-expression
created_at: 2025-03-15T17:46:30Z
updated_at: 2025-03-17T20:55:56Z
url: https://github.com/astral-sh/ruff/pull/16765
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] Emit error if int/float/complex/bytes/boolean literals appear in type expressions outside `typing.Literal[]`

---

_@MatthewMckee4_

Expand `InvalidTypeExpression` enum to include number, bytes and boolean in type expression

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes https://github.com/astral-sh/ruff/issues/16532

## Test Plan

Still need to look into where tests should be put for this improvement

---

_Comment by @MatthewMckee4 on 2025-03-15 18:14_

I may be wrong, but it seems that byte literals are already handled here: 
https://github.com/astral-sh/ruff/blob/44aec36e341dafa3932b11f856f368b0e14b9ec6/crates/red_knot_python_semantic/src/types/infer.rs#L5445-L5452.

And tested here:
https://github.com/astral-sh/ruff/blob/44aec36e341dafa3932b11f856f368b0e14b9ec6/crates/red_knot_python_semantic/resources/mdtest/annotations/string.md?plain=1#L88-L89

---

_Label `red-knot` added by @AlexWaygood on 2025-03-15 18:17_

---

_Comment by @MatthewMckee4 on 2025-03-16 12:38_

There's now a test failing at
https://github.com/astral-sh/ruff/blob/ccec3647a38eb8c514d9b69124500b374d308f95/crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md?plain=1#L141

I think this is expected though, as the generics are not implemented for SpecialForm i think?

---

_Marked ready for review by @MatthewMckee4 on 2025-03-16 12:41_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-16 12:41_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-03-16 12:41_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-16 12:41_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-16 12:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 20:54_

Hmm, I'm not sure we need the new branches in this method? The existing code already leads to this method returning an `Err` for all of these variants, and I think the existing error message is okay? All tests pass if I revert all the changes you've made to this file :-)

The difference between the `match` statement in this method and the `match` statement in `infer.rs` is that this method operates on _types_ whereas the one in `infer.rs` operates on _AST nodes_. So the one in `infer.rs` rejects things like this, where you have AST nodes that are just never valid in type expressions, such as int literals, e.g.

```py
def f(x: 1): ...
```

but this `match` statement here rejects _types_ that are invalid in type expressions, even if the _AST nodes_ representing them are valid in type expressions. E.g. here, `x` is an `Name` node, and `Name` nodes are fine in type expressions. But the inferred _type_ of the node is `Literal[42]`, and that's still invalid in a type expression. So the `x`` would pass through the check in `infer.rs` but then would be rejected by the check here in this file at a later stage:

```py
x = 42

def f(y: x): ...
```

the `match` in this method is more or less complete (except for the `KnownInstance` branch; I don't think it should need any changes in this PR

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:131 on 2025-03-16 21:04_

we need to make sure we _avoid_ emitting an error here: this is a valid type annotation!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:50 on 2025-03-16 21:05_

it would be nice if we could specifically say "int literal", "float literal" or "complex literal" in the error message -- "number literal" is a slightly odd phrase that I doubt our users would be familiar with

---

_@AlexWaygood reviewed on 2025-03-16 21:11_

Thanks! I think you're mostly on the right track, but there's a few issues here to sort out

---

_@AlexWaygood reviewed on 2025-03-16 21:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:14_

> All tests pass if I revert all the changes you've made to this file

Ah, your latest tests you added in https://github.com/astral-sh/ruff/pull/16765/commits/4f120d9308f7a7841c304eed38f16172cca41b65 now fail. But that's just because you're asserting the exact error message, which you've changed as part of this PR. I think the existing error messages for these branches were okay!

---

_@MatthewMckee4 reviewed on 2025-03-16 21:15_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:15_

Yeah okay i see what you mean, ill revert these changes and update the tests

---

_@AlexWaygood reviewed on 2025-03-16 21:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:131 on 2025-03-16 21:18_

Oh, I'm sorry. This _is_ an invalid type annotation, because this is `other.Literal` rather than `typing.Literal`. So this is a great change! I should have looked more closely.

---

_@MatthewMckee4 reviewed on 2025-03-16 21:19_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:131 on 2025-03-16 21:19_

I think we still need to implement this functionality though right? Specifically for SpecialForm (As usually this should be invalid if not SpecialForm)

---

_@AlexWaygood reviewed on 2025-03-16 21:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6123 on 2025-03-16 21:19_

I think we can delete this comment as part of your PR: it's incorrect and confusing. We avoid calling this method at all when inferring the slice expressions for special forms such as `Literal`, etc.

```suggestion
```

---

_@AlexWaygood reviewed on 2025-03-16 21:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:131 on 2025-03-16 21:21_

We don't want to allow numeric literals inside arbitrary special forms, just inside specific special forms such as `typing.Literal` and `typing.Annotated`. And we already do that. My original comment here was totally off-base here, sorry!

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:131 on 2025-03-16 21:29_

All good, is this test still useful then? Do you think it would be better suited in the invalid.md file?

---

_@MatthewMckee4 reviewed on 2025-03-16 21:29_

---

_@MatthewMckee4 reviewed on 2025-03-16 21:31_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:31_

Do you think we should keep the 'instance of float' check?

---

_@MatthewMckee4 reviewed on 2025-03-16 21:38_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:50 on 2025-03-16 21:38_

Sounds good, i dont think i know the best way to get the type of the Expr::NumberLiteral as a string representation

---

_@AlexWaygood reviewed on 2025-03-16 21:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:131 on 2025-03-16 21:39_

It's still useful, yeah! I think it makes sense where it is, as it demonstrates that a `Literal` symbol must be imported from the `typing` module in order for us to give it `typing.Literal`'s special semantics

---

_@MatthewMckee4 reviewed on 2025-03-16 21:40_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:40_

Should i also add check for complex number?

---

_@AlexWaygood reviewed on 2025-03-16 21:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:50 on 2025-03-16 21:44_

I think you can do something like this:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 8ad821fe3..90e6a325b 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -6126,11 +6126,36 @@ impl<'db> TypeInferenceBuilder<'db> {
                 );
                 Type::unknown()
             }
-            ast::Expr::NumberLiteral(_literal) => {
+            ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
+                value: ast::Number::Int(_),
+                ..
+            }) => {
+                self.context.report_lint(
+                    &INVALID_TYPE_FORM,
+                    expression,
+                    format_args!("Int literal is not allowed in type expressions"),
+                );
+                Type::unknown()
+            }
+            ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
+                value: ast::Number::Float(_),
+                ..
+            }) => {
+                self.context.report_lint(
+                    &INVALID_TYPE_FORM,
+                    expression,
+                    format_args!("Float literal is not allowed in type expressions"),
+                );
+                Type::unknown()
+            }
+            ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
+                value: ast::Number::Complex { .. },
+                ..
+            }) => {
                 self.context.report_lint(
                     &INVALID_TYPE_FORM,
                     expression,
-                    format_args!("Number Literal is not allowed in type expressions"),
+                    format_args!("Complex literal is not allowed in type expressions"),
                 );
                 Type::unknown()
             }
```

---

_@AlexWaygood reviewed on 2025-03-16 21:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:52_

> Do you think we should keep the 'instance of float' check?

We do want to disallow `float` instances in type expressions. But I think we'll want to do it in a more general way than what you've got here right now. Nearly all `Instance` types are actually invalid in type expressions; the only `Instance` types that we want to _avoid_ emitting an error on are specific classes from the `typing` module such as `typing.TypeVar`, `typing.ParamSpec`, `typing.TypeVarTuple`, etc.

Overall I think I'd defer all that for this PR. It's a separate issue. So I'd revert the 'instance of float' check for now

---

_@MatthewMckee4 reviewed on 2025-03-16 21:52_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:50 on 2025-03-16 21:52_

Thanks

---

_@MatthewMckee4 reviewed on 2025-03-16 21:56_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:56_

Okay, removed it for now

---

_@MatthewMckee4 reviewed on 2025-03-16 21:57_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types.rs`:3280 on 2025-03-16 21:57_

How does that look?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6169 on 2025-03-16 22:01_

I think we could make this less repetitive by defining a small closure at the top of the method:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index b91beeec7..ddb729cce 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -6088,6 +6088,13 @@ impl<'db> TypeInferenceBuilder<'db> {
     /// Infer the type of a type expression without storing the result.
     fn infer_type_expression_no_store(&mut self, expression: &ast::Expr) -> Type<'db> {
         // https://typing.readthedocs.io/en/latest/spec/annotations.html#grammar-token-expression-grammar-type_expression
+
+        let report_invalid_type_expression = |message: std::fmt::Arguments| {
+            self.context
+                .report_lint(&INVALID_TYPE_FORM, expression, message);
+            Type::unknown()
+        };
+
         match expression {
             ast::Expr::Name(name) => match name.ctx {
                 ast::ExprContext::Load => self
@@ -6118,55 +6125,30 @@ impl<'db> TypeInferenceBuilder<'db> {
                 todo_type!("ellipsis literal in type expression")
             }
 
-            ast::Expr::BytesLiteral(_literal) => {
-                self.context.report_lint(
-                    &INVALID_TYPE_FORM,
-                    expression,
-                    format_args!("Bytes literal is not allowed in type expressions"),
-                );
-                Type::unknown()
-            }
+            ast::Expr::BytesLiteral(_literal) => report_invalid_type_expression(format_args!(
+                "Bytes literals are not allowed in type expressions"
+            )),
             ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
                 value: ast::Number::Int(_),
                 ..
-            }) => {
-                self.context.report_lint(
-                    &INVALID_TYPE_FORM,
-                    expression,
-                    format_args!("Int literal is not allowed in type expressions"),
-                );
-                Type::unknown()
-            }
+            }) => report_invalid_type_expression(format_args!(
+                "Int literals are not allowed in type expressions"
+            )),
             ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
                 value: ast::Number::Float(_),
                 ..
-            }) => {
-                self.context.report_lint(
-                    &INVALID_TYPE_FORM,
-                    expression,
-                    format_args!("Float literal is not allowed in type expressions"),
-                );
-                Type::unknown()
-            }
+            }) => report_invalid_type_expression(format_args!(
+                "Float literals are not allowed in type expressions"
+            )),
             ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
                 value: ast::Number::Complex { .. },
                 ..
-            }) => {
-                self.context.report_lint(
-                    &INVALID_TYPE_FORM,
-                    expression,
-                    format_args!("Complex literal is not allowed in type expressions"),
-                );
-                Type::unknown()
-            }
-            ast::Expr::BooleanLiteral(_literal) => {
-                self.context.report_lint(
-                    &INVALID_TYPE_FORM,
-                    expression,
-                    format_args!("Boolean literal is not allowed in type expressions"),
-                );
-                Type::unknown()
-            }
+            }) => report_invalid_type_expression(format_args!(
+                "Complex literals are not allowed in type expressions"
+            )),
+            ast::Expr::BooleanLiteral(_literal) => report_invalid_type_expression(format_args!(
+                "Boolean literals are not allowed in type expressions"
+            )),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:123 on 2025-03-16 22:04_

I'd be inclined to make these tests more concise, similar to the existing tests in this file, e.g.

````suggestion
## Invalid AST nodes

```py
def _(
    a: 1,  # error [invalid-type-form] "Int literals are not allowed in type expressions"
    b: 2.3,  # error [invalid-type-form] "Float literals are not allowed in type expressions"
    c: 4j,  # error [invalid-type-form] "Complex literals are not allowed in type expressions"
    d: True,  # error [invalid-type-form] "Boolean literals are not allowed in type expressions"
    e: b"foo",  # error [invalid-type-form] "Bytes literals are not allowed in type expressions"
):
    reveal_type(a)  # revealed: Unknown
    reveal_type(b)  # revealed: Unknown
    reveal_type(c)  # revealed: Unknown
    reveal_type(d)  # revealed: Unknown
    reveal_type(e)  # revealed: Unknown
```
````


---

_@AlexWaygood reviewed on 2025-03-16 22:05_

Looks close!

---

_@MatthewMckee4 reviewed on 2025-03-16 22:11_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:123 on 2025-03-16 22:11_

Looks good. with the byte string here, the error is:
```bash
[byte-string-type-annotation] "Type expressions cannot use bytes literal"
```
Hence why i had included "int |", what do you want to do?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:123 on 2025-03-16 22:13_

Oh, I see! Yes, using `int | b"a"` would work well for that one :-)

---

_@AlexWaygood reviewed on 2025-03-16 22:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6128 on 2025-03-16 22:20_

```suggestion
            ast::Expr::BytesLiteral(_) => report_invalid_type_expression(format_args!(
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6149 on 2025-03-16 22:21_

```suggestion
            ast::Expr::BooleanLiteral(_) => report_invalid_type_expression(format_args!(
```

---

_@AlexWaygood approved on 2025-03-16 22:21_

looks great, thank you!

---

_Comment by @AlexWaygood on 2025-03-16 22:25_

I'll wait until our CI's back up and running before merging this (everything's disabled at the moment due to https://github.com/astral-sh/ruff/issues/16768, we should get it up again tomorrow)

---

_Comment by @MatthewMckee4 on 2025-03-16 22:35_

Sounds good, thank you for the feedback!

---

_Closed by @MichaReiser on 2025-03-17 07:45_

---

_Reopened by @MichaReiser on 2025-03-17 07:45_

---

_Comment by @github-actions[bot] on 2025-03-17 07:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-03-17 08:06_

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

_Renamed from "Add invalid type expressions." to "[red-knot] Emit error if int/float/complex/bytes/boolean literals appear in type expressions outside `typing.Literal[]`" by @AlexWaygood on 2025-03-17 11:22_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:6130 on 2025-03-17 11:30_

Should we include `typing.Annotated` here as `Annotated[int, b"hello"]` is valid? Similar comment for other messages.

---

_@dhruvmanila reviewed on 2025-03-17 11:30_

---

_@AlexWaygood reviewed on 2025-03-17 11:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6130 on 2025-03-17 11:33_

We could do. It feels sort-of like a rare edge-case, though? I think maybe we can revisit this error message when we have subdiagnostics and make it more precise with additional notes.

(Another nice improvement would be to have an autofix in your editor that imports `typing.Literal` and changes the annotation from `b"foo"` to `Literal[b"foo"]`, since that's what you _probably_ meant)

---

_@dhruvmanila reviewed on 2025-03-17 11:38_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:6130 on 2025-03-17 11:38_

Yeah, I think including all possible context would become a bit too verbose i.e., including both Literal and Annotated. We could probably just say "Invalid type expression" or "Bytes literals are not allowed as type expressions in this context".

That said, we can move forward with you have here and can iterate later.

---

_@AlexWaygood reviewed on 2025-03-17 11:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6130 on 2025-03-17 11:39_

"in this context" sounds like a good compromise for now until we have subdiagnostics!

---

_Merged by @AlexWaygood on 2025-03-17 11:56_

---

_Closed by @AlexWaygood on 2025-03-17 11:56_

---

_Branch deleted on 2025-03-17 20:55_

---
