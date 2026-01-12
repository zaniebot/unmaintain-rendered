```yaml
number: 1612
title: "Fix leftover whitespace when removing `pass` for `PIE790`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: 1610-fix-whitespace-removing-pass
created_at: 2023-01-04T01:41:06Z
updated_at: 2023-01-04T01:45:28Z
url: https://github.com/astral-sh/ruff/pull/1612
synced_at: 2026-01-12T15:55:06Z
```

# Fix leftover whitespace when removing `pass` for `PIE790`

---

_@edgarrmondragon_

Closes https://github.com/charliermarsh/ruff/issues/1610


---

_@edgarrmondragon reviewed on 2023-01-04 01:43_

---

_Review comment by @edgarrmondragon on `src/flake8_pie/snapshots/ruff__flake8_pie__tests__PIE790_PIE790.py.snap`:51 on 2023-01-04 01:43_

This amends

```python
def multi_statement() -> None:
    """This is a function."""
    pass; print("hello")
```

into

```python
def multi_statement() -> None:
    """This is a function."""
    print("hello")
```

---

_Comment by @charliermarsh on 2023-01-04 01:44_

Thanks!

---

_Merged by @charliermarsh on 2023-01-04 01:45_

---

_Closed by @charliermarsh on 2023-01-04 01:45_

---

_Branch deleted on 2023-01-04 01:45_

---
