```yaml
number: 4488
title: Read content length from response rather than request
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-06-24T19:46:21Z
updated_at: 2024-06-24T19:58:22Z
url: https://github.com/astral-sh/uv/pull/4488
synced_at: 2026-01-10T13:48:28Z
```

# Read content length from response rather than request

---

_Pull request opened by @charliermarsh on 2024-06-24 19:46_

## Summary

I might be mistaken, but I think we need to read the header from the response, not the request. The request would only contain headers that we set.

I verified (with extra logging) that the request header is `None` while PyPI returns a valid length in the response header.


---

_Label `bug` added by @charliermarsh on 2024-06-24 19:46_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-24 19:47_

---

_@ibraheemdev approved on 2024-06-24 19:48_

---

_Merged by @charliermarsh on 2024-06-24 19:58_

---

_Closed by @charliermarsh on 2024-06-24 19:58_

---

_Branch deleted on 2024-06-24 19:58_

---
