```yaml
number: 11623
title: "[red-knot] infer int literal types"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: intlit
created_at: 2024-05-30T23:25:51Z
updated_at: 2024-05-31T19:52:33Z
url: https://github.com/astral-sh/ruff/pull/11623
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] infer int literal types

---

_Pull request opened by @carljm on 2024-05-30 23:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Give red-knot the ability to infer int literal types. This is quick and easy, mostly because these types are a convenient way to observe control-flow handling with simple assignments.

## Test Plan

Added test.


---

_Comment by @github-actions[bot] on 2024-05-30 23:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @carljm on 2024-05-31 03:39_

---

_Review requested from @AlexWaygood by @carljm on 2024-05-31 03:39_

---

_Review requested from @plredmond by @carljm on 2024-05-31 03:40_

---

_Label `internal` added by @carljm on 2024-05-31 03:40_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:151 on 2024-05-31 06:46_

Could we change `Type::IntLiteral` to store a `ruff_python_ast::Int` instead of an `i64` to get support for big int literals with the optimization to avoid allocating for values that fit into `i64`?

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:155 on 2024-05-31 06:48_

I would probably use a match here since we want to handle all branches in the future (and rename `v` to `value`...)
```suggestion
					match value {
							ast::Number::Int(int) => Ok(n.as_i64().map(Type::IntLiteral).unwrap_or(Type::Unknown)),	
							ast::Number::Float(_) | ast::Number::Complex(_) => {
                // TODO builtins.float or builtins.complex
                Ok(Type::Unknown)
              }
          }
 ```

---

_@MichaReiser approved on 2024-05-31 06:48_

---

_@AlexWaygood reviewed on 2024-05-31 09:21_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:151 on 2024-05-31 09:21_

This caused issues for pyright as well, so it's definitely something we'll need to support eventually (but I don't think it *has* to be done in this PR):
- https://github.com/microsoft/pyright/issues/2860
- https://github.com/microsoft/pyright/issues/2879

Relevant pyright commit: https://github.com/microsoft/pyright/commit/65f444daecfa3884e082c6cd528c6bd5ee2102df

---

_@AlexWaygood reviewed on 2024-05-31 09:31_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:154 on 2024-05-31 09:31_

`builtins.complex` and `builtins.float` are not valid parameters to `Literal` in type expressions. But I suppose for our purposes, we need to hang on to the actual value of the symbol as well as the type, as many lint rules may find the actual value useful. But we may need to think carefully about how we display these types to users in diagnostic messages -- it might be confusing if we reported to a user that a variable has type `Literal[3.1234552]` or whatever, when that isn't a valid Python type according to the specification.

I think mypy does something similar where they have an internal representation for `Literal` floats; this is important for when they want to compile Python code with mypyc (not a concern for us). But that Literal type is never shown to users.

---

_@AlexWaygood approved on 2024-05-31 09:32_

LGTM, modulo comments

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:151 on 2024-05-31 17:32_

Sure, that's a good idea, I can make this change.

---

_@carljm reviewed on 2024-05-31 17:32_

---

_@carljm reviewed on 2024-05-31 17:34_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:154 on 2024-05-31 17:34_

`builtins.float` and `builtins.complex` here are meant to simply say that we should infer those general types (the type `float` or the type `complex`), not a `Literal` of those types.

I agree we may want to consider internally supporting literals of them as well, but this comment wasn't meant to imply anything about that.

Is there a way to word it that would make that clearer?

---

_@AlexWaygood reviewed on 2024-05-31 17:40_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:154 on 2024-05-31 17:40_

Right, sorry -- I was reading the TODO comment in the context of the PR's purpose and assumed you meant something to do with `Literal` types. But in the context of the function I think it's clear as-is; I just didn't look closely enough.

---

_@carljm reviewed on 2024-05-31 19:19_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:151 on 2024-05-31 19:19_

After further discussion, this would make `Type` non `Copy` which I don't want to do -- I think we will need to intern bignum literals (as well as e.g. string literals). So I'm going to leave this as-is for now.

---

_Merged by @carljm on 2024-05-31 19:52_

---

_Closed by @carljm on 2024-05-31 19:52_

---

_Branch deleted on 2024-05-31 19:52_

---
