```yaml
number: 8578
title: Fix Python install path in tests
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/python-install
created_at: 2024-10-25T22:14:17Z
updated_at: 2024-10-27T01:56:53Z
url: https://github.com/astral-sh/uv/pull/8578
synced_at: 2026-01-12T16:08:23Z
```

# Fix Python install path in tests

---

_@zanieb_

The shared arguments were resetting the `UV_PYTHON_INSTALL_DIR`, breaking the intent of the test.

Added some uninstall tests too, needed later for #8571 

---

_Label `internal` added by @zanieb on 2024-10-25 22:14_

---

_Merged by @zanieb on 2024-10-27 01:56_

---

_Closed by @zanieb on 2024-10-27 01:56_

---

_Branch deleted on 2024-10-27 01:56_

---
