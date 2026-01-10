```yaml
number: 13955
title: False positive TCH003 fix for singledispatch functions with type annotations (redux)
type: issue
state: closed
author: eli-schwartz
labels:
  - bug
assignees: []
created_at: 2024-10-28T02:58:50Z
updated_at: 2024-10-29T00:33:29Z
url: https://github.com/astral-sh/ruff/issues/13955
synced_at: 2026-01-10T11:09:55Z
```

# False positive TCH003 fix for singledispatch functions with type annotations (redux)

---

_Issue opened by @eli-schwartz on 2024-10-28 02:58_

#11520 again

Consider the same code block from there again:
```python
from __future__ import annotations

from functools import singledispatch
from typing import Any

from collections.abc import MutableMapping, Set


@singledispatch
def foo(x: Any, y: MutableMapping) -> Set:
    raise NotImplementedError


@foo.register
def _(x: int, y: MutableMapping) -> Set:
    return set([x])
```

but tweaked a bit. Now the function also returns an imported type too.

```
$ ruff check bug.py
bug.py:6:45: TCH003 Move standard library import `collections.abc.Set` into a type-checking block
  |
4 | from typing import Any, TYPE_CHECKING
5 | 
6 | from collections.abc import MutableMapping, Set
  |                                             ^^^ TCH003
7 | 
8 | @singledispatch
  |
  = help: Move into type-checking block

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
$ ruff check --fix --unsafe-fixes bug.py
Found 1 error (1 fixed, 0 remaining).
$ python bug.py
Traceback (most recent call last):
  File "/tmp/bug.py", line 16, in <module>
    @foo.register
     ^^^^^^^^^^^^
  File "/usr/lib/python3.12/functools.py", line 877, in register
    argname, cls = next(iter(get_type_hints(func).items()))
                             ^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/typing.py", line 2310, in get_type_hints
    hints[name] = _eval_type(value, globalns, localns, type_params)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/typing.py", line 415, in _eval_type
    return t._evaluate(globalns, localns, type_params, recursive_guard=recursive_guard)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/typing.py", line 947, in _evaluate
    eval(self.__forward_code__, globalns, localns),
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<string>", line 1, in <module>
NameError: name 'Set' is not defined. Did you mean: 'set'?
```

The underlying issue is that in order for singledispatch to learn about the annotations of the decorated function, it has to run an introspection that doesn't distinguish between what singledispatch needs and what singledispatch doesn't need.

This was fixed in #11523 by "treating all singledispatch arguments as runtime-required" -- but it's not about the arguments, it's about the function signature. Return values, too, are part of the function signature.

---

_Comment by @dhruvmanila on 2024-10-28 04:33_

Can you specify what Ruff version do you have? And also provide the Ruff config if any is being used in your command invocation? I'm unable to reproduce this: https://play.ruff.rs/6e46194f-daf9-4a61-98ce-c3d1bccf019e

---

_Label `needs-mre` added by @dhruvmanila on 2024-10-28 04:33_

---

_Comment by @eli-schwartz on 2024-10-28 04:42_

Note that the linked issue also included a ruff config necessary to reproduce the previous bug report. That is still necessary here -- all I did was extend/enhance the `bug.py` from that issue to make it reproduce again.

I'm using ruff 0.7.1

---

_Comment by @eli-schwartz on 2024-10-28 04:47_

I updated your playground case: https://play.ruff.rs/ab7beaa2-920d-4ef4-905e-1bbfd97d8719

---

_Comment by @dhruvmanila on 2024-10-28 04:52_

Got it, thanks!

---

_Label `needs-mre` removed by @dhruvmanila on 2024-10-28 04:52_

---

_Label `bug` added by @dhruvmanila on 2024-10-28 04:52_

---

_Closed by @charliermarsh on 2024-10-29 00:33_

---
