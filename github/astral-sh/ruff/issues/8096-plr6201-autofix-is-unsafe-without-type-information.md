---
number: 8096
title: PLR6201 autofix is unsafe without type information
type: issue
state: closed
author: Avasam
labels: []
assignees: []
created_at: 2023-10-20T16:13:05Z
updated_at: 2023-10-20T17:38:01Z
url: https://github.com/astral-sh/ruff/issues/8096
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR6201 autofix is unsafe without type information

---

_Issue opened by @Avasam on 2023-10-20 16:13_

Non-hashable entries should not be put in a set.
See the following minimal reproduction:
```
dict1 = {"foo": 1}
dict2 = {"foo": 2}
dicttest = {"foo": 3}
print(dicttest in (dict1, dict2))
```

It gets autofix to:
```
dict1 = {"foo": 1}
dict2 = {"foo": 2}
dicttest = {"foo": 3}
print(dicttest in {dict1, dict2})
```

which results in: 
```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'dict'
```
and pyright only reports as `error: Set entry must be hashable` since https://github.com/microsoft/pyright/releases/tag/1.1.323

---

_Referenced in [astral-sh/ruff#8097](../../astral-sh/ruff/pulls/8097.md) on 2023-10-20 16:41_

---

_Closed by @zanieb on 2023-10-20 17:38_

---
