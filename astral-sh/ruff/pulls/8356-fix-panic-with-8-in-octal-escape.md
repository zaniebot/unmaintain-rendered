```yaml
number: 8356
title: Fix panic with 8 in octal escape
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: octal-escape
created_at: 2023-10-30T13:21:08Z
updated_at: 2023-10-30T13:47:21Z
url: https://github.com/astral-sh/ruff/pull/8356
synced_at: 2026-01-12T15:55:26Z
```

# Fix panic with 8 in octal escape

---

_@konstin_

**Summary** The digits for an octal escape are 0 to 7, not 0 to 8, fixing the panic in #8355

**Test plan** Regression test parser fixture

---

_Label `bug` added by @konstin on 2023-10-30 13:21_

---

_Label `parser` added by @konstin on 2023-10-30 13:21_

---

_@charliermarsh approved on 2023-10-30 13:23_

---

_Merged by @konstin on 2023-10-30 13:42_

---

_Closed by @konstin on 2023-10-30 13:42_

---

_Branch deleted on 2023-10-30 13:42_

---

_Comment by @github-actions[bot] on 2023-10-30 13:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---
