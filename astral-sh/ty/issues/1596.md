```yaml
number: 1596
title: "Argument to class `dtype` is incorrect - only accepting Generic and no TypeVar"
type: issue
state: closed
author: loicdiridollou
labels: []
assignees: []
created_at: 2025-11-19T22:38:06Z
updated_at: 2025-11-28T19:44:23Z
url: https://github.com/astral-sh/ty/issues/1596
synced_at: 2026-01-10T01:58:59Z
```

# Argument to class `dtype` is incorrect - only accepting Generic and no TypeVar

---

_Issue opened by @loicdiridollou on 2025-11-19 22:38_

### Summary

Hi team!

We are running into a change of behavior in the latest release of `ty` with the `pandas-stubs` repo.
We have some generics that are specified by `TypeVar` yet when switching to the 27a release, this suddenly start breaking.
I wanted to add a playground to reproduce but numpy is not in the environment so I have added the code below:

```python
from typing import (
    Any,
    TypeAlias,
    TypeVar,
)

import numpy as np

GenericT = TypeVar("GenericT", bound=np.generic, default=Any)
Np1darray: TypeAlias = np.ndarray[tuple[int], np.dtype[GenericT]]
```

The error that I am getting is:
```
error[invalid-argument-type]: Argument to class `dtype` is incorrect
  --> test.py:10:47
   |
 9 | GenericT = TypeVar("GenericT", bound=np.generic, default=Any)
10 | Np1darray: TypeAlias = np.ndarray[tuple[int], np.dtype[GenericT]]
   |                                               ^^^^^^^^^^^^^^^^^^ Expected `generic[Any]`, found `typing.TypeVar`
   |
info: rule `invalid-argument-type` is enabled by default
```

We have that `np.dtype[GenericT]` call in a couple of places and `ty` is not raising in all cases which was a surprising behavior.
I am not sure what to make up with this change in version 27.
Let us know if you have any insights

Thanks!

### Version

ty 0.0.1-alpha.27 (26d7b6864 2025-11-18)

---

_Comment by @loicdiridollou on 2025-11-19 22:41_

We have run `ty` on the whole repo and this is an example of a failing run with the errors (the code above is just the smallest I could write to reproduce the problem): https://github.com/pandas-dev/pandas-stubs/actions/runs/19518732434/job/55877104496?pr=1495

---

_Comment by @carljm on 2025-11-19 23:40_

Thank you for the report! We are actively working on improving type alias support, and some things are temporarily regressing in the process.

I created a minimized example case for what's happening here: https://play.ty.dev/261399e7-15ac-4ad9-a487-b613f1b15c47

I think this case should be resolved in @sharkdp 's current work on generic type aliases.

---

_Added to milestone `Beta` by @carljm on 2025-11-19 23:41_

---

_Comment by @loicdiridollou on 2025-11-20 01:46_

Thanks @carljm for the example! Always happy to help when those issues arise, we are big consumers of `ty` on the pandas-stubs repo so we will see these edge cases!

---

_Assigned to @sharkdp by @sharkdp on 2025-11-24 09:03_

---

_Closed by @sharkdp on 2025-11-28 19:38_

---

_Comment by @sharkdp on 2025-11-28 19:44_

Confirming that both the OP example and the minimal example are now working.

---
