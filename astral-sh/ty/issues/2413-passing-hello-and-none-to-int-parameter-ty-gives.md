```yaml
number: 2413
title: "Passing `'Hello'` and `None` to `int` parameter, ty gives no error"
type: issue
state: closed
author: hyperkai
labels: []
assignees: []
created_at: 2026-01-09T11:28:56Z
updated_at: 2026-01-09T20:02:32Z
url: https://github.com/astral-sh/ty/issues/2413
synced_at: 2026-01-12T15:54:26Z
```

# Passing `'Hello'` and `None` to `int` parameter, ty gives no error

---

_@hyperkai_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

Passing `'Hello'` and `None` to `int` parameter, ty gives no error as shown below:

```python
from collections.abc import Callable

v: Callable[[int], int] = lambda x: x

v('Hello') # No error
v(None)    # No error
```

[Playground](https://play.ty.dev/082c9c88-b642-4503-a92f-15da42d3e223)

### Version

_No response_

---

_Closed by @AlexWaygood on 2026-01-09 11:52_

---

_Comment by @MichaReiser on 2026-01-09 12:40_

and/or https://github.com/astral-sh/ty/issues/136

---

_Comment by @carljm on 2026-01-09 20:02_

To clarify the connection to #136, this gives the expected errors on the last two lines:

```py
from collections.abc import Callable

def _(v: Callable[[int], int]):
    v('Hello')
    v(None)
```

In the OP example, we infer the RHS as `Callable[[Unknown], Unknown]`, which is assignable to `Callable[[int], int]`, but we currently prefer the inferred type over the declared type. We plan to change this.

---
