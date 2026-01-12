```yaml
number: 2011
title: Ty and None values unclear when using object initialisation logic
type: issue
state: closed
author: laurensnaturalis
labels:
  - question
assignees: []
created_at: 2025-12-17T13:52:52Z
updated_at: 2025-12-17T14:21:30Z
url: https://github.com/astral-sh/ty/issues/2011
synced_at: 2026-01-12T15:54:26Z
```

# Ty and None values unclear when using object initialisation logic

---

_@laurensnaturalis_

### Question

```py

from numpy._typing import NDArray
import numpy as np


class SomeClass(object):
    features: NDArray[np.float64] | None = None

    def __init__(self):
        self.loaded = False

    def load_data(self):
        self.features = np.zeros((1,))
        self.loaded = True

a = SomeClass()
if not a.loaded:
    a.load_data()
else:
    if len(a.features) > 0:
        # do something with a.features
        pass

```

gives for the line `if len(a.features) > 0:`

```
Argument to function `len` is incorrect: Expected `Sized`, found `ndarray[Any, dtype[floating[_64Bit]]] | None` ty(invalid-argument-type)
```

The above pattern seems to be fairly common. How can I fix this without resorting to either `ty: ignore` or default values other than None?

I tried

(a) removing the `| None' for features which gives as error 

```
Object of type `None` is not assignable to `ndarray[Any, dtype[floating[_64Bit]]]` ty(invalid-assignment)
```

(b) Using a property for `features` which gives as error  

```
return type does not match returned value: expected ndarray[Any, dtype[floating[_64Bit]]]', found `ndarray[Any, dtype[floating[_64Bit]]] | None ty(invalid-return-type)
```

(c) `features: NDArray[np.float64] = np.zeros(0)` works, but is ugly and it's not possible to have sensible default values for different kinds of objects
### Version

ty 0.02

---

_Label `question` added by @laurensnaturalis on 2025-12-17 13:52_

---

_Comment by @sharkdp on 2025-12-17 14:15_

Thank you for your interest in ty!

You filed two very similar issues earlier today (https://github.com/astral-sh/ty/issues/2002, https://github.com/astral-sh/ty/issues/2005). All of these questions seem to originate from a misunderstanding of union types. When an object has type `A | B`, it means the object could *either* be of type `A` *or* of type `B`. You can therefore not expect it to have all behaviors of type `A`.

More concretely for your cases, if an object has type `Array | None`, you can not treat it like an `Array`. You first need to exclude the possibility that it might be `None` first.

It might be useful to read https://mypy.readthedocs.io/en/stable/kinds_of_types.html#alternative-union-syntax and https://mypy.readthedocs.io/en/stable/kinds_of_types.html#optional-types-and-the-none-type before coming back with more questions. Also, if you have general questions about Python typing, the [Python discord](https://discord.com/invite/python) or the Typing section on [discuss.python.org](https://discuss.python.org/c/typing/32) might be more suited. If you think you've found a bug in ty, you can certainly report it here, but please consider checking with another type-checker before doing so.

---

_Closed by @laurensnaturalis on 2025-12-17 14:21_

---
