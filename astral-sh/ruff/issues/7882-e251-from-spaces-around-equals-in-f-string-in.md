---
number: 7882
title: E251 from spaces around equals in f-string in function argument
type: issue
state: closed
author: mymoomin
labels:
  - bug
assignees: []
created_at: 2023-10-10T02:31:05Z
updated_at: 2023-10-12T10:15:01Z
url: https://github.com/astral-sh/ruff/issues/7882
synced_at: 2026-01-10T01:22:47Z
---

# E251 from spaces around equals in f-string in function argument

---

_Issue opened by @mymoomin on 2023-10-10 02:31_

On Ruff version 0.0.292, `ruff --preview --isolated` will misinterpret spaces around equals in f-strings that are function arguments as spaces around keyword arguments and raise an error.

```py
a = 1
b = f"{a = }"  # This is fine
print(a = 1)  # Expected E251 Unexpected spaces around keyword / parameter equals
print(f"{a = }")  # Unexpected E251
```



---

_Comment by @charliermarsh on 2023-10-10 02:32_

Thanks! \cc @dhruvmanila who fixed a few of these prior to release.

---

_Label `bug` added by @charliermarsh on 2023-10-10 02:32_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-10-10 13:18_

---

_Referenced in [astral-sh/ruff#7894](../../astral-sh/ruff/pulls/7894.md) on 2023-10-10 13:27_

---

_Closed by @dhruvmanila on 2023-10-12 05:26_

---

_Comment by @mymoomin on 2023-10-12 10:15_

Thank you for the incredibly fast fix!

---
