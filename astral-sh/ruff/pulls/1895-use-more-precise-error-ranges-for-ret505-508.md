```yaml
number: 1895
title: Use more precise error ranges for RET505~508
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-RET-range
created_at: 2023-01-15T16:30:35Z
updated_at: 2023-01-15T18:54:24Z
url: https://github.com/astral-sh/ruff/pull/1895
synced_at: 2026-01-12T15:55:07Z
```

# Use more precise error ranges for RET505~508

---

_@harupy_

Fix:

```
resources/test/fixtures/flake8_return/RET505.py:5:5: RET505 Unnecessary `elif` after `return` statement
   |
 5 |       if x:  # [no-else-return]
   |  _____^
 6 | |         a = 1
 7 | |         return y
 8 | |     elif z:
 9 | |         b = 2
10 | |         return w
11 | |     else:
12 | |         c = 3
13 | |         return z
   | |________________^ RET505
```

to:

```
resources/test/fixtures/flake8_return/RET505.py:8:5: RET505 Unnecessary `elif` after `return` statement
  |
8 |     elif z:
  |     ^^^^ RET505
  |
```

so we can directly jump to `elif` or `else` that violates `RET50x` from the error message.

---

_Renamed from "Fix error range of RET505~508" to "Use more precise error ranges for RET505~508" by @harupy on 2023-01-15 16:35_

---

_Merged by @charliermarsh on 2023-01-15 18:54_

---

_Closed by @charliermarsh on 2023-01-15 18:54_

---
