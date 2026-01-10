```yaml
number: 20201
title: "[`flake8-comprehensions`] Skip `C417` when lambda contains `yield`/`yield from`"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-20198
created_at: 2025-09-02T01:18:01Z
updated_at: 2025-09-03T21:15:25Z
url: https://github.com/astral-sh/ruff/pull/20201
synced_at: 2026-01-10T17:46:21Z
```

# [`flake8-comprehensions`] Skip `C417` when lambda contains `yield`/`yield from`

---

_Pull request opened by @danparizher on 2025-09-02 01:18_

## Summary

Fixes #20198


---

_Comment by @github-actions[bot] on 2025-09-02 01:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:85 on 2025-09-03 15:57_

nit: We might want to strip this down a bit like the other test cases:


```suggestion
map(lambda x: (yield x), [1, 2, 3])
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:126 on 2025-09-03 15:58_

Do you have a case in mind where it only changes the semantics? I thought it would always cause a syntax error.

---

_@ntBre reviewed on 2025-09-03 15:59_

Thank you! Just one nit about the test case. Nice use of `any_over_expr`!

---

_Label `bug` added by @ntBre on 2025-09-03 15:59_

---

_@danparizher reviewed on 2025-09-03 20:21_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:126 on 2025-09-03 20:21_

You're right, I'll update the comment to make it more accurate.

---

_@ntBre approved on 2025-09-03 20:30_

Thanks!

---

_Renamed from "[`flake8-comprehensions`] Skip `C417` when lambda contains `yield`/`yield from` (`C417`)" to "[`flake8-comprehensions`] Skip `C417` when lambda contains `yield`/`yield from`" by @ntBre on 2025-09-03 20:39_

---

_Merged by @ntBre on 2025-09-03 20:39_

---

_Closed by @ntBre on 2025-09-03 20:39_

---

_Branch deleted on 2025-09-03 21:15_

---
