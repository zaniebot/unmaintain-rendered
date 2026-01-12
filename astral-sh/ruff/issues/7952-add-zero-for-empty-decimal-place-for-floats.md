```yaml
number: 7952
title: Add zero for empty decimal place for floats
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
  - formatter
assignees: []
created_at: 2023-10-13T16:43:46Z
updated_at: 2023-10-16T07:39:30Z
url: https://github.com/astral-sh/ruff/issues/7952
synced_at: 2026-01-12T15:54:47Z
```

# Add zero for empty decimal place for floats

---

_@konstin_

Ours:
```python
testcommon("%#.*g", (109, -1.e49 / 3.0))
```
Black:
```python
testcommon("%#.*g", (109, -1.0e49 / 3.0))
```

We should add the zero for empty decimal place

---

_Label `bug` added by @konstin on 2023-10-13 16:43_

---

_Label `good first issue` added by @konstin on 2023-10-13 16:43_

---

_Label `formatter` added by @konstin on 2023-10-13 16:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-13 21:14_

---

_Closed by @charliermarsh on 2023-10-16 01:42_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-10-16 07:39_

---
