```yaml
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
synced_at: 2026-01-10T11:09:47Z
```

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

_Label `bug` added by @charliermarsh on 2023-06-09 04:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-09 04:09_

---

_Closed by @charliermarsh on 2023-06-09 04:56_

---
