---
number: 13062
title: "How to include 'dev' dependency-groups in uv pip compile?"
type: issue
state: closed
author: yx-altera
labels:
  - question
assignees: []
created_at: 2025-04-22T22:48:39Z
updated_at: 2025-04-23T11:18:19Z
url: https://github.com/astral-sh/uv/issues/13062
synced_at: 2026-01-10T01:25:28Z
---

# How to include 'dev' dependency-groups in uv pip compile?

---

_Issue opened by @yx-altera on 2025-04-22 22:48_

### Question

I have the following dev dependency-groups:
```
[dependency-groups]
dev = [
    "ipykernel>=6.29.5",
    "pytest>=8.3.5",
    "pytest-asyncio>=0.23.0",
    "pytest-xdist>=3.6.1",
]
```

How to include these in uv pip compile?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @yx-altera on 2025-04-22 22:48_

---

_Comment by @yx-altera on 2025-04-22 23:11_

--group dev seems do the trick, it includes dev group and the default one.

---

_Comment by @konstin on 2025-04-23 11:18_

Closing as solved

---

_Closed by @konstin on 2025-04-23 11:18_

---
