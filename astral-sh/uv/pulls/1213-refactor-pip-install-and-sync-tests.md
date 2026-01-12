```yaml
number: 1213
title: Refactor pip install and sync tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/refactor-pip-install-and-sync-tests
created_at: 2024-01-31T20:18:11Z
updated_at: 2024-02-02T17:01:39Z
url: https://github.com/astral-sh/uv/pull/1213
synced_at: 2026-01-12T16:04:30Z
```

# Refactor pip install and sync tests

---

_@konstin_

Mostly a mechanical refactor to use the `puffin_snapshot!` and `TestContext` infrastructure in the pip install and pip sync tests, in preparation for adding programmatic windows testing filters.

---

_Review requested from @charliermarsh by @konstin on 2024-01-31 20:18_

---

_@zanieb approved on 2024-02-02 02:10_

Conceptually yes. I did not read through carefully.

---

_@zanieb reviewed on 2024-02-02 02:10_

---

_Review comment by @zanieb on `crates/puffin/tests/common/mod.rs`:77 on 2024-02-02 02:10_

A little more context here would be nice so people don't have to go read the issue to know what this refers to

---

_Merged by @konstin on 2024-02-02 09:26_

---

_Closed by @konstin on 2024-02-02 09:26_

---

_Branch deleted on 2024-02-02 09:26_

---

_Label `internal` added by @zanieb on 2024-02-02 17:01_

---
