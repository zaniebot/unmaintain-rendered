```yaml
number: 15554
title: "`SIM108` false negative when variable has type annotation"
type: issue
state: open
author: nickdrozd
labels:
  - rule
assignees: []
created_at: 2025-01-17T16:09:16Z
updated_at: 2025-01-17T17:46:37Z
url: https://github.com/astral-sh/ruff/issues/15554
synced_at: 2026-01-10T11:09:57Z
```

# `SIM108` false negative when variable has type annotation

---

_Issue opened by @nickdrozd on 2025-01-17 16:09_

Assignment with `if/else` gets flagged with `SIM108`, suggesting ternary operator. Good. But if the variable is annotated, not flagged:

```python
from random import randint

if randint(0, 3):
    x: int = 4  # not flagged
else:
    x = 5
```

Could be `if` var annotated or `else` var, not flagged. Also not flagged if both vars have annotation, even if annotation is the same.

---

_Label `rule` added by @MichaReiser on 2025-01-17 16:34_

---

_Comment by @MichaReiser on 2025-01-17 16:34_

That makes sense to me but is slightly more complicated because that only works if both branches infer to the same type, unless the fix removes the type annotation.

---

_Comment by @nickdrozd on 2025-01-17 17:12_

If the variable is declared with different types in the branches, Mypy flags it as a redefinition:

```python
if randint(0, 3):
    x: int = 4
else:
    x: str = 'q'  # mypy flags as redefined
```

(It's not even clear whether there is one variable `x` defined in two places or two separate variables each called `x`. Yet another reason not to define a variable in branches.)

In this case it would be fine to add a new annotation that is the union of types in the branches:

```python
x: int | str = 4 if randint(0, 3) else 'q'
```

---

_Comment by @MichaReiser on 2025-01-17 17:46_

The problem I see is more about:

```
if randint(0, 3):
    x: int = 4
else:
    x = 'q'  # mypy flags as redefined
```

Mypy might also flag this as redefinition but I'd have to check if this is true for every type checker (including red knot). But I could still see us flagging this but we may not want to provide a fix for it.

---
