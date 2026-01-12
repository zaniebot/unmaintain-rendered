```yaml
number: 5672
title: "Formatter: Don't insert a newline between if and parentheses in list comprehensions"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-11T07:34:19Z
updated_at: 2023-07-11T14:51:26Z
url: https://github.com/astral-sh/ruff/issues/5672
synced_at: 2026-01-12T15:54:45Z
```

# Formatter: Don't insert a newline between if and parentheses in list comprehensions

---

_@konstin_

We format this black formatted code from django [django/apps/config.py](https://github.com/django/django/blob/e54f711d4287b3ea57026a02b48ab7e28ca6dcc1/django/apps/config.py#L128-L136)
```python
configs = [
    (name, candidate)
    for name, candidate in members
    if (
        issubclass(candidate, cls)
        and candidate is not cls
        and getattr(candidate, "default", True)
    )
]
```
as
```python
configs = [
    (name, candidate)
    for name, candidate in members
    if
    (
        issubclass(candidate, cls)
        and candidate is not cls
        and getattr(candidate, "default", True)
    )
]
```
We should change our formatting to also not emit the newline before the parentheses.

---

_Label `formatter` added by @konstin on 2023-07-11 07:34_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-07-11 10:12_

---

_Closed by @MichaReiser on 2023-07-11 14:51_

---
