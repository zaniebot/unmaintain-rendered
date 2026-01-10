```yaml
number: 2273
title: Fix parsing requirement where a variable follows an operator without a space
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-parsing-req-variable-after-op-no-space
created_at: 2024-03-07T12:12:13Z
updated_at: 2024-03-07T12:22:17Z
url: https://github.com/astral-sh/uv/pull/2273
synced_at: 2026-01-10T14:54:43Z
```

# Fix parsing requirement where a variable follows an operator without a space

---

_Pull request opened by @konstin on 2024-03-07 12:12_

Fix parsing `pytest;'4.0'>=python_version`, where previously the operator and the variable were incorrectly tokenized as one invalid operator.

Fixes #2247

---

_Label `bug` added by @konstin on 2024-03-07 12:12_

---

_Merged by @konstin on 2024-03-07 12:22_

---

_Closed by @konstin on 2024-03-07 12:22_

---

_Branch deleted on 2024-03-07 12:22_

---
