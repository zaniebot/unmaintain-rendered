```yaml
number: 12879
title: "Applying `UP015` to `aiofiles`"
type: issue
state: closed
author: jamesbraza
labels:
  - rule
assignees: []
created_at: 2024-08-14T06:18:04Z
updated_at: 2024-08-30T23:39:01Z
url: https://github.com/astral-sh/ruff/issues/12879
synced_at: 2026-01-12T15:54:52Z
```

# Applying `UP015` to `aiofiles`

---

_@jamesbraza_

With `ruff==0.5.7`, [UP015](https://docs.astral.sh/ruff/rules/redundant-open-modes/) applies to the built-in `open` function:

```python
import aiofiles

with open("stub.txt", mode="r") as f:  # Ruff UP015 removes this mode="r"
    pass

async with aiofiles.open("stub.txt", mode="r") as f:  # Ruff doesn't yet remove this mode="r"
    pass
```

It would be nice to apply this same logic to `aiofiles.open`'s `mode` argument

---

_Label `rule` added by @MichaReiser on 2024-08-14 08:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 23:05_

---

_Closed by @charliermarsh on 2024-08-30 23:39_

---
