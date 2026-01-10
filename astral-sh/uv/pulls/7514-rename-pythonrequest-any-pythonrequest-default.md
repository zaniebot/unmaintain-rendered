```yaml
number: 7514
title: "Rename `PythonRequest::Any` -> `PythonRequest::Default`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/python-request-all
created_at: 2024-09-18T18:52:40Z
updated_at: 2024-09-19T10:56:07Z
url: https://github.com/astral-sh/uv/pull/7514
synced_at: 2026-01-10T12:53:49Z
```

# Rename `PythonRequest::Any` -> `PythonRequest::Default`

---

_Pull request opened by @zanieb on 2024-09-18 18:52_

As we support more complex Python discovery behaviors such as:

- #7431 
- #7335 
- #7300 

`Any` is no longer accurate, we actually are looking for a reasonable default Python version to use which may exclude the first one we find. Separately, we need the idea of `Any` to improve behavior when listing versions (e.g., #7286) where we do actually want to match _any_ Python version. As a first step, we'll rename `Any` to `Default`. Then, we'll introduce a new `Any` that actually behaves as we'd expect.


---

_Label `internal` added by @zanieb on 2024-09-18 18:52_

---

_Marked ready for review by @zanieb on 2024-09-18 19:47_

---

_@charliermarsh approved on 2024-09-19 02:25_

---

_Merged by @zanieb on 2024-09-19 10:56_

---

_Closed by @zanieb on 2024-09-19 10:56_

---

_Branch deleted on 2024-09-19 10:56_

---
