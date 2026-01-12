```yaml
number: 19656
title: "[`flake8-errmsg`] Exclude `typing.cast` from `EM101`"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19596
created_at: 2025-07-31T02:49:42Z
updated_at: 2025-08-01T17:37:45Z
url: https://github.com/astral-sh/ruff/pull/19656
synced_at: 2026-01-12T15:56:44Z
```

# [`flake8-errmsg`] Exclude `typing.cast` from `EM101`

---

_@danparizher_

## Summary

Fixes #19596


---

_Comment by @github-actions[bot] on 2025-07-31 02:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-31 12:56_

---

_Label `bug` added by @ntBre on 2025-07-31 12:56_

---

_Label `fixes` added by @ntBre on 2025-07-31 12:56_

---

_Renamed from "[`flake8_errmsg`] Exclude `typing.cast` from `EM101`" to "[`flake8-errmsg`] Exclude `typing.cast` from `EM101`" by @ntBre on 2025-07-31 14:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs`:189 on 2025-07-31 16:27_

nit: this looks identical to the `if-let` below. Can we just add `func` to the destructuring there and return from inside the `if`?

It looks like this whole function would be nicer as a sequence of early returns:

```rust
let Expr::Call ... else return;
if checker.semantic().match_typing_expr(func, cast) return;
let Some(first) = args.first() else return;
```

but let's save that for a quick follow-up.

---

_@ntBre approved on 2025-07-31 16:30_

Thanks! Just one small simplification suggestion and an idea for a follow-up, if you're interested.

---

_Review requested from @ntBre by @danparizher on 2025-07-31 22:51_

---

_@ntBre approved on 2025-08-01 17:37_

---

_Merged by @ntBre on 2025-08-01 17:37_

---

_Closed by @ntBre on 2025-08-01 17:37_

---
