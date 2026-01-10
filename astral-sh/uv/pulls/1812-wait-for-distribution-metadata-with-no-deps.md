```yaml
number: 1812
title: "Wait for distribution metadata with `--no-deps`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-02-21T14:37:42Z
updated_at: 2024-02-21T14:45:10Z
url: https://github.com/astral-sh/uv/pull/1812
synced_at: 2026-01-10T15:33:24Z
```

# Wait for distribution metadata with `--no-deps`

---

_Pull request opened by @charliermarsh on 2024-02-21 14:37_

## Summary

We still need to wait for the distribution metadata (for direct dependencies), even when resolving with `--no-deps`, since we rely on it to report diagnostics to the user.

Closes https://github.com/astral-sh/uv/issues/1801.


---

_Label `bug` added by @charliermarsh on 2024-02-21 14:37_

---

_Merged by @charliermarsh on 2024-02-21 14:45_

---

_Closed by @charliermarsh on 2024-02-21 14:45_

---

_Branch deleted on 2024-02-21 14:45_

---
