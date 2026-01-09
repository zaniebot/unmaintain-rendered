---
number: 1939
title: flake8-commas autofixes even when rule is not included in list of fixables
type: issue
state: closed
author: rhkleijn
labels: []
assignees: []
created_at: 2023-01-18T00:37:59Z
updated_at: 2023-01-18T01:09:03Z
url: https://github.com/astral-sh/ruff/issues/1939
synced_at: 2026-01-07T13:12:14-06:00
---

# flake8-commas autofixes even when rule is not included in list of fixables

---

_Issue opened by @rhkleijn on 2023-01-18 00:37_

Using version 0.0.224 of ruff I ran into the following with the flake8-commas rules:

Consider this minimal example:
```python
# nocomma.py
print(
    1
)
```

When running `ruff nocomma.py --select COM --fix --fixable I001` the file gets changed even though the `COM` rules were not included in the list of fixables:

```python
# nocomma.py
print(
    1,
)
```
I would expect it to just report the violation and not alter any files.



---

_Comment by @charliermarsh on 2023-01-18 00:52_

Let me take a look!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-18 00:52_

---

_Comment by @charliermarsh on 2023-01-18 00:53_

Sorry, that's just an oversight. Will fix now.

---

_Referenced in [astral-sh/ruff#1940](../../astral-sh/ruff/pulls/1940.md) on 2023-01-18 01:08_

---

_Closed by @charliermarsh on 2023-01-18 01:09_

---
