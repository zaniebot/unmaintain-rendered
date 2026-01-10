```yaml
number: 2279
title: Fix Conda Python detection on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/conda
created_at: 2024-03-07T14:40:24Z
updated_at: 2024-03-07T14:48:53Z
url: https://github.com/astral-sh/uv/pull/2279
synced_at: 2026-01-10T14:54:43Z
```

# Fix Conda Python detection on Windows

---

_Pull request opened by @charliermarsh on 2024-03-07 14:40_

## Summary

In #2102, I did some refactor, and changed a method to return the Python executable path rather than the parent directory path. But I missed this one codepath for Conda on Windows.

Closes https://github.com/astral-sh/uv/issues/2269.

## Test Plan

- Installed micromamba on my Windows machine.
- Reproduced the failure in the linked issue.
- Verified that `python.exe` exists at `${CONDA_PREFIX}\python.exe`.
- Ran with this change; installed successfully.


---

_Label `bug` added by @charliermarsh on 2024-03-07 14:40_

---

_Label `windows` added by @charliermarsh on 2024-03-07 14:40_

---

_Comment by @charliermarsh on 2024-03-07 14:43_

Gonna add some Conda testing on CI in a subsequent PR.

---

_Merged by @charliermarsh on 2024-03-07 14:45_

---

_Closed by @charliermarsh on 2024-03-07 14:45_

---

_Branch deleted on 2024-03-07 14:45_

---

_Comment by @MatthieuDartiailh on 2024-03-07 14:48_

Amazing ! 

---
