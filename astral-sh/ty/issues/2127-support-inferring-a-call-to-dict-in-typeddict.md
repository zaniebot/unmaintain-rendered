---
number: 2127
title: "Support inferring a call to `dict(...)` in TypedDict context"
type: issue
state: closed
author: carljm
labels:
  - bidirectional inference
  - typeddict
assignees: []
created_at: 2025-12-20T01:11:14Z
updated_at: 2025-12-22T15:24:15Z
url: https://github.com/astral-sh/ty/issues/2127
synced_at: 2026-01-10T01:52:52Z
---

# Support inferring a call to `dict(...)` in TypedDict context

---

_Issue opened by @carljm on 2025-12-20 01:11_

Originally reported at https://www.reddit.com/r/Python/comments/1podix2/comment/nuhhob3/

```py
from typing import TypedDict

class TD(TypedDict):
    x: int

t: TD = dict(x=1) # error: "Object of type `dict[str, int]` is not assignable to `TD`"
```

https://play.ty.dev/63687f42-6944-4a20-b04f-94a38b171e73

---

_Added to milestone `Stable` by @carljm on 2025-12-20 01:11_

---

_Label `bidirectional inference` added by @carljm on 2025-12-20 01:11_

---

_Label `typeddict` added by @AlexWaygood on 2025-12-20 09:37_

---

_Comment by @Hugo-Polloli on 2025-12-20 11:22_

I'm going to try this over the weekend if that's ok :)

---

_Closed by @carljm on 2025-12-20 15:59_

---

_Comment by @mflova on 2025-12-20 22:05_

Despite the recommenation of not using mutable data structures as default input arguments, the following code snippet would not be working despite the fix. Is this expected?

```py
def func(foo: TD = dict(x=1)) -> None:
    ...
```

(or even with the {"x": 1})

---

_Comment by @AlexWaygood on 2025-12-22 15:24_

Thanks @mflova, I opened https://github.com/astral-sh/ty/issues/2161

---
