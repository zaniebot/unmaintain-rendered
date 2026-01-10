```yaml
number: 10860
title: Treat version mismatch errors as non-fatal in fast paths
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/igno
created_at: 2025-01-22T14:52:04Z
updated_at: 2025-01-22T15:13:27Z
url: https://github.com/astral-sh/uv/pull/10860
synced_at: 2026-01-10T11:45:14Z
```

# Treat version mismatch errors as non-fatal in fast paths

---

_Pull request opened by @charliermarsh on 2025-01-22 14:52_

## Summary

I noticed that we're only handling `Error::WheelMetadataNameMismatch` here; but `Error::WheelMetadataVersionMismatch` should also be treated as non-fatal.


---

_Label `bug` added by @charliermarsh on 2025-01-22 14:52_

---

_Merged by @charliermarsh on 2025-01-22 15:13_

---

_Closed by @charliermarsh on 2025-01-22 15:13_

---

_Branch deleted on 2025-01-22 15:13_

---
