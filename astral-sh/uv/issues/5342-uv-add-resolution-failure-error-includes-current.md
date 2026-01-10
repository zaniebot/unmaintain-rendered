---
number: 5342
title: "`uv add` resolution failure error includes current project version"
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-07-23T15:58:38Z
updated_at: 2024-08-16T13:23:06Z
url: https://github.com/astral-sh/uv/issues/5342
synced_at: 2026-01-10T01:23:48Z
---

# `uv add` resolution failure error includes current project version

---

_Issue opened by @zanieb on 2024-07-23 15:58_

```
$ uv add 'httpx>9999'
error: Because only httpx<=9999 is available and example==0.1.0 depends on httpx>9999, we can conclude that example==0.1.0 cannot be used.
And because only example==0.1.0 is available and you require example, we can conclude that the requirements are unsatisfiable.
```

It's really weird to include the following:

> only example==0.1.0 is available and you require example

---

_Label `error messages` added by @zanieb on 2024-07-23 15:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 19:51_

---

_Comment by @charliermarsh on 2024-08-01 20:31_

Should it even say "you require example"?

---

_Comment by @zanieb on 2024-08-01 20:37_

Definitely not, that's part of the problem.

---

_Comment by @charliermarsh on 2024-08-01 20:40_

Even if `example` is a workspace member, but not the root...?

---

_Comment by @zanieb on 2024-08-01 20:43_

I'm not sure. We should probably have a special-case for workspace members though 😭 

---

_Assigned to @zanieb by @charliermarsh on 2024-08-09 14:06_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-09 14:06_

---

_Comment by @zanieb on 2024-08-16 13:23_

Solved in #6092 

---

_Closed by @zanieb on 2024-08-16 13:23_

---
