```yaml
number: 16822
title: "[red-knot] Emit errors for more AST nodes that are invalid (or only valid in specific contexts) in type expressions"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: emit-errors-for-invalid-type-expressions
created_at: 2025-03-17T21:33:18Z
updated_at: 2025-03-18T17:21:49Z
url: https://github.com/astral-sh/ruff/pull/16822
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Emit errors for more AST nodes that are invalid (or only valid in specific contexts) in type expressions

---

_Pull request opened by @MatthewMckee4 on 2025-03-17 21:33_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add error messages for invalid nodes in type expressions

Fixes #16816 

## Test Plan

Extend annotations/invalid.md to handle these invalid AST nodes error messages 


---

_@MatthewMckee4 reviewed on 2025-03-17 21:34_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:6036 on 2025-03-17 21:34_

@AlexWaygood What do you think of making this a instance method?

---

_@MatthewMckee4 reviewed on 2025-03-17 21:35_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:6036 on 2025-03-17 21:35_

I was having issues with mutable references in ast::Expr::BoolOp as infer_boolean_expression requires a mutable reference to self, and i believe the closure does too

---

_Comment by @github-actions[bot] on 2025-03-17 21:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-03-17 21:56_

---

_Comment by @MatthewMckee4 on 2025-03-17 22:16_

I havent covered slice type expression yet

---

_Comment by @MatthewMckee4 on 2025-03-17 22:18_

I believe tests failing is mainly due to Callable type inference calls this function with tuple expression instead of the individual arguments, ill look into this more

---

_Comment by @MatthewMckee4 on 2025-03-17 22:51_

The errors mainly stem from "Tuples are not allowed in type expressions"

In generic arguments with > 1 generic parameter

I'm not really sure the best way to handle this.

Perhaps we can create a new function that can call infer_type_expression_no_store if no tuples are expected in the Expr and else it can try to "flatten" the type and / or call infer_type_expression_no_store on each type in the Tuple
*or infer_type_expression

@AlexWaygood What are your thoughts?

---

_Comment by @dhruvmanila on 2025-03-18 03:22_

> I believe tests failing is mainly due to Callable type inference calls this function with tuple expression instead of the individual arguments, ill look into this more

Yeah, I haven't looked at the test failures but I think those are correct because we fallback to calling `infer_type_expression` for the slice expression if it's an invalid type form for a specific special form like `Callable`. For example: https://github.com/astral-sh/ruff/blob/f9ee155874a3b5e5d875f41f80367a70243630f5/crates/red_knot_python_semantic/src/types/infer.rs#L6584-L6590

This is a requirement because currently the tests require that every expression has an inferred type even in the case of failure: https://github.com/astral-sh/ruff/blob/f9ee155874a3b5e5d875f41f80367a70243630f5/crates/red_knot_project/tests/check.rs#L165-L171

---

_Comment by @MatthewMckee4 on 2025-03-18 11:44_

```rust
    fn infer_type_expression_or_tuple_type_expression(
        &mut self,
        expression: &ast::Expr,
    ) -> Type<'db> {
        match expression {
            ast::Expr::Tuple(ast::ExprTuple {
                elts: expressions, ..
            }) => Type::Tuple(TupleType::new(
                self.db(),
                expressions
                    .iter()
                    .map(|expression| self.infer_type_expression(expression))
                    .collect::<Box<_>>(),
            )),
            _ => self.infer_type_expression(expression),
        }
    }
 ```

This was sort of what i was thinking, and anywhere we call infer_type_expression and want to allow tuple expressions, we can call this. This is useful for
```rust
            KnownInstanceType::Dict => {
                self.infer_type_expression(arguments_slice);
                KnownClass::Dict.to_instance(self.db())
            }
```

Where if we used infer_type_expression_or_tuple_type_expression, we can actually get the Type of the tuple back, for the future as these generics are currently not supported. But for KnownInstaneType::List, we continue to use infer_type_expression

---

_Comment by @AlexWaygood on 2025-03-18 11:49_

I'd be inclined to postpone the complicated ones like `Expr::Tuple` and `Expr::List` for now and just do the easy ones in this PR -- we can tackle the complicated ones in followup(s). PRs are cheap! :-)

---

_Comment by @MatthewMckee4 on 2025-03-18 11:50_

Okay can revert them to no error message

---

_Marked ready for review by @MatthewMckee4 on 2025-03-18 12:01_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-18 12:01_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-18 12:01_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-18 12:01_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-18 12:01_

---

_Comment by @github-actions[bot] on 2025-03-18 12:01_

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

_@carljm approved on 2025-03-18 14:30_

---

_Comment by @carljm on 2025-03-18 14:31_

LGTM, thank you! Will leave it for @AlexWaygood to merge in case he has any review comments.

---

_Comment by @MatthewMckee4 on 2025-03-18 15:05_

I need to test Slice node and ill also improve some error messages

---

_Comment by @carljm on 2025-03-18 15:46_

FWIW I'm not personally a big fan of providing arbitrary "examples" in the error messages ("like `1 := 'foo'`", "like `1 < 2`") in the error messages. It seems a bit arbitrary what kind of example to select, it seems confusing to me to give specific code examples that don't actually come from the user's code, and it seems unnecessary because, by definition, we already have a suitable example at hand: the one from the user's code that we are emitting the diagnostic on!

So I would be inclined to remove these from the error messages, but open to what others think.

---

_Comment by @MatthewMckee4 on 2025-03-18 15:50_

That's fair, my thinking was that "Unary operations", and others, could a bit vague for some users.

Then again, you could argue, people who find themselves writing these examples in type expressions, probably know what they are called.

I'm happy to revert either way

---

_Comment by @carljm on 2025-03-18 15:52_

I think the strongest argument against is that the diagnostic will literally always be pointing to a specific example in the user's code, which seems like it should make things pretty clear.

Given Alex's thumbs-up above, yeah I think you should go ahead and remove these from the messages.

---

_@AlexWaygood approved on 2025-03-18 17:11_

Thank you, this is great! I pushed a couple of nitpick changes, but nothing major.

---

_Merged by @AlexWaygood on 2025-03-18 17:16_

---

_Closed by @AlexWaygood on 2025-03-18 17:16_

---

_Branch deleted on 2025-03-18 17:18_

---
