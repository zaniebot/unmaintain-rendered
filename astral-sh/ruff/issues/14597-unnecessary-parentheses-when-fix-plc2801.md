```yaml
number: 14597
title: Unnecessary parentheses when fix PLC2801
type: issue
state: closed
author: bebound
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-11-26T03:49:18Z
updated_at: 2024-11-26T12:47:04Z
url: https://github.com/astral-sh/ruff/issues/14597
synced_at: 2026-01-12T15:54:54Z
```

# Unnecessary parentheses when fix PLC2801

---

_@bebound_

`ruff check a.py --select PLC2801 --fix --preview --unsafe-fixes` changes the code from
```python
assert 'kk'.__str__() == 'kk'
```
to
```python
assert (str('kk')) == 'kk'
```

expected behavior: `assert str('kk') == 'kk'`

My ruff version is 0.8.0

---

_Label `bug` added by @charliermarsh on 2024-11-26 03:52_

---

_Label `fixes` added by @charliermarsh on 2024-11-26 03:52_

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-26 04:40_

---

_Closed by @dylwil3 on 2024-11-26 12:47_

---
