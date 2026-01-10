```yaml
number: 2218
title: SIM103 changes behavior
type: issue
state: closed
author: spaceone
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-01-26T20:09:51Z
updated_at: 2023-01-31T21:31:27Z
url: https://github.com/astral-sh/ruff/issues/2218
synced_at: 2026-01-10T11:09:45Z
```

# SIM103 changes behavior

---

_Issue opened by @spaceone on 2023-01-26 20:09_

```
$ cat foo.py
def foo(bar: int) -> bool:
        if bar:
                return True
        else:
                return False

assert foo(1) is True
$ python3 foo.py
$ ruff --isolated --select SIM103 --fix foo.py                                                                                                                              
Found 1 error (1 fixed, 0 remaining).
$ cat foo.py                                                                                                                                                                
def foo(bar: int) -> bool:
        return bar

assert foo(1) is True
$ python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 4, in <module>
    assert foo(1) is True
AssertionError
```

maybe `SIM103` could try to detect if the input is a boolean and otherwise wrap it in a `bool()` call?

---

_Comment by @spaceone on 2023-01-26 20:47_

SIM210 is similar and adds `bool()` wrapping.

---

_Label `autofix` added by @charliermarsh on 2023-01-26 20:48_

---

_Comment by @charliermarsh on 2023-01-26 21:11_

This is probably difficult to avoid right now.

---

_Comment by @charliermarsh on 2023-01-26 21:12_

I guess we could _always_ wrap in `bool()`? It'd be extra strange to write this code if `bar` were a bool.

---

_Label `bug` added by @charliermarsh on 2023-01-26 21:12_

---

_Comment by @andersk on 2023-01-26 22:43_

Seems better to be timid than wrong; it’s easier for the user to observe that an extra `bool()` can be removed than that a missing `bool()` should be added.

We could have a heuristic that checks if the test expression is a comparator that’s known to probably return boolean? (There are exceptions like `numpy` arrays and `z3` constraints, but such exceptions probably aren’t being used as an `if` test.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-26 23:02_

---

_Closed by @charliermarsh on 2023-01-26 23:22_

---

_Comment by @spaceone on 2023-01-31 21:31_

this has been improved in 1dd9ccf7f61487781844d8efb38a1b869ec263f9:

`bool()` wrapping is not done for comparisions anymore.

---
