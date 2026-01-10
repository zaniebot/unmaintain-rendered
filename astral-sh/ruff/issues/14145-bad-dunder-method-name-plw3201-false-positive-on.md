```yaml
number: 14145
title: "`bad-dunder-method-name` (`PLW3201`) - false positive on `__replace__` dunder"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - python313
assignees: []
created_at: 2024-11-07T02:13:28Z
updated_at: 2024-11-07T06:09:02Z
url: https://github.com/astral-sh/ruff/issues/14145
synced_at: 2026-01-10T11:09:55Z
```

# `bad-dunder-method-name` (`PLW3201`) - false positive on `__replace__` dunder

---

_Issue opened by @DetachHead on 2024-11-07 02:13_

```py
from copy import replace


class Foo:
    def __replace__(self, *args: object, **kwargs: object): # PLW3201: Dunder method `__replace__` has no special meaning in Python 3
        print("hi")


replace(Foo()) # calls `Foo.__replace__`
```
[playground](https://play.ruff.rs/e6bae1cb-f4c9-4929-97e3-cc74a45eb0c2)

---

_Label `bug` added by @dhruvmanila on 2024-11-07 04:05_

---

_Label `python313` added by @dhruvmanila on 2024-11-07 04:05_

---

_Closed by @dhruvmanila on 2024-11-07 06:09_

---

_Closed by @dhruvmanila on 2024-11-07 06:09_

---
