```yaml
number: 20612
title: "[`flake8-annotations`] Fix return type annotations to handle shadowed builtin symbols (`ANN201`, `ANN202`, `ANN204`, `ANN205`, `ANN206`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-20610
created_at: 2025-09-28T19:18:52Z
updated_at: 2025-10-02T22:48:12Z
url: https://github.com/astral-sh/ruff/pull/20612
synced_at: 2026-01-12T15:57:06Z
```

# [`flake8-annotations`] Fix return type annotations to handle shadowed builtin symbols (`ANN201`, `ANN202`, `ANN204`, `ANN205`, `ANN206`)

---

_@danparizher_

## Summary

Fixes #20610


---

_Comment by @github-actions[bot] on 2025-09-28 19:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dscorbett reviewed on 2025-09-30 19:54_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:271 on 2025-09-30 19:54_

`None` is a keyword: it shouldn’t be imported.

---

_Label `bug` added by @ntBre on 2025-10-01 17:05_

---

_Label `fixes` added by @ntBre on 2025-10-01 17:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/snapshots/ruff_linter__rules__flake8_annotations__tests__shadowed_builtins.snap`:63 on 2025-10-01 17:11_

This one should also be `builtins.str` right?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:147 on 2025-10-01 17:12_

nit: I think this is the same


```suggestion
                type_expr(python_type, checker, at)
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:228 on 2025-10-01 17:26_

Can we still preserve the original `name` helper? Possibly with a generic `impl Into<Name>` argument instead of `&str`? That seemed to help with some of the repetition here. It looks like we could possibly even include the `get_or_import_builtin` call in the helper.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:274 on 2025-10-01 17:29_

I don't think this should change anything in the output, but for convenience we should be able to return an `Expr::NoneLiteral(ExprNoneLiteral::default())` here.

---

_@ntBre reviewed on 2025-10-01 17:31_

Thanks! This looks good overall, just a couple of simplification suggestions and one question/possible missed case for the `@override` method.

---

_Review requested from @ntBre by @danparizher on 2025-10-01 23:14_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:831 on 2025-10-02 22:39_

This is a bit awkward because we just did basically the same `match` inside of `simple_magic_return_type`. I tried moving `simple_magic_return_type` into this file and making it return a `PythonType` directly, though, and then we don't have a nice way to get back to the string representation. So I think this is fine as-is.

---

_@ntBre approved on 2025-10-02 22:40_

Thank you! I pushed one commit moving the `name` helper back into `type_expr` just to keep it more consistent with the old code. I'm also not sure what I was thinking with the `impl Into<Name>` suggestion, so I just put that back to `&str` too 😅 

---

_Merged by @ntBre on 2025-10-02 22:44_

---

_Closed by @ntBre on 2025-10-02 22:44_

---

_Branch deleted on 2025-10-02 22:48_

---
