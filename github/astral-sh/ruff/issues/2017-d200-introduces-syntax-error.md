---
number: 2017
title: D200 introduces syntax error
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-20T09:18:52Z
updated_at: 2023-01-20T12:41:59Z
url: https://github.com/astral-sh/ruff/issues/2017
synced_at: 2026-01-07T13:12:14-06:00
---

# D200 introduces syntax error

---

_Issue opened by @spaceone on 2023-01-20 09:18_

D200 causes a syntax error if the docstring endswith the quote: 
```
-"""
-python3 -m foo "$(date --utc '+%Y%m%d%H%M%SZ')"
-"""
+"""python3 -m foo "$(date --utc '+%Y%m%d%H%M%SZ')""""
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-20 12:20_

---

_Referenced in [astral-sh/ruff#2025](../../astral-sh/ruff/pulls/2025.md) on 2023-01-20 12:40_

---

_Closed by @charliermarsh on 2023-01-20 12:42_

---

_Referenced in [astral-sh/ruff#2027](../../astral-sh/ruff/pulls/2027.md) on 2023-01-20 13:00_

---

_Referenced in [astral-sh/ruff#2397](../../astral-sh/ruff/issues/2397.md) on 2023-01-31 14:22_

---
