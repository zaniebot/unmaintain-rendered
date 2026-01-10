```yaml
number: 1435
title: "Enum literals are no longer considered subtypes of the enum-class instance unioned with `AlwaysTruthy`"
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
  - enums
assignees: []
created_at: 2025-10-24T12:11:58Z
updated_at: 2025-10-24T13:34:17Z
url: https://github.com/astral-sh/ty/issues/1435
synced_at: 2026-01-10T02:06:25Z
```

# Enum literals are no longer considered subtypes of the enum-class instance unioned with `AlwaysTruthy`

---

_Issue opened by @github-actions on 2025-10-24 12:11_

Run listed here: https://github.com/astral-sh/ty/actions/runs/18779122084

---

_Label `bug` added by @github-actions[bot] on 2025-10-24 12:11_

---

_Label `type properties` added by @github-actions[bot] on 2025-10-24 12:12_

---

_Label `enums` added by @AlexWaygood on 2025-10-24 12:27_

---

_Comment by @AlexWaygood on 2025-10-24 12:27_

Minimal repro:

```py
from ty_extensions import AlwaysTruthy, is_subtype_of
from typing import Literal
import enum

class X(enum.Enum):
    A = 1

reveal_type(is_subtype_of(Literal[X.A], X))  # Literal[True]
reveal_type(is_subtype_of(Literal[X.A], X | AlwaysTruthy))  # Literal[False]!
```

Almost certainly the immediate cause was https://github.com/astral-sh/ruff/pull/21049, but it looks like that probably just exposed a pre-existing bug of some kind here.

---

_Renamed from "Daily property test run failed on Fri Oct 24 2025" to "Enum literals are no longer considered subtypes of the enum-class instance unioned with `AlwaysTruthy`" by @AlexWaygood on 2025-10-24 12:28_

---

_Comment by @AlexWaygood on 2025-10-24 12:31_

Okay, the issue is that we now correctly simplify the union `X | AlwaysTruthy` to `AlwaysTruthy`: `X` is a `@final` class (because it is an enum class with members that doesn't override `__bool__` or `__len__`), so all instances of `X` will always be truthy, so `X` is a subtype of `AlwaysTruthy`. But although we now recognise `X` as a subtype of `AlwaysTruthy`, we do not yet recognise `Literal[X.A]` as a subtype of `AlwaysTruthy`, so this simplification of the union breaks our understanding of the subtype relationship.

Should be easy to fix.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-24 12:31_

---

_Closed by @AlexWaygood on 2025-10-24 13:34_

---
