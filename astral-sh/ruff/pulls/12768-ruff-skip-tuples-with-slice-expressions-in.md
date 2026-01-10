```yaml
number: 12768
title: "[ruff] Skip tuples with slice expressions in `incorrectly-parenthesized-tuple-in-subscript (RUF031)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: ruf031-slice-in-subscript
created_at: 2024-08-09T01:29:14Z
updated_at: 2024-08-15T02:44:05Z
url: https://github.com/astral-sh/ruff/pull/12768
synced_at: 2026-01-10T21:38:32Z
```

# [ruff] Skip tuples with slice expressions in `incorrectly-parenthesized-tuple-in-subscript (RUF031)`

---

_Pull request opened by @dylwil3 on 2024-08-09 01:29_

## Summary

Adding parentheses to a tuple in a subscript with elements that include slice expressions causes a syntax error.  For example, `d[(1,2,:)]` is a syntax error.

So, when `lint.ruff.parenthesize-tuple-in-subscript = true` and the tuple includes a slice expression, we skip this check and fix.

Closes #12766.


---

_Comment by @dylwil3 on 2024-08-09 01:30_

Probably should wait until #12762 is merged in and the subsequent merge conflicts have been resolved.

---

_Label `bug` added by @charliermarsh on 2024-08-09 01:30_

---

_Comment by @charliermarsh on 2024-08-09 01:30_

Thanks @dylwil3!

---

_Comment by @github-actions[bot] on 2024-08-09 01:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-09 06:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-09 06:10_

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-08-09 06:10_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs`:68 on 2024-08-09 09:18_

This is a bitwise `&` but I think we want a boolean `&`

```suggestion
    if prefer_parentheses && tuple_subscript.elts.iter().any(Expr::is_slice_expr) {
```

---

_@AlexWaygood reviewed on 2024-08-09 09:18_

---

_@AlexWaygood approved on 2024-08-09 09:18_

Thanks!

---

_Comment by @AlexWaygood on 2024-08-09 09:21_

The fix being applied in this PR is very specific. But I think that's okay, because I think this syntax error can only occur due to the very specific details of literal slices inside subscripts. So I don't think the bug report reveals a deeper underlying issue that we need to fix.

---

_Merged by @AlexWaygood on 2024-08-09 09:22_

---

_Closed by @AlexWaygood on 2024-08-09 09:22_

---

_Branch deleted on 2024-08-15 02:44_

---
