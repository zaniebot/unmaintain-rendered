---
number: 2147
title: Support for installing remotely hosted dependency files (requirements.txt, lock files, constraints, overrides, etc)
type: issue
state: closed
author: ketozhang
labels:
  - duplicate
assignees: []
created_at: 2024-03-04T09:02:50Z
updated_at: 2024-03-04T14:36:18Z
url: https://github.com/astral-sh/uv/issues/2147
synced_at: 2026-01-07T13:12:17-06:00
---

# Support for installing remotely hosted dependency files (requirements.txt, lock files, constraints, overrides, etc)

---

_Issue opened by @ketozhang on 2024-03-04 09:02_

Missing feature from `uv pip` that is available in `pip` is that it cannot install a dependency file from a remote source.

## Example

Deploy an application released in GitHub releases using a lock file and a wheel file.
```console
$ export URL=https://github.com/myusername/myapp/releases/download/v1.2.3/
$ uv pip install -r $URL/requirements.txt $URL/myapp.whl
```

---

_Comment by @johanjk on 2024-03-04 11:34_

Duplicate of https://github.com/astral-sh/uv/issues/1332. See https://github.com/astral-sh/uv/pull/2081 for ongoing discussion.

---

_Comment by @charliermarsh on 2024-03-04 14:31_

Closing into other issues.

---

_Closed by @charliermarsh on 2024-03-04 14:31_

---

_Label `duplicate` added by @zanieb on 2024-03-04 14:36_

---
