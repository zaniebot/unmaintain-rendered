```yaml
number: 2505
title: Go-to for call expression should jump to the matching overload if possible
type: issue
state: open
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2026-01-15T06:22:37Z
updated_at: 2026-01-15T06:22:37Z
url: https://github.com/astral-sh/ty/issues/2505
synced_at: 2026-01-15T06:49:13Z
```

# Go-to for call expression should jump to the matching overload if possible

---

_@dhruvmanila_

For example:

```py
from typing import overload

class A: ...
class B: ...

@overload
def foo(x: A) -> A: ...
@overload
def foo(x: B) -> B: ...
def foo(x): ...

def _(a: A, b: B, ab: A | B):
    foo(a)  # go-to should jump to overload 1
    foo(b)  # go-to should jump to overload 2
    foo(ab)  # go-to should return both overloads
```

---

_Label `server` added by @dhruvmanila on 2026-01-15 06:22_

---
