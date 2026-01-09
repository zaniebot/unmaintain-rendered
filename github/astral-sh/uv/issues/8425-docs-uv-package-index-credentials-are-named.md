---
number: 8425
title: "Docs: UV package index credentials are named incorrectly"
type: issue
state: closed
author: pythonweb2
labels: []
assignees: []
created_at: 2024-10-21T19:57:27Z
updated_at: 2024-10-21T20:03:10Z
url: https://github.com/astral-sh/uv/issues/8425
synced_at: 2026-01-07T13:12:17-06:00
---

# Docs: UV package index credentials are named incorrectly

---

_Issue opened by @pythonweb2 on 2024-10-21 19:57_

Had to review the PR (#7741) to find this, it should be:

- UV_HTTP_BASIC_{name}_USERNAME
- UV_HTTP_BASIC_{name}_PASSWORD

Right now in the docs, the env variables are prefixed with `UV_INDEX_` instead.

Thank you!

![image](https://github.com/user-attachments/assets/5f90f4b0-3c21-4287-bfeb-17c322a55904)


---

_Comment by @pythonweb2 on 2024-10-21 20:03_

Oh, I was using an older version, and can see that this was updated in the code, rather than the docs. Nevermind then.

---

_Closed by @pythonweb2 on 2024-10-21 20:03_

---
