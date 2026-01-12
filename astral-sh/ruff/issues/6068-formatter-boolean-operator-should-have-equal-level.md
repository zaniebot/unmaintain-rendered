```yaml
number: 6068
title: "Formatter: boolean operator should have equal level"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-25T11:13:26Z
updated_at: 2023-08-07T17:22:35Z
url: https://github.com/astral-sh/ruff/issues/6068
synced_at: 2026-01-12T15:54:45Z
```

# Formatter: boolean operator should have equal level

---

_@konstin_

We format `and` in one line and break the `or`, even though like black we should break both

black:
```python
if not (
    isinstance(aaaaaaaaaaaaaaaaaaaaaaa, bbbbbbbbb)
    or numpy
    and isinstance(ccccccccccc, dddddd)
):
    pass

if not (
    isinstance(aaaaaaaaaaaaaaaaaaaaaaa, bbbbbbbbb)
    and numpy
    or isinstance(ccccccccccc, dddddd)
):
    pass
```
ours:
```python
if not (
    isinstance(aaaaaaaaaaaaaaaaaaaaaaa, bbbbbbbbb)
    or numpy and isinstance(ccccccccccc, dddddd)
):
    pass

if not (
    isinstance(aaaaaaaaaaaaaaaaaaaaaaa, bbbbbbbbb) and numpy
    or isinstance(ccccccccccc, dddddd)
):
    pass
```

---

_Label `formatter` added by @konstin on 2023-07-25 11:13_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 16:09_

---

_Assigned to @zanieb by @zanieb on 2023-08-02 20:52_

---

_Closed by @zanieb on 2023-08-07 17:22_

---
