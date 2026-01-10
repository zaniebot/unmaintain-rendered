```yaml
number: 14590
title: "[red-knot] Fix Leaking Narrowing Constraint in `ast::ExprIf`"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-leaked-narrowing-for-if-expr
created_at: 2024-11-25T17:40:42Z
updated_at: 2024-11-25T18:41:08Z
url: https://github.com/astral-sh/ruff/pull/14590
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Fix Leaking Narrowing Constraint in `ast::ExprIf`

---

_Pull request opened by @cake-monotone on 2024-11-25 17:40_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #14588


```py
x: Literal[42, "hello"] = 42 if bool_instance() else "hello"
reveal_type(x)  # revealed: Literal[42] | Literal["hello"]

_ = ... if isinstance(x, str) else ...

# The `isinstance` test incorrectly narrows the type of `x`.
# As a result, `x` is revealed as Literal["hello"], but it should remain Literal[42, "hello"].
reveal_type(x)  # revealed: Literal["hello"]
```

## Test Plan
mdtest included!


---

_Review requested from @carljm by @cake-monotone on 2024-11-25 17:40_

---

_Review requested from @MichaReiser by @cake-monotone on 2024-11-25 17:40_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-11-25 17:40_

---

_Review requested from @sharkdp by @cake-monotone on 2024-11-25 17:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/expression/if.md`:30 on 2024-11-25 17:43_

micro-nit: `if` tests do not create new scopes in Python, unlike e.g. in Rust, so we have to use the vaguer term "block" here

```suggestion
The test inside an if expression should not affect code outside of the block.
```

---

_Label `red-knot` added by @MichaReiser on 2024-11-25 17:45_

---

_Comment by @github-actions[bot] on 2024-11-25 17:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-11-25 17:53_

LGTM (though David and Carl have done much more work in this area, so I'll wait for feedback from one of them before merging).

Great catch, thank you!

---

_@cake-monotone reviewed on 2024-11-25 18:04_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/expression/if.md`:30 on 2024-11-25 18:04_

Thanks! :)

---

_@carljm approved on 2024-11-25 18:36_

Great catch, thank you!!

---

_Merged by @carljm on 2024-11-25 18:36_

---

_Closed by @carljm on 2024-11-25 18:36_

---

_Branch deleted on 2024-11-25 18:41_

---
