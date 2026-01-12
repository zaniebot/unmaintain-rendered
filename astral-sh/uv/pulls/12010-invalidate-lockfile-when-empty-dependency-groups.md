```yaml
number: 12010
title: Invalidate lockfile when empty dependency groups are added or removed
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2025-03-06T17:33:50Z
updated_at: 2025-03-06T17:44:22Z
url: https://github.com/astral-sh/uv/pull/12010
synced_at: 2026-01-12T16:10:05Z
```

# Invalidate lockfile when empty dependency groups are added or removed

---

_@charliermarsh_

## Summary

Since https://github.com/astral-sh/uv/pull/8598, we (correctly) include empty groups in the lockfile, so we can validate them properly in the satisfaction check.

Closes https://github.com/astral-sh/uv/issues/12007.


---

_Label `bug` added by @charliermarsh on 2025-03-06 17:33_

---

_Merged by @charliermarsh on 2025-03-06 17:44_

---

_Closed by @charliermarsh on 2025-03-06 17:44_

---

_Branch deleted on 2025-03-06 17:44_

---
