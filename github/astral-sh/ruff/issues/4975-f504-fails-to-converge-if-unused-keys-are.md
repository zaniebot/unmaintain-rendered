---
number: 4975
title: "F504: Fails to converge if unused keys are concatenated string literals"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-09T04:02:22Z
updated_at: 2023-06-09T04:56:37Z
url: https://github.com/astral-sh/ruff/issues/4975
synced_at: 2026-01-07T13:12:15-06:00
---

# F504: Fails to converge if unused keys are concatenated string literals

---

_Issue opened by @addisoncrump on 2023-06-09 04:02_

You can concatenate string literals like so: `'a' 'b'` => `'ab'`.

When F504 identifies an unused key which is defined with concatenated string literals, it fails to converge: 

```py
x = '' % {'a''b' : ''}
```

```
$ ruff concat-f504.py --no-cache --fix
debug error: Failed to converge after 100 iterations in `concat-f504.py` with rule codes F504:---
x = '' % {'a''b' : ''}

---
concat-f504.py:1:5: F504 `%`-format string has unused named argument(s): ab
Found 101 errors (100 fixed, 1 remaining).
```

---

_Referenced in [astral-sh/ruff#4972](../../astral-sh/ruff/issues/4972.md) on 2023-06-09 04:04_

---

_Label `bug` added by @charliermarsh on 2023-06-09 04:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-09 04:09_

---

_Referenced in [astral-sh/ruff#4976](../../astral-sh/ruff/pulls/4976.md) on 2023-06-09 04:16_

---

_Closed by @charliermarsh on 2023-06-09 04:56_

---
