```yaml
number: 10859
title: Make GitHub fast path errors non-fatal
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/non
created_at: 2025-01-22T14:49:38Z
updated_at: 2025-01-22T16:54:05Z
url: https://github.com/astral-sh/uv/pull/10859
synced_at: 2026-01-10T11:45:14Z
```

# Make GitHub fast path errors non-fatal

---

_Pull request opened by @charliermarsh on 2025-01-22 14:49_

## Summary

For example: in the linked issue, the user has a symlink at `pyproject.toml`. The GitHub CDN doesn't give us any way to determine whether a file is a symlink, so we should just log the error and move on to the slow path.

Closes https://github.com/astral-sh/uv/issues/10857


---

_Label `bug` added by @charliermarsh on 2025-01-22 14:49_

---

_@zanieb approved on 2025-01-22 15:29_

---

_Merged by @charliermarsh on 2025-01-22 16:54_

---

_Closed by @charliermarsh on 2025-01-22 16:54_

---

_Branch deleted on 2025-01-22 16:54_

---
