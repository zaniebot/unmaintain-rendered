---
number: 6364
title: "Multiple calls to `uv add --script` shifts the inline metadata comment"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2024-08-21T18:13:31Z
updated_at: 2024-08-21T20:57:10Z
url: https://github.com/astral-sh/uv/issues/6364
synced_at: 2026-01-07T13:12:17-06:00
---

# Multiple calls to `uv add --script` shifts the inline metadata comment

---

_Issue opened by @dhruvmanila on 2024-08-21 18:13_

Adding multiple dependencies using `uv add --script test.py "..."` would _shift_ the inline metadata comment by one line. I guess there must be some extra newline which is being added before the comment.

It's best described by this short video:

https://github.com/user-attachments/assets/9c6fb067-0084-4c6b-a6cf-9ebfbcfe92e9



---

_Label `bug` added by @zanieb on 2024-08-21 18:21_

---

_Referenced in [astral-sh/uv#6366](../../astral-sh/uv/pulls/6366.md) on 2024-08-21 18:51_

---

_Closed by @charliermarsh on 2024-08-21 20:57_

---

_Closed by @charliermarsh on 2024-08-21 20:57_

---
