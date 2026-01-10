```yaml
number: 1918
title: Generate versioned pip launchers
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: konsti/special-case-pip-entrypoint-names
created_at: 2024-02-23T15:29:43Z
updated_at: 2024-04-29T13:02:06Z
url: https://github.com/astral-sh/uv/pull/1918
synced_at: 2026-01-10T14:37:54Z
```

# Generate versioned pip launchers

---

_Pull request opened by @konstin on 2024-02-23 15:29_

Users expect pip to have `pip`, `pip3` and `pip3.x` entrypoints. But pip is a universal wheel, so it contains the `pip3.x` entrypoint where it was built on. To fix this, pip special cases itself when installing (https://github.com/pypa/pip/blob/3898741e29b7279e7bffe044ecfbe20f6a438b1e/src/pip/_internal/operations/install/wheel.py#L283), replacing the wheel entrypoint with one for the current version. We now do the same.

Fixes #1593

---

_Label `bug` added by @konstin on 2024-02-23 15:29_

---

_Label `compatibility` added by @konstin on 2024-02-23 15:29_

---

_@zanieb approved on 2024-02-23 15:51_

---

_Merged by @konstin on 2024-02-23 18:01_

---

_Closed by @konstin on 2024-02-23 18:01_

---

_Branch deleted on 2024-02-23 18:01_

---

_Branch restored on 2024-02-23 18:06_

---

_Branch deleted on 2024-04-29 13:02_

---
