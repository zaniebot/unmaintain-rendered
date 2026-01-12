```yaml
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
synced_at: 2026-01-12T15:54:42Z
```

# D200 introduces syntax error

---

_@spaceone_

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

_Closed by @charliermarsh on 2023-01-20 12:42_

---
