```yaml
number: 1452
title: "Classmethods and staticmethods should not be considered instances of `types.FunctionType`"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-10-29T14:49:10Z
updated_at: 2025-10-30T17:14:01Z
url: https://github.com/astral-sh/ty/issues/1452
synced_at: 2026-01-12T15:54:25Z
```

# Classmethods and staticmethods should not be considered instances of `types.FunctionType`

---

_@AlexWaygood_

### Summary

Classmethods and staticmethods should not be considered instances of `types.FunctionType`, but they currently are by ty. This means that the following assertion fails:

```py
from ty_extensions import static_assert, is_subtype_of, TypeOf
from typing import reveal_type
import types
import inspect

class F:
    @classmethod
    def f(): ...

type Fff = TypeOf[inspect.getattr_static(F, 'f')]
static_assert(not is_subtype_of(Fff, types.FunctionType))
```

It also means that we incorrectly believe that classmethods/staticmethods have attributes such as `__kwdefaults__` that actually only exist on functions, and it means that we emit false-positive diagnostics if you access an attribute such as `__func__` that only exists on a classmethod/staticmethod, but not on a function:

```py
inspect.getattr_static(F, 'f').__kwdefaults__  # no error here (false negative)
inspect.getattr_static(F, 'f').__func__  # error here (false positive)
```

See https://play.ty.dev/5daebf80-6f88-4bde-83b4-796e3a965665

---

_Label `bug` added by @AlexWaygood on 2025-10-29 14:49_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-10-29 14:49_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-29 14:59_

---

_Comment by @AlexWaygood on 2025-10-30 17:13_

See https://github.com/astral-sh/ruff/pull/21145. It seems a good fix for this might be somewhat involved; we should probably have separate `Type` variants for classmethods and staticmethods.

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-10-30 17:14_

---
