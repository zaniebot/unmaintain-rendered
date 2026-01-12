```yaml
number: 6735
title: Avoid including non-excluded members in parent workspaces
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/roots
created_at: 2024-08-28T01:30:28Z
updated_at: 2024-08-28T01:39:47Z
url: https://github.com/astral-sh/uv/pull/6735
synced_at: 2026-01-12T16:07:30Z
```

# Avoid including non-excluded members in parent workspaces

---

_@charliermarsh_

## Summary

If you're in a directory, and there's workspace above it, we check if the directory is excluded from the workspace members... But not if it's _included_ in the first place.

Closes https://github.com/astral-sh/uv/issues/6732.


---

_Label `bug` added by @charliermarsh on 2024-08-28 01:30_

---

_Merged by @charliermarsh on 2024-08-28 01:39_

---

_Closed by @charliermarsh on 2024-08-28 01:39_

---

_Branch deleted on 2024-08-28 01:39_

---
