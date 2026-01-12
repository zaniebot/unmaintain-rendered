```yaml
number: 3248
title: "FBT001: False-positive for boolean properties"
type: issue
state: closed
author: aberres
labels: []
assignees: []
created_at: 2023-02-27T09:14:57Z
updated_at: 2023-02-27T12:10:54Z
url: https://github.com/astral-sh/ruff/issues/3248
synced_at: 2026-01-12T15:54:43Z
```

# FBT001: False-positive for boolean properties

---

_@aberres_

In the case below, I think ruff should not error out with `FBT002`. A property setter must be a positional arg.

## Code

```python
class C:
    @property
    def deleted(self) -> bool:
       ...

    @deleted.setter
    def deleted(self, value: bool): # FBT001 Boolean positional arg in function definition 
       ...


c = C()

c.deleted = False
```

## Error

`fbt002.py:7:23: FBT001 Boolean positional arg in function definition`

---

_Renamed from "FBT002L " to "FBT002: False-positive for boolean properties" by @aberres on 2023-02-27 09:18_

---

_Comment by @aberres on 2023-02-27 12:09_

Uah, this project is to fast for me 🙈

I upgraded ruff in pre-commit but forgot to update it in my venv. It seems this one is already fixed.

---

_Renamed from "FBT002: False-positive for boolean properties" to "FBT001: False-positive for boolean properties" by @aberres on 2023-02-27 12:09_

---

_Closed by @aberres on 2023-02-27 12:10_

---
