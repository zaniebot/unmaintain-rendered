```yaml
number: 19388
title: "[`flake8-use-pathlib`] Fix false negative on direct `Path()` instantiation (`PTH210`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-19329
created_at: 2025-07-16T18:20:35Z
updated_at: 2025-07-16T20:50:50Z
url: https://github.com/astral-sh/ruff/pull/19388
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-use-pathlib`] Fix false negative on direct `Path()` instantiation (`PTH210`)

---

_Pull request opened by @danparizher on 2025-07-16 18:20_

## Summary

Fixes #19329


---

_Comment by @github-actions[bot] on 2025-07-16 18:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-07-16 20:13_

---

_Label `rule` added by @ntBre on 2025-07-16 20:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:169 on 2025-07-16 20:25_

I think we can reuse part of the implementation for `is_pathlib_path` for this:


```suggestion
        expr => PathlibPathChecker::match_initializer(expr, semantic),
```

(with this diff at the top too):

```diff
-use ruff_python_semantic::analyze::typing;
+use ruff_python_semantic::analyze::typing::{self, PathlibPathChecker, TypeChecker};
```

---

_@ntBre approved on 2025-07-16 20:28_

Thanks! I just had one simplification suggestion by using the `typing` trait method.

---

_Renamed from "[`flake8_use_pathlib`] Fix false negative for PTH210 on direct `Path()` instantiation with `with_suffix`" to "[`flake8-use-pathlib`] Fix false negative on direct `Path()` instantiation (`PTH210`)" by @ntBre on 2025-07-16 20:30_

---

_Merged by @ntBre on 2025-07-16 20:43_

---

_Closed by @ntBre on 2025-07-16 20:43_

---

_Branch deleted on 2025-07-16 20:47_

---
