```yaml
number: 10955
title: "UP035: doesn't detect errors when it should"
type: issue
state: closed
author: morotti
labels:
  - rule
assignees: []
created_at: 2024-04-15T14:22:19Z
updated_at: 2025-11-10T12:25:37Z
url: https://github.com/astral-sh/ruff/issues/10955
synced_at: 2026-01-10T11:09:53Z
```

# UP035: doesn't detect errors when it should

---

_Issue opened by @morotti on 2024-04-15 14:22_

Hello, 

I am trying to use ruff to automatically fix issues with collections import that have moved to collections.abc in python 3.10+

I've found that ruff can only detect and fix the import about half the time.

```
from collections import Iterable

def myfunction(arg):
    if isinstance(arg, Iterable):
        return True
    return False

myfunction("string")
```


```
import collections

def myfunction(arg):
    if isinstance(arg, collections.Iterable):
        return True
    return False

myfunction("string")
```

Try with `ruff --select ALL example.py`

In the top example, ruff finds the error and can fix it automatically. 

    example.py:1:1: UP035 [*] Import from `collections.abc` instead: `Iterable`

In the bottom example, ruff doesn't find any error when it should have.
The code fails at runtime.

```
Traceback (most recent call last):
  File "/home/username/example.py", line 8, in <module>
    myfunction("string")
  File "/home/username/example.py", line 4, in myfunction
    if isinstance(arg, collections.Iterable):
                       ^^^^^^^^^^^^^^^^^^^^
AttributeError: module 'collections' has no attribute 'Iterable'
```

Running on latest ruff release, 0.3.7, on python 3.11



---

_Comment by @charliermarsh on 2024-04-15 14:39_

That rule flags imports, not usages, and so it doesn't flag attribute accesses on the module, just `from collections import ...` cases.

---

_Label `rule` added by @charliermarsh on 2024-05-03 00:33_

---

_Comment by @jerr0328 on 2024-07-11 14:55_

It looks like the same as #3936 and #7526

---

_Closed by @MichaReiser on 2025-11-10 12:25_

---
