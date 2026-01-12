```yaml
number: 7362
title: "Remove `ExprContext` from `ComparableExpr`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comparable
created_at: 2023-09-13T19:07:13Z
updated_at: 2023-09-14T15:47:01Z
url: https://github.com/astral-sh/ruff/pull/7362
synced_at: 2026-01-12T02:39:09Z
```

# Remove `ExprContext` from `ComparableExpr`

---

_Pull request opened by @charliermarsh on 2023-09-13 19:07_

`ComparableExpr` includes the `ExprContext` field on an expression, so, e.g., the two tuples in `(a, b) = (a, b)` won't be considered equal. Similarly, the tuples in `[(a, b) for (a, b) in c]` _also_ wouldn't be considered equal. I find this behavior surprising, since `ComparableExpr` is intended to allow you to compare two ASTs, but `ExprContext` is really encoding information about the broader context for the expression.

---

_Label `internal` added by @charliermarsh on 2023-09-13 19:07_

---

_Comment by @MichaReiser on 2023-09-13 19:12_

Before I review this PR. Can we use this as a chance to document what equality under `CompareExpr` means? I never fully understood how the equality is defined. Is it: that the two expressions are likely to evaluate to the same value, considering that they run in the same context, the trees have the same shape, the source is identical, or some other equality?

---

_Comment by @charliermarsh on 2023-09-13 19:33_

Two expressions are considered equal if "the trees have the same shape". They do not have to be source-identical, and they can even differ in their ASTs in ways that don't affect the resulting _value_ (for example, we already ignore the `is_implicit_concatenated` field when comparing strings).

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-13 22:51_

---

_Comment by @github-actions[bot] on 2023-09-13 23:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:6 on 2023-09-14 06:29_

Thanks. Can we be a bit more explicit about the same shape. Let's say we change our string types to store the implicit concatenation parts. Would two strings with different parts then be considered unequal because the AST now has a different shape?

---

_@MichaReiser approved on 2023-09-14 06:31_

---

_Merged by @charliermarsh on 2023-09-14 15:40_

---

_Closed by @charliermarsh on 2023-09-14 15:40_

---

_Branch deleted on 2023-09-14 15:40_

---
