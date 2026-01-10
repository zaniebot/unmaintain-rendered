```yaml
number: 3993
title: "Remove need to return Python version in `get_dependencies`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/req-p-2
created_at: 2024-06-03T18:33:18Z
updated_at: 2024-06-03T18:42:39Z
url: https://github.com/astral-sh/uv/pull/3993
synced_at: 2026-01-10T13:54:02Z
```

# Remove need to return Python version in `get_dependencies`

---

_Pull request opened by @charliermarsh on 2024-06-03 18:33_

## Summary

Once we use a _range_ rather than a precise version, it won't actually make sense to return a version here. It's no longer required, so I'm removing it.

---

_Marked ready for review by @charliermarsh on 2024-06-03 18:33_

---

_Label `internal` added by @charliermarsh on 2024-06-03 18:33_

---

_Merged by @charliermarsh on 2024-06-03 18:42_

---

_Closed by @charliermarsh on 2024-06-03 18:42_

---

_Branch deleted on 2024-06-03 18:42_

---
