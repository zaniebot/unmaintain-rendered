```yaml
number: 8900
title: Fails to parse type aliases in same line body
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - parser
assignees: []
created_at: 2023-11-29T05:46:50Z
updated_at: 2023-11-30T19:15:20Z
url: https://github.com/astral-sh/ruff/issues/8900
synced_at: 2026-01-10T11:09:51Z
```

# Fails to parse type aliases in same line body

---

_Issue opened by @MichaReiser on 2023-11-29 05:46_

Ruff fails to parse type aliases in same line bodies

```python
class X: type InClass = int
```

This prevents us from importing Black's `type_aliases.py` test case

---

_Renamed from "Fails to parse type aliases in class inheritance clause" to "Fails to parse type aliases in same line body" by @MichaReiser on 2023-11-29 05:47_

---

_Label `bug` added by @MichaReiser on 2023-11-29 05:47_

---

_Label `parser` added by @MichaReiser on 2023-11-29 05:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-30 00:56_

---

_Closed by @charliermarsh on 2023-11-30 19:15_

---
