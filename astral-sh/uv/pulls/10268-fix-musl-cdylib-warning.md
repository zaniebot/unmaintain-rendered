```yaml
number: 10268
title: Fix musl cdylib warning
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cdylib-warning
created_at: 2025-01-02T09:50:03Z
updated_at: 2025-01-02T14:40:40Z
url: https://github.com/astral-sh/uv/pull/10268
synced_at: 2026-01-12T16:09:12Z
```

# Fix musl cdylib warning

---

_@konstin_

The `cdylib` was used for the pyo3 bindings to uv-pep508, which don't exist anymore. It was now creating warnings on musl due to musl (statically linked) no supporting shared libraries.

---

_Assigned to @konstin by @konstin on 2025-01-02 09:50_

---

_Label `internal` added by @konstin on 2025-01-02 09:50_

---

_Merged by @charliermarsh on 2025-01-02 14:40_

---

_Closed by @charliermarsh on 2025-01-02 14:40_

---

_Branch deleted on 2025-01-02 14:40_

---
