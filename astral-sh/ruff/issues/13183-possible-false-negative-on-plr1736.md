```yaml
number: 13183
title: Possible false negative on PLR1736
type: issue
state: closed
author: sbrugman
labels:
  - bug
  - rule
assignees: []
created_at: 2024-08-31T13:30:10Z
updated_at: 2024-09-01T16:22:46Z
url: https://github.com/astral-sh/ruff/issues/13183
synced_at: 2026-01-12T15:54:52Z
```

# Possible false negative on PLR1736

---

_@sbrugman_

The following example I would expect to be recognised by [PLR1736](https://docs.astral.sh/ruff/rules/unnecessary-list-index-lookup/) 

Minimal reproducible example:

```python
data = {"a": 1, "b": 2}
column_names = ["a", "b"]
for index, column_name in enumerate(column_names):
    _ = data[column_names[index]]
```

Command:
```
ruff check --isolated --select PLR1736 examples/plr.py
```

Output:

```text
All checks passed!
```

Expected output:
```text
examples/plr.py:8:8: PLR1736 [*] List index lookup in `enumerate()` loop
  |
7 | for index, column_name in enumerate(column_names):
8 |     _ = data[column_names[index]]
  |             ^^^^^^^^^^^^^^^^^^^^ PLR1736
  |
  = help: Use the loop variable directly
```

Version: ruff 0.6.3

---

_Label `bug` added by @AlexWaygood on 2024-08-31 21:08_

---

_Label `rule` added by @AlexWaygood on 2024-08-31 21:08_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-09-01 10:13_

---

_Closed by @AlexWaygood on 2024-09-01 16:22_

---

_Closed by @AlexWaygood on 2024-09-01 16:22_

---
