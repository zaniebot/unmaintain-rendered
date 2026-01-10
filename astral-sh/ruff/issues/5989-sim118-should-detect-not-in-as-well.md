```yaml
number: 5989
title: "`SIM118` should detect `not in` as well"
type: issue
state: closed
author: dosisod
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-07-22T21:37:25Z
updated_at: 2023-07-23T01:46:22Z
url: https://github.com/astral-sh/ruff/issues/5989
synced_at: 2026-01-10T11:09:48Z
```

# `SIM118` should detect `not in` as well

---

_Issue opened by @dosisod on 2023-07-22 21:37_

Last issue for now!

Ruff only detects `in`, but it should detect `not in` as well:

```python
d = {}

if "key" in d.keys():
    pass

if "key" not in d.keys():
    pass
```

Ruff output:

```
$ ruff x.py
x.py:3:4: SIM118 [*] Use `"key" in d` instead of `"key" in d.keys()`
```

Expected:

```
x.py:3:4: SIM118 [*] Use `"key" in d` instead of `"key" in d.keys()`
x.py:6:4: SIM118 [*] Use `"key" not in d` instead of `"key" not in d.keys()`
```

---

_Label `bug` added by @charliermarsh on 2023-07-23 00:15_

---

_Label `accepted` added by @charliermarsh on 2023-07-23 00:15_

---

_Closed by @charliermarsh on 2023-07-23 01:46_

---
