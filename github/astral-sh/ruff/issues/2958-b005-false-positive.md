---
number: 2958
title: B005 false positive
type: issue
state: closed
author: henryiii
labels:
  - bug
assignees: []
created_at: 2023-02-16T16:54:23Z
updated_at: 2023-02-17T00:05:32Z
url: https://github.com/astral-sh/ruff/issues/2958
synced_at: 2026-01-07T13:12:14-06:00
---

# B005 false positive

---

_Issue opened by @henryiii on 2023-02-16 16:54_

The following code works in flake8-bugbear but triggers a false positive in Ruff:

```python
line = ""
line.lstrip("\ufeff")
```

```
B005 Using `.strip()` with multi-character strings is misleading the reader
```

It's incorrectly thinking that "\ufeff" is more than one character.

The B005 flag is active, and tested on 0.0.240 & 0.0.246.


---

_Label `bug` added by @charliermarsh on 2023-02-16 16:55_

---

_Comment by @artemmukhin on 2023-02-17 00:01_

This looks like a duplicate of already fixed #2851

---

_Comment by @charliermarsh on 2023-02-17 00:05_

Yeah I just tested and it _seems_ like this is fixed on main.

---

_Closed by @charliermarsh on 2023-02-17 00:05_

---

_Referenced in [astral-sh/ruff#2976](../../astral-sh/ruff/pulls/2976.md) on 2023-02-17 00:39_

---
