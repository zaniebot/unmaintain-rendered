```yaml
number: 10338
title: "Accept requirements in `uv remove`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/rem
created_at: 2025-01-06T23:33:03Z
updated_at: 2025-01-07T13:47:07Z
url: https://github.com/astral-sh/uv/pull/10338
synced_at: 2026-01-10T11:44:43Z
```

# Accept requirements in `uv remove`

---

_Pull request opened by @charliermarsh on 2025-01-06 23:33_

## Summary

This allows, e.g., `uv remove flask[dotenv]` to remove `flask`. Like `pip install` and `uv pip install`, the content after the package name has no effect.

Closes https://github.com/astral-sh/uv/issues/9764.


---

_Label `compatibility` added by @charliermarsh on 2025-01-07 00:01_

---

_Merged by @charliermarsh on 2025-01-07 13:47_

---

_Closed by @charliermarsh on 2025-01-07 13:47_

---

_Branch deleted on 2025-01-07 13:47_

---
