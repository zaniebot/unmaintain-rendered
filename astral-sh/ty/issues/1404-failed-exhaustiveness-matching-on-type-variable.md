```yaml
number: 1404
title: Failed exhaustiveness matching on type variable
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - narrowing
  - type properties
assignees: []
created_at: 2025-10-20T12:55:13Z
updated_at: 2025-10-30T14:38:58Z
url: https://github.com/astral-sh/ty/issues/1404
synced_at: 2026-01-10T02:06:25Z
```

# Failed exhaustiveness matching on type variable

---

_Issue opened by @sharkdp on 2025-10-20 12:55_

Consider this example where we currently emit a false positive.

```py
from typing import Self
from enum import Enum

class Answer(Enum):
    YES = "yes"
    NO = "no"

    def is_yes(self: Self) -> bool:    # ty: Function can implicitly return `None`, which is not assignable to return type `bool`
        match self:
            case Answer.YES:
                return True
            case Answer.NO:
                return False
```

When the annotation is changed to `self: Answer`, everything works as expected. But we do not recognize that `Self@is_yes` with an upper bound of `Answer` can be narrowed down.

Similar problems certainly exist for other final types and other narrowing patterns.

It seems like we can generally simplify a non-inferable typevar `T` with a `final` upper bound `F` to that upper bound `F`? Or let `T` share more properties of `F` (like whether or not we consider it to be `final`).

---

_Label `bug` added by @sharkdp on 2025-10-20 12:55_

---

_Label `narrowing` added by @sharkdp on 2025-10-20 12:55_

---

_Label `type properties` added by @sharkdp on 2025-10-20 12:55_

---

_Closed by @sharkdp on 2025-10-30 14:38_

---
