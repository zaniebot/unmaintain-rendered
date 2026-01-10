---
number: 6809
title: "Failed to create fix for UnnecessaryCollectionCall: Failed to extract expression from source"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - needs-decision
  - fuzzer
assignees: []
created_at: 2023-08-23T10:16:35Z
updated_at: 2023-10-01T08:24:46Z
url: https://github.com/astral-sh/ruff/issues/6809
synced_at: 2026-01-10T01:22:46Z
---

# Failed to create fix for UnnecessaryCollectionCall: Failed to extract expression from source

---

_Issue opened by @qarmin on 2023-08-23 10:16_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
DOCUMENTATION = """
"""
def main():
    dict(
        job_slice_count=
ict(type="int"),
    )
```

error:
```
error: Failed to create fix for UnnecessaryCollectionCall: Failed to extract expression from source
```

[PY_FILE_TEST_13739308565.py.zip](https://github.com/astral-sh/ruff/files/12418025/PY_FILE_TEST_13739308565.py.zip)


---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-23 13:28_

---

_Label `bug` added by @charliermarsh on 2023-08-23 13:28_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-23 13:28_

---

_Comment by @charliermarsh on 2023-08-23 13:43_

I think this file contains a bidirectional character or something similar.

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-23 13:43_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-23 13:58_

---

_Comment by @qarmin on 2023-10-01 08:24_

No longer broken

---

_Closed by @qarmin on 2023-10-01 08:24_

---
