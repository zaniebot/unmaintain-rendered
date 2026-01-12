```yaml
number: 6121
title: "Consolidate `ExprYield`/`ExprYieldFrom` into `AnyExpressionYield`"
type: pull_request
state: closed
author: qdegraaf
labels: []
assignees: []
base: main
head: ref/anyexpryield
created_at: 2023-07-27T10:42:08Z
updated_at: 2023-07-27T15:26:56Z
url: https://github.com/astral-sh/ruff/pull/6121
synced_at: 2026-01-12T15:55:20Z
```

# Consolidate `ExprYield`/`ExprYieldFrom` into `AnyExpressionYield`

---

_@qdegraaf_

## Summary

Removes some duplicated logic by consolidating both `yield` expressions into one enum, as suggested by @MichaReiser in https://github.com/astral-sh/ruff/pull/5921#pullrequestreview-1541019917 

## Test Plan

Existing tests and functionality were checked to be unchanged, this is a pure refactor PR


---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_yield.rs`:86 on 2023-07-27 11:16_

nit: rename this to something like `keyword`, otherwise it's confusing since the value represented is not an expression, while `val` is an expression

---

_@konstin approved on 2023-07-27 11:16_

---

_@MichaReiser reviewed on 2023-07-27 13:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_yield.rs`:9 on 2023-07-27 13:40_

Nit: You should get `Formatter, `Format`, `FormatResult`, and all the inports from `ruff_formatter::prelude` automatically by using `crate::prelude::*` (Meaning, we could remove those)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_yield.rs`:10 on 2023-07-27 13:40_

You have to rebase past my parser refactor. This is now
```suggestion
use ruff_python_ast::{Expr, ExprYield, ExprYieldFrom, Ranged};
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:227 on 2023-07-27 13:42_

Nit: I would keep the `impl NeedsParentheses for ExprYield` and `ExprYieldFrom` so that callers (including the call here) don't need to know that the implementation is shared by calling into `AnyExpressionYield::from(expr).needs_parentheses`

---

_@MichaReiser approved on 2023-07-27 13:42_

---

_Comment by @zanieb on 2023-07-27 14:32_

Hey @qdegraaf — sorry about this but we needed to remove a commit from the main branch so I've changed the base of your branch to fix the history. Please let me know if you have any questions or problems!

---

_Closed by @qdegraaf on 2023-07-27 15:12_

---

_Comment by @qdegraaf on 2023-07-27 15:26_

> Hey @qdegraaf — sorry about this but we needed to remove a commit from the main branch so I've changed the base of your branch to fix the history. Please let me know if you have any questions or problems!

Hey @zanieb. Thanks for the heads up. I got too trigger happy anyway and refreshed my fork forgetting I had a couple other PRs open as well. Reopened this one in: https://github.com/astral-sh/ruff/pull/6127

FYI @konstin @MichaReiser due to previous reviews

---
