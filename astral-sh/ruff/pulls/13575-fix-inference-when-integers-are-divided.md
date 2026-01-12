```yaml
number: 13575
title: Fix inference when integers are divided
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: zb/rk-int-div
created_at: 2024-09-30T18:52:55Z
updated_at: 2024-09-30T20:50:38Z
url: https://github.com/astral-sh/ruff/pull/13575
synced_at: 2026-01-12T15:55:44Z
```

# Fix inference when integers are divided

---

_@zanieb_

Fixes the `Operator::Div` case and adds `Operator::FloorDiv` support

Closes https://github.com/astral-sh/ruff/issues/13570

---

_Label `bug` added by @zanieb on 2024-09-30 18:52_

---

_Label `red-knot` added by @zanieb on 2024-09-30 18:52_

---

_Review requested from @carljm by @zanieb on 2024-09-30 18:52_

---

_Review requested from @MichaReiser by @zanieb on 2024-09-30 18:52_

---

_Review requested from @AlexWaygood by @zanieb on 2024-09-30 18:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3996 on 2024-09-30 19:51_

We should emit a diagnostic for this. Shouldn't be hard to do. It can be in a later PR, but let's at least add a TODO for it, either here in the test or in the code above.

---

_Comment by @github-actions[bot] on 2024-09-30 19:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4009 on 2024-09-30 20:03_

You're hitting exactly the overflow you expect, but you're hitting it sooner than you expect, because `i64::MIN` is a negative number, meaning in the Python AST it turns into a positive number with unary minus, and the positive number overflows i64, so it falls back to `int` instead of `LiteralInt` already in `infer_number_literal_expression`.

But then our current implementation of `infer_unary_expression` only has cases for literals, and otherwise falls back to `Unknown`, so by the time you get to your floor division, one of the operands is already unknown.

So I don't think you can use this case to check division overflow; it's really a test of integer-literal overflow and unary-op handling of non-literal types. I don't think there is actually any way to hit division-overflow with literal integer operands, so I would just remove this from the test.

---

_@carljm approved on 2024-09-30 20:04_

Awesome, thank you!

---

_@zanieb reviewed on 2024-09-30 20:14_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:3996 on 2024-09-30 20:14_

Will do. I saw another TODO around division by zero.

---

_@zanieb reviewed on 2024-09-30 20:27_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:4009 on 2024-09-30 20:27_

Great thanks for the thorough explanation — you saved me a lot of time! I'm trying to test the `.checked_div(m)...unwrap_or_else` block and I guess we can just assume that we'll only fail there from division by zero. I'll add a comment.

---

_@carljm approved on 2024-09-30 20:33_

Excellent!

---

_Merged by @zanieb on 2024-09-30 20:50_

---

_Closed by @zanieb on 2024-09-30 20:50_

---

_Branch deleted on 2024-09-30 20:50_

---
