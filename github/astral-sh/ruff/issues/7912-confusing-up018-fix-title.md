---
number: 7912
title: Confusing UP018 fix title
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-10-11T07:58:52Z
updated_at: 2023-10-11T09:41:53Z
url: https://github.com/astral-sh/ruff/issues/7912
synced_at: 2026-01-07T13:12:15-06:00
---

# Confusing UP018 fix title

---

_Issue opened by @konstin on 2023-10-11 07:58_

```python
python_version = str("*")
```
suggests "Replace with empty string". The fix does the correct thing and removes the superfluous `str` call, but the message is wrong since this is not an empty string.

---

_Referenced in [astral-sh/ruff#7913](../../astral-sh/ruff/pulls/7913.md) on 2023-10-11 09:15_

---

_Closed by @konstin on 2023-10-11 09:41_

---
