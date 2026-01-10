```yaml
number: 514
title: Show error paths in install-wheel-rs
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: install-wheel-rs-error-paths
created_at: 2023-11-29T17:49:12Z
updated_at: 2023-11-29T19:14:36Z
url: https://github.com/astral-sh/uv/pull/514
synced_at: 2026-01-10T15:44:44Z
```

# Show error paths in install-wheel-rs

---

_Pull request opened by @konstin on 2023-11-29 17:49_

Ensure that we consistently show a path for all io errors in install-wheel-rs either (preferred) through `fs_err`, or as fallback by a custom error type. For zip reading errors, we rely on the caller to add the name and/or location of the wheel.

---

_Review requested from @zanieb by @konstin on 2023-11-29 17:49_

---

_@zanieb approved on 2023-11-29 17:59_

Thanks!

---

_Merged by @konstin on 2023-11-29 19:14_

---

_Closed by @konstin on 2023-11-29 19:14_

---

_Branch deleted on 2023-11-29 19:14_

---
