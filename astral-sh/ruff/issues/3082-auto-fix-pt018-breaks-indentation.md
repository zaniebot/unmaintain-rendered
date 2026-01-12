```yaml
number: 3082
title: Auto fix PT018 breaks indentation
type: issue
state: closed
author: janosh
labels:
  - bug
assignees: []
created_at: 2023-02-21T03:32:15Z
updated_at: 2023-02-21T18:10:32Z
url: https://github.com/astral-sh/ruff/issues/3082
synced_at: 2026-01-12T15:54:43Z
```

# Auto fix PT018 breaks indentation

---

_@janosh_

Before:

```py
# t.py

def test_size(self):
    width = 121
    height = 121
    assert width == 121 and height == 121
```

After 

```sh
ruff check t.py --select PT --fix
```

with `ruff 0.0.249`

```py
def test_size(self):
    width = 121
    assert width == 121
assert height == 121
```

---

_Label `bug` added by @charliermarsh on 2023-02-21 03:32_

---

_Comment by @charliermarsh on 2023-02-21 03:33_

Thank you, definitely a bug.

---

_Closed by @charliermarsh on 2023-02-21 18:10_

---
