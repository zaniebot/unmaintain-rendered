---
number: 14274
title: Incorrect fix suggested for FURB189
type: issue
state: closed
author: penguinolog
labels: []
assignees: []
created_at: 2024-11-11T12:37:41Z
updated_at: 2024-11-11T12:58:02Z
url: https://github.com/astral-sh/ruff/issues/14274
synced_at: 2026-01-07T13:12:16-06:00
---

# Incorrect fix suggested for FURB189

---

_Issue opened by @penguinolog on 2024-11-11 12:37_

Preview check FURB189 suggest using `collections.UserStr`, but correct base class if `collections.UserString` (https://docs.python.org/3/library/collections.html#userstring-objects)

Example code to trigger checker:
```python
class MyStr(str):
    __ slots__ = ()
```

Check command used: `ruff check --preview --select=FURB189`

Ruff version: 0.7.3

---

_Renamed from "Incorrect fix proposed for FURB189" to "Incorrect fix suggested for FURB189" by @penguinolog on 2024-11-11 12:38_

---

_Comment by @sbrugman on 2024-11-11 12:56_

Hi @penguinolog thanks for flagging this!

The issue has already been resolved in main and will be fixed in the upcoming release (https://github.com/astral-sh/ruff/pull/14209).

---

_Comment by @penguinolog on 2024-11-11 12:58_

Closing as duplicate for fixed #14209 

---

_Closed by @penguinolog on 2024-11-11 12:58_

---
