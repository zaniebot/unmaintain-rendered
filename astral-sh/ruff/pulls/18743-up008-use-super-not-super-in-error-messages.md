```yaml
number: 18743
title: "[UP008]: use `super()`, not `__super__` in error messages"
type: pull_request
state: merged
author: sobolevn
labels:
  - documentation
assignees: []
merged: true
base: main
head: UP008-error-message
created_at: 2025-06-18T06:35:50Z
updated_at: 2025-06-18T17:57:57Z
url: https://github.com/astral-sh/ruff/pull/18743
synced_at: 2026-01-12T15:56:24Z
```

# [UP008]: use `super()`, not `__super__` in error messages

---

_@sobolevn_

When I try to grep CPython with `__super__` I get 0 results:

```
(.venv) ~/Desktop/cpython  main ✔                                                    
» ag __super__ . 
                
```

That's how we can understand that the naming is not the best.

---

_Comment by @github-actions[bot] on 2025-06-18 06:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-06-18 17:52_

---

_@ntBre approved on 2025-06-18 17:56_

Thank you!

---

_Merged by @ntBre on 2025-06-18 17:57_

---

_Closed by @ntBre on 2025-06-18 17:57_

---
