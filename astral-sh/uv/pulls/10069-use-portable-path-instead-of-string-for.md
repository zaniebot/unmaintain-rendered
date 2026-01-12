```yaml
number: 10069
title: Use portable path instead of string for subdirectory
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/port
created_at: 2024-12-21T00:15:19Z
updated_at: 2024-12-21T00:28:38Z
url: https://github.com/astral-sh/uv/pull/10069
synced_at: 2026-01-12T16:09:06Z
```

# Use portable path instead of string for subdirectory

---

_@charliermarsh_

## Summary

A few places where there are extra conversions to and from string that seem unnecessary; a few places where we're using `PathBuf` instead of `PortablePathBuf`.

---

_Label `internal` added by @charliermarsh on 2024-12-21 00:15_

---

_Marked ready for review by @charliermarsh on 2024-12-21 00:17_

---

_Merged by @charliermarsh on 2024-12-21 00:28_

---

_Closed by @charliermarsh on 2024-12-21 00:28_

---

_Branch deleted on 2024-12-21 00:28_

---
