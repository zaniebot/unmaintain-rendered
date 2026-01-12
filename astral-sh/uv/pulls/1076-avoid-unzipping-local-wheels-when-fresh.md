```yaml
number: 1076
title: Avoid unzipping local wheels when fresh
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/path-cache
created_at: 2024-01-24T14:59:08Z
updated_at: 2024-01-24T15:01:18Z
url: https://github.com/astral-sh/uv/pull/1076
synced_at: 2026-01-12T16:04:24Z
```

# Avoid unzipping local wheels when fresh

---

_@charliermarsh_

Since the archive is a single file in this case, we can rely on the modification timestamp to check for freshness.

---

_Merged by @charliermarsh on 2024-01-24 15:01_

---

_Closed by @charliermarsh on 2024-01-24 15:01_

---

_Branch deleted on 2024-01-24 15:01_

---
