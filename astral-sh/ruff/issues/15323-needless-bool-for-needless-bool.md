```yaml
number: 15323
title: needless bool for needless-bool
type: issue
state: open
author: nickdrozd
labels:
  - bug
  - fixes
  - type-inference
assignees: []
created_at: 2025-01-07T18:16:10Z
updated_at: 2025-01-07T19:03:08Z
url: https://github.com/astral-sh/ruff/issues/15323
synced_at: 2026-01-12T15:54:54Z
```

# needless bool for needless-bool

---

_@nickdrozd_

Function to check random numbers:

```python
from random import randint

def check_zero(end: int) -> bool:
    """Check if two random numbers from 0 to END are both 0."""
    if randint(0, end) == 0 and randint(0, end) == 0:
        return True
    else:
        return False
```

Raises warning `SIM103 Return the condition directly`. Good. Fix with `--unsafe-fixes --fix`:

```python
from random import randint

def check_zero(end: int) -> bool:
    """Check if two random numbers from 0 to END are both 0."""
    return bool(randint(0, end) == 0 and randint(0, end) == 0)
```

Multi-return `if`/`else` expression replaced with boolean expression. Good. But also wrapped with `bool` cast. Bad! `bool` cast of a boolean expression is unnecessary.

---

_Label `bug` added by @dylwil3 on 2025-01-07 18:22_

---

_Label `fixes` added by @dylwil3 on 2025-01-07 18:22_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-01-07 18:26_

---

_Comment by @AlexWaygood on 2025-01-07 18:37_

> Multi-return `if`/`else` expression replaced with boolean expression. Good. But also wrapped with `bool` cast. Bad! `bool` cast of a boolean expression is unnecessary.

We need to be careful here! If we can say for sure that both the left-hand side and the right-hand side is of type `bool`, then yes, we can safely remove the `bool()` call wrapping the expression. Otherwise we cannot, or a type checker might complain that the function's annotation has `bool` for the return type when actually it returns something else:

```pycon
>>> 5 and 42
42
```

Perhaps you might say that we can say for sure that equality comparisons will always evaluate to a `bool`, but... sadly that's also not true!

```pycon
>>> class Foo:
...     def __eq__(self, other):
...         return 42
...         
>>> Foo() == Foo() and Foo() == Foo()
42
```

---

_Comment by @dylwil3 on 2025-01-07 18:44_

Excellent point! We can at least add in the cases where ruff's semantic model can infer the type as bool (which will be fairly limited, and I think will not include the example in this issue due to the function call).

---

_Label `type-inference` added by @dylwil3 on 2025-01-07 18:45_

---

_Comment by @dylwil3 on 2025-01-07 18:56_

In fact, [here is a case](https://play.ruff.rs/0ec08839-1400-443d-b7ac-0eadfe041df4) where the autofix is wrong because it _doesn't_ cast to bool:

```python
import numpy as np 

def f(x:np.ndarray,y:np.ndarray):
    if x<y:
        return True
    else:
        return False
```

gets fixed to 

```python
import numpy as np 

def f(x:np.ndarray,y:np.ndarray):
    return x < y
```

Arguably comparison operators are _more_ likely to produce false positives than boolean operators like `and`/`or` (since numpy does not override the latter - it only overrides the bitwise versions).

In any event the fix is marked as unsafe... so there's a question of whether we should be restricting the fix behavior further, extending it (as in the OP), or some mixture (eg avoiding comparison operators but allowing boolean operators).

I'm gonna push a few different versions and see what the ecosystem check reveals and then maybe we can discuss further. (Assuming the ecosystem check will tell me anything... I remember we made a change recently around what it does with fix behavior...)

---

_Comment by @AlexWaygood on 2025-01-07 18:59_

Right. We could also take whether or not the function is explicitly annotated into consideration, possibly? In untyped code, there's maybe not much of a difference between returning something truthy and something that is the literal `True` constant; the `bool()` cast maybe just adds overhead there. But if the function is annotated as returning `bool`, they're probably using a type checker, so we might break their CI by removing the `bool()` cast... anyway, looking forward to seeing the results of your experiments :D

---

_Comment by @nickdrozd on 2025-01-07 19:03_

Python's boolean handling never ceases to surprise! I see what you mean about being careful. Feel free to close this.

---
