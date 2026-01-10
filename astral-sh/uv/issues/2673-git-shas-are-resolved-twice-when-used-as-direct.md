---
number: 2673
title: Git SHAs are resolved twice when used as direct references
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - performance
assignees: []
created_at: 2024-03-26T19:41:08Z
updated_at: 2024-03-27T01:39:02Z
url: https://github.com/astral-sh/uv/issues/2673
synced_at: 2026-01-10T01:23:20Z
---

# Git SHAs are resolved twice when used as direct references

---

_Issue opened by @charliermarsh on 2024-03-26 19:41_

E.g., notice that the "Updated" portion appears twice here:

```
❯ uv pip install git+https://github.com/tiangolo/fastapi
 Updated https://github.com/tiangolo/fastapi (8bfed9a)
 Updated https://github.com/tiangolo/fastapi (8bfed9a)
Resolved 9 packages in 868ms
   Built fastapi @ git+https://github.com/tiangolo/fastapi@8bfed9aee2401a937bc38f336Downloaded 4 packages in 280ms
Installed 7 packages in 3ms
 + annotated-types==0.6.0
 + anyio==4.3.0
 + fastapi==0.110.0 (from git+https://github.com/tiangolo/fastapi@8bfed9aee2401a937bc38f336cddd903722c5286)
 + pydantic==2.6.4
 + pydantic-core==2.16.3
 + starlette==0.36.3
 + typing-extensions==4.10.0
```


---

_Label `bug` added by @charliermarsh on 2024-03-26 19:41_

---

_Comment by @charliermarsh on 2024-03-26 19:42_

The behavior is ultimately correct, we should just avoid the second lookup.

---

_Label `performance` added by @zanieb on 2024-03-26 20:40_

---

_Referenced in [astral-sh/uv#2682](../../astral-sh/uv/pulls/2682.md) on 2024-03-27 01:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-27 01:29_

---

_Closed by @charliermarsh on 2024-03-27 01:39_

---
