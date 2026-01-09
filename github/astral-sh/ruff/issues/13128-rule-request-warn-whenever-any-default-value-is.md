---
number: 13128
title: "Rule request: Warn whenever any default value is provided in __post_init__"
type: issue
state: closed
author: NeilGirdhar
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-08-27T21:10:25Z
updated_at: 2024-09-02T12:10:56Z
url: https://github.com/astral-sh/ruff/issues/13128
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule request: Warn whenever any default value is provided in __post_init__

---

_Issue opened by @NeilGirdhar on 2024-08-27 21:10_

Default values should not be given in the arguments of `__post_init__`.  They should only be provided as default values for class-level _init-only_ member variables.

```python
from dataclasses import InitVar, dataclass


@dataclass
class C:
    x: InitVar[int] = 1

    def __post_init__(self, x: int = 4) -> None:
        print(x)  # noqa: T201


c = C()  # Prints 1!  4 is ignored.
```
The above text could be used for the Ruff rule warning message.

---

_Comment by @dhruvmanila on 2024-08-28 05:36_

Interesting, thanks for raising this rule request. This seems like a reasonable rule to implement, most likely in a `RUF` category. What do you think? @AlexWaygood 

---

_Label `rule` added by @dhruvmanila on 2024-08-28 05:36_

---

_Comment by @dhruvmanila on 2024-08-28 05:37_

Although, I couldn't really find any usages of such pattern: https://grep.app/search?q=def%20__post_init__%5C(self,%5B%5E%3D%5Cn%5D+%3D&regexp=true&filter%5Blang%5D%5B0%5D=Python

---

_Comment by @NeilGirdhar on 2024-08-28 08:46_

> Although, I couldn't really find any usages of such pattern

Fair enough.  It came up when I was working on my own code.  I had accidentally added some parameter defaults, and then I though to check what happens.

That said, if this pattern is that rare, then I agree that there are more important issues to be added to Ruff and we can close this.

---

_Comment by @AlexWaygood on 2024-08-28 09:03_

I agree this is a reasonable thing to have a rule for. There's no chance of false positives, and I think I remember hitting this footgun myself at one point in a previous project. The `RUF` category sounds good to me.

---

_Label `accepted` added by @AlexWaygood on 2024-08-28 09:03_

---

_Label `help wanted` added by @AlexWaygood on 2024-08-28 22:04_

---

_Referenced in [astral-sh/ruff#13192](../../astral-sh/ruff/pulls/13192.md) on 2024-09-01 11:34_

---

_Closed by @AlexWaygood on 2024-09-02 12:10_

---
