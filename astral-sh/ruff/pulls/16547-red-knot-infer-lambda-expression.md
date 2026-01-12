```yaml
number: 16547
title: "[red-knot] Infer `lambda` expression"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/infer-lambda-expr
created_at: 2025-03-07T07:19:55Z
updated_at: 2025-03-11T05:55:22Z
url: https://github.com/astral-sh/ruff/pull/16547
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Infer `lambda` expression

---

_@dhruvmanila_

## Summary

Part of https://github.com/astral-sh/ruff/issues/15382

This PR adds support for inferring the `lambda` expression and return the `CallableType`.

Currently, this is only limited to inferring the parameters and a todo type for the return type.

For posterity, I tried using the `file_expression_type` to infer the return type of lambda but it would always lead to cycle. The main reason is that in `infer_parameter_definition`, the default expression is being inferred using `file_expression_type`, which is correct, but it then 

Take the following source code as an example:
```py
lambda x=1: x
```

Here's how the code will flow:
* `infer_scope_types` for the global scope
* `infer_lambda_expression`
* `infer_expression` for the default value `1`
* `file_expression_type` for the return type using the body expression. This is because the body creates it's own scope
* `infer_scope_types` (lambda body scope)
* `infer_name_load` for the symbol `x` whose visible binding is the lambda parameter `x`
* `infer_parameter_definition` for parameter `x`
* `file_expression_type` for the default value `1`
* `infer_scope_types` for the global scope because of the default expression

This will then reach to `infer_definition` for the parameter `x` again which then creates the cycle.

## Test Plan

Add tests around `lambda` expression inference.

---

_Label `red-knot` added by @dhruvmanila on 2025-03-07 07:19_

---

_Marked ready for review by @dhruvmanila on 2025-03-10 12:43_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-10 12:43_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-10 12:43_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-10 12:43_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-10 12:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/expression/lambda.md`:1 on 2025-03-10 14:19_

Could we also add a test like this?

```py
lambda x=1: reveal_type(x)  # revealed: Unknown | Literal[1]
```

to demonstrate that it works the same way as `def` functions?

I also wondered about adding tests that show `lambda`s being called correctly and incorrectly such as

```py
y = lambda x: "foo"
y(42)
y(42, 42)
```

But it looks like that hasn't been implemented yet (which is fine, of course!), so I'm happy to leave that for now

---

_@AlexWaygood approved on 2025-03-10 14:20_

This looks great, thanks!

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:129 on 2025-03-10 16:26_

This is similar to `infer_definition_types` and it allowed me to quickly understand the origin of the cycle and what was causing it.

---

_@dhruvmanila reviewed on 2025-03-10 16:26_

---

_@dhruvmanila reviewed on 2025-03-10 16:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/expression/lambda.md`:1 on 2025-03-10 16:28_

Yeah, I can add that. I actually had that in my playground file but I forgot to transfer that to the mdtest. Thanks for the suggestion.

> I also wondered about adding tests that show `lambda`s being called correctly and incorrectly such as

Yeah, this isn't implemented yet and will tackle in a follow-up.

---

_Comment by @github-actions[bot] on 2025-03-11 04:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-03-11 05:26_

Looks great, thank you!

---

_Comment by @carljm on 2025-03-11 05:27_

As we discussed in 1:1, we can infer a return type in some cases if we make default values of the parameters standalone expressions, so they can be resolved without creating a cycle.

Longer term, we may want to do "inlining" of lambdas at their usage sites so we can do better transparent inference, but that's for later.

---

_Merged by @dhruvmanila on 2025-03-11 05:55_

---

_Closed by @dhruvmanila on 2025-03-11 05:55_

---

_Branch deleted on 2025-03-11 05:55_

---
