```yaml
number: 14116
title: Fix PyPI publish test script
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-pypi-publish-test
created_at: 2025-06-17T18:46:52Z
updated_at: 2025-06-18T07:51:54Z
url: https://github.com/astral-sh/uv/pull/14116
synced_at: 2026-01-12T16:11:02Z
```

# Fix PyPI publish test script

---

_@konstin_

The script stumbled over a newline introduced in https://github.com/pypi/warehouse/pull/18266 (which is valid).

Also fixed: Don't read versions for the same package from other indexes. We were using `project_name` here instead of `target`, while using the latter and only reading from a single index simplifies the code too.

---

_Label `internal` added by @konstin on 2025-06-17 18:46_

---

_@konstin reviewed on 2025-06-17 18:47_

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:224 on 2025-06-17 18:47_

The star also handles https://github.com/astral-sh/uv/pull/14116

---

_@zanieb approved on 2025-06-17 19:58_

---

_Merged by @konstin on 2025-06-18 07:51_

---

_Closed by @konstin on 2025-06-18 07:51_

---

_Branch deleted on 2025-06-18 07:51_

---
