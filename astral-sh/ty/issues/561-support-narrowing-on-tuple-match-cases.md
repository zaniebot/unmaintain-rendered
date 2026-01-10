---
number: 561
title: Support narrowing on tuple match cases
type: issue
state: open
author: MatthewMckee4
labels:
  - narrowing
assignees: []
created_at: 2025-06-01T12:24:08Z
updated_at: 2025-12-22T02:22:23Z
url: https://github.com/astral-sh/ty/issues/561
synced_at: 2026-01-10T01:52:52Z
---

# Support narrowing on tuple match cases

---

_Issue opened by @MatthewMckee4 on 2025-06-01 12:24_

From the typing spec: https://typing.python.org/en/latest/spec/tuples.html#type-compatibility-rules, we currently do not support this.
```py
def func(subj: tuple[int | str, int | str]):
    match subj:
        case x, str():
            reveal_type(subj)  # tuple[int | str, str]
        case y:
            reveal_type(subj)  # tuple[int | str, int]
```

---

_Label `narrowing` added by @AlexWaygood on 2025-06-01 15:40_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-22 02:22_

---
