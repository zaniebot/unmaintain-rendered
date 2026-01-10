```yaml
number: 15919
title: "[`pyupgrade`] Unwrap unary expressions correctly (`UP018`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: UP018
created_at: 2025-02-04T00:26:37Z
updated_at: 2025-02-14T08:23:46Z
url: https://github.com/astral-sh/ruff/pull/15919
synced_at: 2026-01-10T19:57:22Z
```

# [`pyupgrade`] Unwrap unary expressions correctly (`UP018`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-04 00:26_

## Summary

Resolves #15859.

The rule now adds parentheses if the original call wraps an unary expression and is:

* The left-hand side of a binary expression where the operator is `**`.
* The caller of a call expression.
* The subscripted of a subscript expression.
* The object of an attribute access.

The fix will also be marked as unsafe if there are any comments in its range.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2025-02-04 00:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2025-02-04 01:17_

Another case to consider is `await int(-1)`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:257 on 2025-02-04 09:04_

Could we reuse the `Precedence` infrastructure introduced in https://github.com/astral-sh/ruff/pull/15762 over implementing the correct precedence manually here?

---

_@MichaReiser reviewed on 2025-02-04 09:04_

---

_@InSyncWithFoo reviewed on 2025-02-07 22:13_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:257 on 2025-02-07 22:13_

I'm not sure how to use that `OperatorPrecedence` to differentiate `foo(int(-1))` and `int(-1)()`.

By the way, there are now four `OperatorPrecedence`s around the codebase: two in the formatter, one in the parser and one in the linter.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:257 on 2025-02-10 08:56_

https://github.com/astral-sh/ruff/issues/16071

---

_@MichaReiser reviewed on 2025-02-10 08:56_

---

_@MichaReiser reviewed on 2025-02-10 09:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:257 on 2025-02-10 09:05_

Couldn't we use the precedence to represent `int`'s position? I think it's: For the second it's `CallAttribute`, for the first it's parenthesized (may require adding a new operator precedence)

---

_@MichaReiser approved on 2025-02-14 07:41_

Nice. Thanks for looking into how to make this work with `OperatorPrecedence`

---

_Label `bug` added by @MichaReiser on 2025-02-14 07:41_

---

_Label `fixes` added by @MichaReiser on 2025-02-14 07:41_

---

_Merged by @MichaReiser on 2025-02-14 07:42_

---

_Closed by @MichaReiser on 2025-02-14 07:42_

---

_Branch deleted on 2025-02-14 08:23_

---
