```yaml
number: 8447
title: "Don't build the maturin test"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/no-pyo3
created_at: 2024-10-22T12:10:19Z
updated_at: 2024-10-22T12:17:29Z
url: https://github.com/astral-sh/uv/pull/8447
synced_at: 2026-01-12T16:08:19Z
```

# Don't build the maturin test

---

_@konstin_

Building pyo3 is slow, and we don't implement a maturin cffi template (not enough usage), so we just skip the building pyo3 stage in the maturin template tests.

Closes #8404

---

_Label `internal` added by @konstin on 2024-10-22 12:10_

---

_@zanieb approved on 2024-10-22 12:12_

---

_Merged by @konstin on 2024-10-22 12:17_

---

_Closed by @konstin on 2024-10-22 12:17_

---

_Branch deleted on 2024-10-22 12:17_

---
