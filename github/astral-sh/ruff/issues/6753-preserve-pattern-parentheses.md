---
number: 6753
title: Preserve pattern parentheses
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-22T06:45:42Z
updated_at: 2023-08-25T03:45:51Z
url: https://github.com/astral-sh/ruff/issues/6753
synced_at: 2026-01-07T13:12:15-06:00
---

# Preserve pattern parentheses

---

_Issue opened by @MichaReiser on 2023-08-22 06:45_

Black seems to preserve parentheses around nested patterns. 

```python
match a:
    case [(*rest), (a as b)]:
        print("test")
    
    case (a): pass
```

This is in line with Black preserving parentheses around nested expressions. 

Surprisingly, Black also seems to preserve parentheses around top-level patterns. I would be okay diverging from Black to align with the Expression formatting.

---

_Referenced in [astral-sh/ruff#5834](../../astral-sh/ruff/issues/5834.md) on 2023-08-22 06:45_

---

_Label `formatter` added by @MichaReiser on 2023-08-22 06:47_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:15_

---

_Referenced in [astral-sh/ruff#6800](../../astral-sh/ruff/pulls/6800.md) on 2023-08-23 06:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-23 16:12_

---

_Closed by @charliermarsh on 2023-08-25 03:45_

---
