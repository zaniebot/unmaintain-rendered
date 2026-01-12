```yaml
number: 4769
title: "FLY002: incorrect behavior of automatic fixer on strings"
type: issue
state: closed
author: saippuakauppias
labels:
  - bug
assignees: []
created_at: 2023-05-31T22:09:23Z
updated_at: 2023-06-03T20:35:07Z
url: https://github.com/astral-sh/ruff/issues/4769
synced_at: 2026-01-12T15:54:44Z
```

# FLY002: incorrect behavior of automatic fixer on strings

---

_@saippuakauppias_

Example:
```python
print(','.join(['first', 'second']))
```

Run ruff:
```
$ ruff test-FLY002.py --select FLY --fix
Found 1 error (1 fixed, 0 remaining).
```

Fixed result:
```
print(f'first,second')
```

Expected: if the list contains only strings (no variables), you do not need to do f-string

---

_Label `bug` added by @charliermarsh on 2023-06-01 01:07_

---

_Comment by @charliermarsh on 2023-06-01 01:18_

Good call.

---

_Closed by @charliermarsh on 2023-06-03 20:35_

---
