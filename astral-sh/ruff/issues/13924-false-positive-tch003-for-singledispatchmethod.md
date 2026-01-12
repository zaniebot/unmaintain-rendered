```yaml
number: 13924
title: " False positive TCH003 for singledispatchmethod"
type: issue
state: closed
author: last-partizan
labels:
  - bug
assignees: []
created_at: 2024-10-25T11:41:12Z
updated_at: 2024-10-28T01:02:47Z
url: https://github.com/astral-sh/ruff/issues/13924
synced_at: 2026-01-12T15:54:53Z
```

#  False positive TCH003 for singledispatchmethod

---

_@last-partizan_

This is almost the same as #11520  , but for `@singledispatchmethod`

```py
from __future__ import annotations

from collections.abc import MutableMapping
from functools import singledispatchmethod
from typing import Any


class Foo:
    @singledispatchmethod
    def foo(self, x: Any) -> int:
        raise NotImplementedError

    @foo.register
    def _(self, x: MutableMapping) -> int:
        return 0
```

```
> ruff check --select TCH app/test.py

app/test.py:3:29: TCH003 Move standard library import `collections.abc.MutableMapping` into a type-checking block
  |
1 | from __future__ import annotations
2 |
3 | from collections.abc import MutableMapping
  |                             ^^^^^^^^^^^^^^ TCH003
4 | from functools import singledispatchmethod
5 | from typing import Any
  |
  = help: Move into type-checking block

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

```
> python --version
Python 3.12.7
> ruff --version
ruff 0.7.1
```

---

_Comment by @MichaReiser on 2024-10-26 08:20_

I don't think I understand this issue. Could you explain me why `MutableMapping` can't be moved into a type checking block?

---

_Comment by @last-partizan on 2024-10-26 15:45_

Because `@simgledispatchmethod` is using runtime types, just like normal `@singledispatch` from #11520

Example:

```py
> python /tmp/test.py
Traceback (most recent call last):
  File "/tmp/test.py", line 10, in <module>
    class Foo:
  File "/tmp/test.py", line 15, in Foo
    @foo.register
     ^^^^^^^^^^^^
  File "/usr/lib/python3.12/functools.py", line 939, in register
    return self.dispatcher.register(cls, func=method)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
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
NameError: name 'MutableMapping' is not defined
```

---

_Label `bug` added by @MichaReiser on 2024-10-26 16:02_

---

_Closed by @charliermarsh on 2024-10-28 01:02_

---

_Closed by @charliermarsh on 2024-10-28 01:02_

---
