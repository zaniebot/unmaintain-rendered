```yaml
number: 2357
title: "`requirements-txt` should respect our `Reqwest` middleware"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-03-11T14:18:41Z
updated_at: 2024-03-21T18:48:58Z
url: https://github.com/astral-sh/uv/issues/2357
synced_at: 2026-01-10T05:40:32Z
```

# `requirements-txt` should respect our `Reqwest` middleware

---

_Issue opened by @charliermarsh on 2024-03-11 14:18_

I changed this in https://github.com/astral-sh/uv/pull/2350 to make the client lazily initiated due to a significant performance regression for no-op-like invocations. However, if we add more middleware, we want to make sure it's respected. It needs to be lazy.

---

_Label `internal` added by @charliermarsh on 2024-03-11 14:18_

---

_Closed by @zanieb on 2024-03-21 18:48_

---
