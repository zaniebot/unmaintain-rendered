```yaml
number: 1659
title: "Type information not propagated through `match` case statement"
type: issue
state: closed
author: save-buffer
labels:
  - narrowing
assignees: []
created_at: 2025-11-27T19:46:51Z
updated_at: 2025-11-27T20:03:21Z
url: https://github.com/astral-sh/ty/issues/1659
synced_at: 2026-01-12T15:54:25Z
```

# Type information not propagated through `match` case statement

---

_@save-buffer_

### Summary

Hi, I noticed that the value of an assignment in a match expression doesn't seem to get propagated into type checking, but I was hoping it would be since it seems relatively straightforward to deduce all the possible values of the assigned variable. 

Version:
```
% ty --version
ty 0.0.1-alpha.28 (8c342496a 2025-11-25)
```

The "share" button on the playground doesn't seem to be working for me (the link is not appearing in my clipboard!) so here's my repro pasted here. Thanks!

```py
from typing import Literal


T = Literal["+", "-", "*", "/"]


def foo(x : str) -> T | None:
    match fst := x[0]:
        case "+" | "-":
            return fst
        case _:
            return None
```

### Version

ty 0.0.1-alpha.28 (8c342496a 2025-11-25)

---

_Comment by @Gankra on 2025-11-27 19:54_

Note that this is not unique to `match`, we similarly don't refine `str` to `Literal` if we see a check in an `if`, the following complains that `return fst` does not satisfy `T | None`:

```py
from typing import Literal

T = Literal["+", "-", "*", "/"]

def foo(x : str) -> T | None:
    fst = x[0]
    if fst == "+":
        return fst
    return None
```

Related to:

* https://github.com/astral-sh/ty/issues/887

---

_Comment by @AlexWaygood on 2025-11-27 20:01_

Thanks for the report! Python dispatches on `match` arms based on equality if the `match` pattern is a literal string, so it's actually unsafe to do this kind of type narrowing. Please see https://github.com/astral-sh/ty/issues/1566

---

_Closed by @AlexWaygood on 2025-11-27 20:01_

---

_Label `narrowing` added by @AlexWaygood on 2025-11-27 20:01_

---

_Comment by @save-buffer on 2025-11-27 20:03_

Got it, thank you!

---
